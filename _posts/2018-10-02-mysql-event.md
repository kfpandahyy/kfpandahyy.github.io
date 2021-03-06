---
Date: October 20, 2018
title: MySQL events
layout: post
comments: true
language: chinese
category: [mysql]
keywords: event
description: event
---
event
<!-- more -->


# CREATE DATABASE

* 关闭GTID
```
+------------------+-----+----------------+-----------+-------------+---------------------------------------+
| Log_name         | Pos | Event_type     | Server_id | End_log_pos | Info                                  |
+------------------+-----+----------------+-----------+-------------+---------------------------------------+
| mysql-bin.000001 |   4 | Format_desc    |         1 |         123 | Server ver: 5.7.23-log, Binlog ver: 4 |
| mysql-bin.000001 | 123 | Previous_gtids |         1 |         154 |                                       |
| mysql-bin.000001 | 154 | Anonymous_Gtid |         1 |         219 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'  |
| mysql-bin.000001 | 219 | Query          |         1 |         310 | create database db1                   |
+------------------+-----+----------------+-----------+-------------+---------------------------------------+
```


* 开启GTID
```
| mysql-bin.000003 | 154 | Gtid           |         1 |         219 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:1' |
| mysql-bin.000003 | 219 | Query          |         1 |         310 | create database db1                                               |
```

# DROP DATABASE
# 关闭GTID
```
| mysql-bin.000001 | 1549 | Anonymous_Gtid |         1 |        1614 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 1614 | Query          |         1 |        1697 | DROP DATABASE db1                                   |
```

* 开启GTID

```
| mysql-bin.000003 | 310 | Gtid           |         1 |         375 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:2' |
| mysql-bin.000003 | 375 | Query          |         1 |         458 | drop database db1                                                 |
```

# INNODB
## CREATE TABLE

* 关闭GTID
```
| mysql-bin.000001 | 310 | Anonymous_Gtid |         1 |         375 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'           |
| mysql-bin.000001 | 375 | Query          |         1 |         482 | use `db1`; create table t(id int)engine=innodb |
```

* 开启GTID

```
| mysql-bin.000003 | 1083 | Gtid           |         1 |        1148 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:7' |
| mysql-bin.000003 | 1148 | Query          |         1 |        1255 | use `db1`; create table t(id int)engine=innodb                    |
```


## INSERT INTO TABLE

* 关闭GTID
```
| mysql-bin.000001 | 482 | Anonymous_Gtid |         1 |         547 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'           |
| mysql-bin.000001 | 547 | Query          |         1 |         618 | BEGIN                                          |
| mysql-bin.000001 | 618 | Table_map      |         1 |         661 | table_id: 108 (db1.t)                          |
| mysql-bin.000001 | 661 | Write_rows     |         1 |         701 | table_id: 108 flags: STMT_END_F                |
| mysql-bin.000001 | 701 | Xid            |         1 |         732 | COMMIT /* xid=19 */                            |
```


* 开启GTID

```
| mysql-bin.000003 | 1255 | Gtid           |         1 |        1320 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:8' |
| mysql-bin.000003 | 1320 | Query          |         1 |        1391 | BEGIN                                                             |
| mysql-bin.000003 | 1391 | Table_map      |         1 |        1434 | table_id: 109 (db1.t)                                             |
| mysql-bin.000003 | 1434 | Write_rows     |         1 |        1474 | table_id: 109 flags: STMT_END_F                                   |
| mysql-bin.000003 | 1474 | Xid            |         1 |        1505 | COMMIT /* xid=30 */                                               |
```

## LOAD DATA

* 关闭GTID
```
| mysql-bin.000004 | 715 | Anonymous_Gtid |         1 |         780 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000004 | 780 | Query          |         1 |         851 | BEGIN                                               |
| mysql-bin.000004 | 851 | Table_map      |         1 |         894 | table_id: 109 (db1.t)                               |
| mysql-bin.000004 | 894 | Write_rows     |         1 |         944 | table_id: 109 flags: STMT_END_F                     |
| mysql-bin.000004 | 944 | Xid            |         1 |         975 | COMMIT /* xid=23 */                                 |
```

