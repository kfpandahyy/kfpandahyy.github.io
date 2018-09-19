---
Date: August 18, 2018
title: MySQL Gtid_state类
layout: post
comments: true
language: chinese
category: [mysql]
keywords: mysql
description: 
---
介绍Gtid_state类，下面是以5.7.17版本中Gtid_state类为例进行介绍。
<!-- more -->

## Gtid_state类的说明

```
/**
  Represents the state of the group log: the set of logged groups, the
  set of lost groups, the set of owned groups, the owner of each owned
  group, and a Mutex_cond_array that protects updates to groups of
  each SIDNO.

  Locking:

  This data structure has a read-write lock that protects the number
  of SIDNOs, and a Mutex_cond_array that contains one mutex per SIDNO.
  The rwlock is always the global_sid_lock.

  Access methods generally assert that the caller already holds the
  appropriate lock:

   - before accessing any global data, hold at least the rdlock.

   - before accessing a specific SIDNO in a Gtid_set or Owned_gtids
     (e.g., calling Gtid_set::_add_gtid(Gtid)), hold either the rdlock
     and the SIDNO's mutex lock; or the wrlock.  If you need to hold
     multiple mutexes, they must be acquired in order of increasing
     SIDNO.

   - before starting an operation that needs to access all SIDs
     (e.g. Gtid_set::to_string()), hold the wrlock.

  The access type (read/write) does not matter; the write lock only
  implies that the entire data structure is locked whereas the read
  lock implies that everything except SID-specific data is locked.
*/
```

## Gtid_state类的成员变量
```
class Gtid_state
{

  ...

private:
  /// Read-write lock that protects updates to the number of SIDs.
  mutable Checkable_rwlock *sid_lock;
  /// The Sid_map used by this Gtid_state.
  mutable Sid_map *sid_map;
  /// Contains one mutex/cond pair for every SIDNO.
  Mutex_cond_array sid_locks;
  /**
    The set of GTIDs that existed in some previously purged binary log.
    This is always a subset of executed_gtids.
  */
  Gtid_set lost_gtids;
  /*
    The set of GTIDs that has been executed and
    stored into gtid_executed table.
  */
  Gtid_set executed_gtids;
  /*
    The set of GTIDs that exists only in gtid_executed table, not in
    binlog files.
  */
  Gtid_set gtids_only_in_table;
  /* The previous GTIDs in the last binlog. */
  Gtid_set previous_gtids_logged;
  /// The set of GTIDs that are owned by some thread.
  Owned_gtids owned_gtids;
  /// The SIDNO for this server.
  rpl_sidno server_sidno;

  /// The number of anonymous transactions owned by any client.
  Atomic_int32 anonymous_gtid_count;
  /// The number of GTID-violating transactions that use GTID_NEXT=AUTOMATIC.
  Atomic_int32 automatic_gtid_violation_count;
  /// The number of GTID-violating transactions that use GTID_NEXT=AUTOMATIC.
  Atomic_int32 anonymous_gtid_violation_count;
  /// The number of clients that are executing
  /// WAIT_FOR_EXECUTED_GTID_SET or WAIT_UNTIL_SQL_THREAD_AFTER_GTIDS.
  Atomic_int32 gtid_wait_count;

  ...
}
```


