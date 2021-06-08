---
title: 'mysql忘记密码[windows]'
date: 2021-03-12 16:52:39
tags:
---
1.关闭服务
```
net stop mysql
mysqld --console --skip-grant-tables --shared-memory
```

2.修改密码
```
mysql -u root -p
use mysql
update user set authentication_string='' where user='root';
quit
```

3.更新密码
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'forget_password';
```