* 开启GTID
```
| mysql-bin.000005 |  838 | Gtid           |         1 |         903 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:19' |
| mysql-bin.000005 |  903 | Query          |         1 |         974 | BEGIN                                                              |
| mysql-bin.000005 |  974 | Table_map      |         1 |        1017 | table_id: 109 (db1.t)                                              |
| mysql-bin.000005 | 1017 | Write_rows     |         1 |        1067 | table_id: 109 flags: STMT_END_F                                    |
| mysql-bin.000005 | 1067 | Xid            |         1 |        1098 | COMMIT /* xid=12 */                                                |
```


## DROP TABLE

* 关闭GTID
```
| mysql-bin.000001 | 732 | Anonymous_Gtid |         1 |         797 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 797 | Query          |         1 |         909 | use `db1`; DROP TABLE `t` /* generated by server */ |
```
* 开启GTID

```
| mysql-bin.000003 | 1505 | Gtid           |         1 |        1570 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:9' |
| mysql-bin.000003 | 1570 | Query          |         1 |        1682 | use `db1`; DROP TABLE `t` /* generated by server */               |
```
# MYISAM
## CREATE TABLE

* 关闭GTID
```
| mysql-bin.000001 | 909 | Anonymous_Gtid |         1 |         974 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 974 | Query          |         1 |        1081 | use `db1`; CREATE TABLE T(ID INT)engine=myisam      |
```
* 开启GTID

```
| mysql-bin.000003 | 1682 | Gtid           |         1 |        1747 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:10' |
| mysql-bin.000003 | 1747 | Query          |         1 |        1854 | use `db1`; create table t(id int)engine=myisam                     |
```

## INSERT INTO TABLE

*关闭GTID
```
| mysql-bin.000001 | 1081 | Anonymous_Gtid |         1 |        1146 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 1146 | Query          |         1 |        1217 | BEGIN                                               |
| mysql-bin.000001 | 1217 | Table_map      |         1 |        1260 | table_id: 110 (db1.T)                               |
| mysql-bin.000001 | 1260 | Write_rows     |         1 |        1300 | table_id: 110 flags: STMT_END_F                     |
| mysql-bin.000001 | 1300 | Query          |         1 |        1372 | COMMIT                                              |
```
* 开启GTID

```
| mysql-bin.000003 | 1854 | Gtid           |         1 |        1919 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:11' |
| mysql-bin.000003 | 1919 | Query          |         1 |        1990 | BEGIN                                                              |
| mysql-bin.000003 | 1990 | Table_map      |         1 |        2033 | table_id: 110 (db1.t)                                              |
| mysql-bin.000003 | 2033 | Write_rows     |         1 |        2073 | table_id: 110 flags: STMT_END_F                                    |
| mysql-bin.000003 | 2073 | Query          |         1 |        2145 | COMMIT                                                             |
```
## DROP TABLE

*关闭GTID
```
| mysql-bin.000001 | 1372 | Anonymous_Gtid |         1 |        1437 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 1437 | Query          |         1 |        1549 | use `db1`; DROP TABLE `T` /* generated by server */ |
```
* 开启GTID

```
| mysql-bin.000003 | 2145 | Gtid           |         1 |        2210 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:12' |
| mysql-bin.000003 | 2210 | Query          |         1 |        2322 | use `db1`; DROP TABLE `t` /* generated by server */                |
```
## LOAD DATA
* 关闭GTID
```
| mysql-bin.000004 | 1324 | Anonymous_Gtid |         1 |        1389 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000004 | 1389 | Query          |         1 |        1460 | BEGIN                                               |
| mysql-bin.000004 | 1460 | Table_map      |         1 |        1503 | table_id: 110 (db1.t)                               |
| mysql-bin.000004 | 1503 | Write_rows     |         1 |        1553 | table_id: 110 flags: STMT_END_F                     |
| mysql-bin.000004 | 1553 | Query          |         1 |        1625 | COMMIT                                              |
```