## Gtid_state类public函数
```
class Gtid_state
{

//构造函数
Gtid_state(Checkable_rwlock *_sid_lock, Sid_map *_sid_map)
	
//这个函数将server_uuid保存到Gtid_state的Sid_map,并得到对应的sidno，保存到server_sidno。
int init();

//清理lost_gtids、executed_gtids、gtids_only_in_table、previous_gtids_logged这几个GTID集合
int clear(THD *thd);

//判断executed_gtids是否包含传入的gtid
bool is_executed(const Gtid &gtid) const

//返回gtid的owner
my_thread_id get_owner(const Gtid &gtid) const

//owned_gtids增加gtid. 如果thd->gtid_next_list非空，则加入gtid到其中，否则加入gtid到thd->owned_gtid
enum_return_status acquire_ownership(THD *thd, const Gtid &gtid);

  /**
    This function updates both the THD and the Gtid_state to reflect that
    the transaction set of transactions has ended, and it does this for the
    whole commit group (by following the thd->next_to_commit pointer).

    It will:

    - Clean up the thread state when a thread owned GTIDs is empty.
    - Release ownership of all GTIDs owned by the THDs. This removes
      the GTIDs from Owned_gtids and clears the ownership status in the
      THDs object.
    - Add the owned GTIDs to executed_gtids when the thread is committing.
    - Decrease counters of GTID-violating transactions.
    - Send a broadcast on the condition variable for every sidno for
      which we released ownership.

    @param first_thd The first thread of the group commit that needs GTIDs to
                     be updated.
  */
void update_commit_group(THD *first_thd);

  /**
    Remove the GTID owned by thread from owned GTIDs, stating that
    thd->owned_gtid was committed.

    This will:
     - remove owned GTID from owned_gtids;
     - remove all owned GTIDS from thd->owned_gtid and thd->owned_gtid_set;

    @param thd Thread for which owned groups are updated.
  */
void update_on_commit(THD *thd);

  /**
    Update the state after the given thread has rollbacked.

    This will:
     - release ownership of all GTIDs owned by the THD;
     - remove owned GTID from owned_gtids;
     - remove all owned GTIDS from thd->owned_gtid and thd->owned_gtid_set;
     - send a broadcast on the condition variable for every sidno for
       which we released ownership.

    @param thd Thread for which owned groups are updated.
  */
void update_on_rollback(THD *thd);


//anonymous_gtid_count 值加1
void acquire_anonymous_ownership()

//anonymous_gtid_count 值减1
void release_anonymous_ownership()

//返回anonymous_gtid_count值
int32 get_anonymous_ownership_count()

  /**
    Increase the global counter when starting a GTID-violating
    transaction having GTID_NEXT=AUTOMATIC.
	automatic_gtid_violation_count 的值加1
  */
void begin_automatic_gtid_violating_transaction()

  /**
    Decrease the global counter when ending a GTID-violating
    transaction having GTID_NEXT=AUTOMATIC.
	automatic_gtid_violation_count 值减1
  */
void end_automatic_gtid_violating_transaction()

  /**
    Return the number of ongoing GTID-violating transactions having
    GTID_NEXT=AUTOMATIC.
  */
int32 get_automatic_gtid_violating_transaction_count()


  /**
    Increase the global counter when starting a GTID-violating
    transaction having GTID_NEXT=ANONYMOUS.
	anonymous_gtid_violation_count 值加1
  */
void begin_anonymous_gtid_violating_transaction()

  /**
    Decrease the global counter when ending a GTID-violating
    transaction having GTID_NEXT=ANONYMOUS.
	anonymous_gtid_violation_count 值减1
  */
void end_anonymous_gtid_violating_transaction()


void end_gtid_violating_transaction(THD *thd);


  /**
    Return the number of ongoing GTID-violating transactions having
    GTID_NEXT=AUTOMATIC.
	返回 anonymous_gtid_violation_count 值
  */
int32 get_anonymous_gtid_violating_transaction_count()
  
  /**
    Increase the global counter when starting a call to
    WAIT_FOR_EXECUTED_GTID_SET or WAIT_UNTIL_SQL_THREAD_AFTER_GTIDS.
	gtid_wait_count 值加1
  */
void begin_gtid_wait(enum_gtid_mode_lock gtid_mode_lock)

  /**
    Decrease the global counter when ending a call to
    WAIT_FOR_EXECUTED_GTID_SET or WAIT_UNTIL_SQL_THREAD_AFTER_GTIDS.
	gtid_wait_count 值减1
  */
void end_gtid_wait()

  /**
    Return the number of clients that have an ongoing call to
    WAIT_FOR_EXECUTED_GTID_SET or WAIT_UNTIL_SQL_THREAD_AFTER_GTIDS.
	返回 gtid_wait_count 值
  */
int32 get_gtid_wait_count()
 
  /**
    Return the last executed GNO for a given SIDNO, e.g.
    for the following set: UUID:1-10, UUID:12, UUID:15-20
    20 will be returned.

    @param sidno The group's SIDNO.

    @retval The GNO or 0 if set is empty.
  */
rpl_gno get_last_executed_gno(rpl_sidno sidno) const; 

  /**
    Generates the GTID (or ANONYMOUS, if GTID_MODE = OFF or
    OFF_PERMISSIVE) for the THD, and acquires ownership.
	*/
enum_return_status generate_automatic_gtid(THD *thd,
                                             rpl_sidno specified_sidno= 0,
                                             rpl_gno specified_gno= 0,
                                             rpl_sidno *locked_sidno= NULL);
  /// Locks a mutex for the given SIDNO.
  void lock_sidno(rpl_sidno sidno) { sid_locks.lock(sidno); }
  /// Unlocks a mutex for the given SIDNO.
  void unlock_sidno(rpl_sidno sidno) { sid_locks.unlock(sidno); }
  /// Broadcasts updates for the given SIDNO.
  void broadcast_sidno(rpl_sidno sidno) { sid_locks.broadcast(sidno); }
  /// Assert that we own the given SIDNO.
  void assert_sidno_lock_owner(rpl_sidno sidno)
  { sid_locks.assert_owner(sidno); }

  //Wait for a signal on the given SIDNO.
  bool wait_for_sidno(THD *thd, rpl_sidno sidno, struct timespec *abstime);

  /**
    This is only a shorthand for wait_for_sidno, which contains
    additional debug printouts and assertions for the case when the
    caller waits for one specific GTID.
  */
  bool wait_for_gtid(THD *thd, const Gtid &gtid, struct timespec* abstime= NULL);
  
  //Wait until the given Gtid_set is included in @@GLOBAL.GTID_EXECUTED.
  bool wait_for_gtid_set(THD *thd, Gtid_set *gtid_set, longlong timeout);
  
  /**
    Locks one mutex for each SIDNO where the given Gtid_set has at
    least one GTID.  Locks are acquired in order of increasing SIDNO.
  */
  void lock_sidnos(const Gtid_set *set);
  
  /**
    Unlocks the mutex for each SIDNO where the given Gtid_set has at
    least one GTID.
  */
  void unlock_sidnos(const Gtid_set *set);
  
  /**
    Broadcasts the condition variable for each SIDNO where the given
    Gtid_set has at least one GTID.
  */
  void broadcast_sidnos(const Gtid_set *set);
  
  /**
    Ensure that owned_gtids, executed_gtids, lost_gtids, gtids_only_in_table,
    previous_gtids_logged and sid_locks have room for at least as many SIDNOs
    as sid_map.
	*/
  enum_return_status ensure_sidno();

  /**
    Adds the given Gtid_set to lost_gtids and executed_gtids.
    lost_gtids must be a subset of executed_gtids.
	*/
  enum_return_status add_lost_gtids(const Gtid_set *gtid_set);

  /// Return a pointer to the Gtid_set that contains the lost groups.
  const Gtid_set *get_lost_gtids() const { return &lost_gtids; }
  
  /*
    Return a pointer to the Gtid_set that contains the stored groups
    in gtid_executed table.
  */
  const Gtid_set *get_executed_gtids() const { return &executed_gtids; }
  
  /*
    Return a pointer to the Gtid_set that contains the stored groups
    only in gtid_executed table, not in binlog files.
  */
  const Gtid_set *get_gtids_only_in_table() const
  {
    return &gtids_only_in_table;
  }
  
  /*
    Return a pointer to the Gtid_set that contains the previous stored
    groups in the last binlog file.
  */
  const Gtid_set *get_previous_gtids_logged() const
  {
    return &previous_gtids_logged;
  }
  
  /// Return a pointer to the Owned_gtids that contains the owned groups.
  const Owned_gtids *get_owned_gtids() const { return &owned_gtids; }
  
  /// Return the server's SID's SIDNO
  rpl_sidno get_server_sidno() const { return server_sidno; }
  
  /// Return the server's SID
  const rpl_sid &get_server_sid() const
  {
    return global_sid_map->sidno_to_sid(server_sidno);
  }
  
  /**
    Debug only: Returns an upper bound on the length of the string
    generated by to_string(), not counting '\0'.  The actual length
    may be shorter.
  */
  size_t get_max_string_length() const
  {
    return owned_gtids.get_max_string_length() +
      executed_gtids.get_string_length() +
      lost_gtids.get_string_length() +
      gtids_only_in_table.get_string_length() +
      previous_gtids_logged.get_string_length() +
      150;
  }
  
  /// Debug only: Generate a string in the given buffer and return the length.
  int to_string(char *buf) const
  {
    char *p= buf;
    p+= sprintf(p, "Executed GTIDs:\n");
    p+= executed_gtids.to_string(p);
    p+= sprintf(p, "\nOwned GTIDs:\n");
    p+= owned_gtids.to_string(p);
    p+= sprintf(p, "\nLost GTIDs:\n");
    p+= lost_gtids.to_string(p);
    p+= sprintf(p, "\nGTIDs only_in_table:\n");
    p+= lost_gtids.to_string(p);
    return (int)(p - buf);
  }
  
  /// Debug only: return a newly allocated string, or NULL on out-of-memory.
  char *to_string() const
  {
    char *str= (char *)my_malloc(key_memory_Gtid_state_to_string,
                                 get_max_string_length(), MYF(MY_WME));
    to_string(str);
    return str;
  }
  
  /**
    Save gtid owned by the thd into executed_gtids variable
    and gtid_executed table.

    @param thd Session to commit
    @retval
        0    OK
    @retval
        -1   Error
  */
  int save(THD *thd);
  
  /**
    Insert the gtid set into table.

    @param gtid_set  contains a set of gtid, which holds
                     the sidno and the gno.

    @retval
      0    OK
    @retval
      -1   Error
  */
  int save(const Gtid_set *gtid_set);
  
  /**
    Save the set of gtids logged in the last binlog into gtid_executed table.

    @param on_rotation  true if it is on binlog rotation.

    @retval
      0    OK
    @retval
      -1   Error
  */
  int save_gtids_of_last_binlog_into_table(bool on_rotation);
  
  /**
    Fetch gtids from gtid_executed table and store them into
    gtid_executed set.

    @retval
      0    OK
    @retval
      1    The table was not found.
    @retval
      -1   Error
  */
  int read_gtid_executed_from_table();
  
  int compress(THD *thd);
  
  /**
    Push a warning to client if user is modifying the gtid_executed
    table explicitly by a non-XA transaction. Push an error to client
    if user is modifying it explicitly by a XA transaction.
	*/
bool warn_or_err_on_modify_gtid_table(THD *thd, TABLE_LIST *table);

}
```

