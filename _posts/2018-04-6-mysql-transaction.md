---
Date: October 21, 2018
title: MySQL Transaction
layout: post
comments: true
language: chinese
category: [mysql]
keywords: mysql transaction
description: mysql transaction
---
mysql transaction
<!-- more -->

## mysql_execute_command
```
mysql_execute_command(THD *thd, bool first_level)
	|-switch (gtid_pre_statement_checks(thd))
		|-case GTID_STATEMENT_EXECUTE:
			|-break;
		|-case GTID_STATEMENT_CANCEL:
			|-DBUG_RETURN(-1);
		|-case GTID_STATEMENT_SKIP:
			|-my_ok(thd);
			|-binlog_gtid_end_transaction(thd);
			|-DBUG_RETURN(0);
  
	|-switch (lex->sql_command)
		...
		|-case SQLCOM_INSERT:
			|-res= lex->m_sql_cmd->execute(thd);
			|-break;
		|-case SQLCOM_DELETE:
			|-res= lex->m_sql_cmd->execute(thd);
			|-break;
		|-case SQLCOM_UPDATE:
			|-res= lex->m_sql_cmd->execute(thd);
			|-break;
		...
 
	|-if (thd->is_error() || (thd->variables.option_bits & OPTION_MASTER_SQL_ERROR))
		|-trans_rollback_stmt(thd);
	|-else
		|-trans_commit_stmt(thd);
	|-if (!(res || thd->is_error()))
		|-binlog_gtid_end_transaction(thd);
```

## trans_commit_stmt
```
trans_commit_stmt(THD *thd)
	|-if (thd->get_transaction()->is_active(Transaction_ctx::STMT))
		|-res= ha_commit_trans(thd, FALSE);
	|-else if (tc_log)
		|-tc_log->commit(thd, false);

```










































