title: mysql问题手册
date: 2016/05/12 13:00:25
categories:
- mysql
tags: [mysql,mysql-safe]
---

###  root忘记密码
```
/etc/init.d/mysql stop
mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
mysql -u root mysql
```
进入之后随便改密码和host

### 查询慢sql
```
set global slow_query_log=1;
tail -f -n200 /var/lib/mysql/localhost-slow.log
```