##重点函数详解
```
update_commit_group(THD *first_thd)
	|-global_sid_lock->rdlock();
	|-update_gtids_impl_lock_sidnos(first_thd); //对要提交的gtid集合涉及到的sidno加锁
	|-for (THD *thd= first_thd; thd != NULL; thd= thd->next_to_commit)
		|-bool is_commit= (thd->commit_error != THD::CE_COMMIT_ERROR);
		//如果要提交的GTID是空的，则跳过这个GTID。如果提交有错误会回滚这个GTID，也跳过这个GTID
		|-if (update_gtids_impl_do_nothing(thd) ||
			(!is_commit && update_gtids_impl_check_skip_gtid_rollback(thd)))
			|-continue;
		|-bool more_trx_with_same_gtid_next= update_gtids_impl_begin(thd); //返回thd->is_commit_in_middle_of_statement
		|-if (thd->owned_gtid.sidno == THD::OWNED_SIDNO_GTID_SET)
			|-update_gtids_impl_own_gtid_set(thd, is_commit); //提交GTID集合
		|-else if (thd->owned_gtid.sidno > 0)
			|-update_gtids_impl_own_gtid(thd, is_commit);	//提交一个GTID
		|-else if (thd->owned_gtid.sidno == THD::OWNED_SIDNO_ANONYMOUS)
			|-update_gtids_impl_own_anonymous(thd, &more_trx_with_same_gtid_next);	//提交匿名GTID
		|-else
			|-update_gtids_impl_own_nothing(thd);	//什么也不做
		|-update_gtids_impl_end(thd, more_trx_with_same_gtid_next);
	|-update_gtids_impl_broadcast_and_unlock_sidnos(); //对涉及到的sidno释放锁
	|-global_sid_lock->unlock();
	|-DBUG_VOID_RETURN;


update_on_commit(THD *thd)
	|-update_gtids_impl(thd, true);
		|-if (update_gtids_impl_do_nothing(thd))
			|-DBUG_VOID_RETURN;
		|-bool more_trx_with_same_gtid_next= update_gtids_impl_begin(thd);
		|-if (thd->owned_gtid.sidno == THD::OWNED_SIDNO_GTID_SET)
			|-update_gtids_impl_own_gtid_set(thd, is_commit);//提交GTID集合
		|-else if (thd->owned_gtid.sidno > 0)
			|-rpl_sidno sidno= thd->owned_gtid.sidno;
			|-update_gtids_impl_lock_sidno(sidno);
			|-update_gtids_impl_own_gtid(thd, is_commit);//提交一个GTID
			|=update_gtids_impl_broadcast_and_unlock_sidno(sidno);
		|-else if (thd->owned_gtid.sidno == THD::OWNED_SIDNO_ANONYMOUS)
			|-update_gtids_impl_own_anonymous(thd, &more_trx_with_same_gtid_next);//提交匿名GTID
		|-else
			|-update_gtids_impl_own_nothing(thd);//什么也不做
		|-update_gtids_impl_end(thd, more_trx_with_same_gtid_next);
		|-DBUG_VOID_RETURN;


update_on_rollback(THD *thd)
	|-if (!update_gtids_impl_check_skip_gtid_rollback(thd))
		|-update_gtids_impl(thd, false);


Gtid_state::generate_automatic_gtid(THD *thd,rpl_sidno specified_sidno,rpl_gno specified_gno,rpl_sidno *locked_sidno)
	|-
靠

靠 靠 靠
```
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	




