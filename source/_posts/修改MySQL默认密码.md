---
title: 修改MySQL默认密码
date: 2016-03-22 11:04:34
tags: MySQL
---
## <i class="fas fa-database"></i> mac MySQL修改密码

1. 关闭mysql服务
2. 进入终端输入：cd /usr/local/mysql/bin/
3. 回车后 登录管理员权限 sudo su
4. 回车后输入以下命令来禁止mysql验证功能 ./mysqld_safe --skip-grant-tables &
5. 回车后mysql会自动重启（偏好设置中mysql的状态会变成running）
6. 输入命令 ./mysql
7. 回车后，输入命令 FLUSH PRIVILEGES; 
8. 回车后，输入命令 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');

<!-- more -->

<head> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/all.js"></script> 
    <script defer src="https://use.fontawesome.com/releases/v5.0.13/js/v4-shims.js"></script> 
</head> 
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.13/css/all.css">


