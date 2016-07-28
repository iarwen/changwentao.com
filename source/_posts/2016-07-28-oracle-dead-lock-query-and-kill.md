title: oracle 数据库死锁查询以及解锁
date: 2016/07/28 14:49:25
categories:
- oracle
tags: [oracle,死锁]
---
查询产生死锁的session以及相关信息：
```
select t2.SID,t2.SERIAL#,*
from v$locked_object t1,v$session t2,v$sqltext t3
where t1.session_id=t2.sid
and t2.sql_address=t3.address
order by t2.logon_time;
```
解锁语句：
```
alter system kill session '134,42671' immediate;
```
134,42671为t2.SID,t2.SERIAL#两个字段

能产生死锁，很大部分是由于SQL语句写的有问题或者对表的锁控制有问题，解除锁仅仅只是消除现象，根源还要细细检查。