* 开启GTID
```
| mysql-bin.000005 | 1447 | Gtid           |         1 |        1512 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:22' |
| mysql-bin.000005 | 1512 | Query          |         1 |        1583 | BEGIN                                                              |
| mysql-bin.000005 | 1583 | Table_map      |         1 |        1626 | table_id: 110 (db1.t)                                              |
| mysql-bin.000005 | 1626 | Write_rows     |         1 |        1676 | table_id: 110 flags: STMT_END_F                                    |
| mysql-bin.000005 | 1676 | Query          |         1 |        1748 | COMMIT                                                             |
```

# MEMORY 
## CREATE TABLE
* 关闭GTID
```
| mysql-bin.000001 | 1853 | Anonymous_Gtid |         1 |        1918 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 1918 | Query          |         1 |        2025 | use `DB1`; CREATE TABLE T(ID INT)ENGINE=MEMORY      |
```
* 开启GTID

```
| mysql-bin.000003 | 2322 | Gtid           |         1 |        2387 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:13' |
| mysql-bin.000003 | 2387 | Query          |         1 |        2494 | use `db1`; create table t(id int)engine=memory                     |
```
## INSERT INTO TABLE
* 关闭GTID
```
| mysql-bin.000001 | 2025 | Anonymous_Gtid |         1 |        2090 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000001 | 2090 | Query          |         1 |        2161 | BEGIN                                               |
| mysql-bin.000001 | 2161 | Table_map      |         1 |        2204 | table_id: 113 (DB1.T)                               |
| mysql-bin.000001 | 2204 | Write_rows     |         1 |        2244 | table_id: 113 flags: STMT_END_F                     |
| mysql-bin.000001 | 2244 | Query          |         1 |        2316 | COMMIT                                              |
```
* 开启GTID

```
| mysql-bin.000003 | 2494 | Gtid           |         1 |        2559 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:14' |
| mysql-bin.000003 | 2559 | Query          |         1 |        2630 | BEGIN                                                              |
| mysql-bin.000003 | 2630 | Table_map      |         1 |        2673 | table_id: 111 (db1.t)                                              |
| mysql-bin.000003 | 2673 | Write_rows     |         1 |        2713 | table_id: 111 flags: STMT_END_F                                    |
| mysql-bin.000003 | 2713 | Query          |         1 |        2785 | COMMIT                                                             |
```
## DROP TABLE 
* 关闭GTID
```
| mysql-bin.000004 | 366 | Anonymous_Gtid |         1 |         431 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000004 | 431 | Query          |         1 |         543 | use `db1`; DROP TABLE `t` /* generated by server */ |
```
* 开启GTID

```
| mysql-bin.000003 | 2785 | Gtid           |         1 |        2850 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:15' |
| mysql-bin.000003 | 2850 | Query          |         1 |        2962 | use `db1`; DROP TABLE `t` /* generated by server */                |
```
## LOAD DATA
* 关闭GTID
```
| mysql-bin.000004 | 1974 | Anonymous_Gtid |         1 |        2039 | SET @@SESSION.GTID_NEXT= 'ANONYMOUS'                |
| mysql-bin.000004 | 2039 | Query          |         1 |        2110 | BEGIN                                               |
| mysql-bin.000004 | 2110 | Table_map      |         1 |        2153 | table_id: 111 (db1.t)                               |
| mysql-bin.000004 | 2153 | Write_rows     |         1 |        2203 | table_id: 111 flags: STMT_END_F                     |
| mysql-bin.000004 | 2203 | Query          |         1 |        2275 | COMMIT                                              |
```
* 开启GTID
```
| mysql-bin.000005 | 2097 | Gtid           |         1 |        2162 | SET @@SESSION.GTID_NEXT= '4c47c528-c609-11e8-9148-fa163e7495d9:25' |
| mysql-bin.000005 | 2162 | Query          |         1 |        2233 | BEGIN                                                              |
| mysql-bin.000005 | 2233 | Table_map      |         1 |        2276 | table_id: 111 (db1.t)                                              |
| mysql-bin.000005 | 2276 | Write_rows     |         1 |        2326 | table_id: 111 flags: STMT_END_F                                    |
| mysql-bin.000005 | 2326 | Query          |         1 |        2398 | COMMIT                                                             |
```


