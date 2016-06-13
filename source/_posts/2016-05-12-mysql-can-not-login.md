title: mysql root账户不能登陆
date: 2016/05/12 13:00:25
categories:
- mysql
tags: [mysql,mysql-safe]
---

适用于忘记密码，修改错host的补救
```
# /etc/init.d/mysql stop
# mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
# mysql -u root mysql
# 进入之后随便改密码和host
```