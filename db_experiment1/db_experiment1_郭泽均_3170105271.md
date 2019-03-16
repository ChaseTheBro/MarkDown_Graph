# 数据库系统原理实验报告1

郭泽均 软件工程 3170105271</br>

# 实验目的和要求

1. 通过安装某个数据库管理系统，初步了解DBMS的运行环境。
2. 了解DBMS交互界面、图形界面和系统管理工具的使用。
3. 搭建实验平台。

# 实验内容和原理

1.	根据某个DBMS的安装说明等文档，安装DBMS。
2.	了解DBMS的用户管理。
3.	熟悉交互界面的基本交互命令。
4.	熟悉图形界面的功能和操作。
5.	了解基本的DBMS管理功能和操作。
6.	熟悉在线帮助系统的使用。
7.	完成实验报告。

# 主要仪器和设备

Macintosh</br>
VPS

# 操作方法与实验步骤

1. 通过Mac的终端工具，在命令行通过ssh连接远程的Linus主机 </br>
操作命令`ssh user_name@server_ip`</br>
连接并操作完成之后通过命令`exit`断开与远端VPS的连接，过程中要注意远程主机的22端口要保持畅通，否则无法连接。</br>
使用指令`ping server_ip`和`telnet server_ip`来判断远端主机的网络情况
2. 我的VPS主机是CentOS系统，故通过指令`yum install mysql-server`来安装mysql数据库。通过`mysql --initialize`初始化数据库。之后通过`systemctl start mysqld`和`systemctl status mysqld`启动和查看数据库的运行状态。
3. Mysql安装成功后，默认的root用户密码为空，使用以下命令来创建root用户的密码`mysqladmin -u root password "new_password";`</br>
现在可以通过以下指令来访问mysql数据库。</br>
```
[root@host]# mysql -u root -p
Enter password:*******
```
4. 连接到VPS之后首先检查mysql服务器是否启动</br>
`ps -ef | grep mysqld`</br>
如果没有启动，使用一下命令启动mysql服务器</br>
```
root@host# cd /usr/bin
./mysqld_safe &
```
如果你想关闭目前运行的 MySQL 服务器, 你可以执行以下命令
```
root@host# cd /usr/bin
./mysqladmin -u root -p shutdown
Enter password: ******
```
5. mysql用户设置</br>
如果你需要添加 MySQL 用户，你只需要在 mysql 数据库中的 user 表添加新用户即可。以下为添加用户的的实例，用户名为guest，密码为guest123，并授权用户可进行 SELECT, INSERT 和 UPDATE操作权限
  ```
  root@host# mysql -u root -p
  Enter password:*******
  mysql> use mysql;
  Database changed

  mysql> INSERT INTO user
            (host, user, password,
             select_priv, insert_priv, update_priv)
             VALUES ('localhost', 'guest',
             PASSWORD('guest123'), 'Y', 'Y', 'Y');
  Query OK, 1 row affected (0.20 sec)

  mysql> FLUSH PRIVILEGES;
  Query OK, 1 row affected (0.01 sec)

  mysql> SELECT host, user, password FROM user WHERE user = 'guest';
  +-----------+---------+------------------+
  | host      | user    | password         |
  +-----------+---------+------------------+
  | localhost | guest | 6f8c114b58f2ce9e |
  +-----------+---------+------------------+
  1 row in set (0.00 sec)
  ```
  另外一种添加用户的方法为通过SQL的 GRANT 命令，以下命令会给指定数据库TUTORIALS添加用户 zara ，密码为 zara123

  ```
  root@host# mysql -u root -p
  Enter password:*******
  mysql> use mysql;
  Database changed

  mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
    -> ON TUTORIALS.*
    -> TO 'zara'@'localhost'
    -> IDENTIFIED BY 'zara123';
  ```
6. 管理mysql的命令 </br>
选择要操作的Mysql数据库，使用该命令后所有Mysql命令都只针对该数据库。</br>
`mysql> use RUNOOB;`</br>
列出 MySQL 数据库管理系统的数据库列表。</br>
`SHOW DATABASES;`</br>
显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。</br>
`SHOW TABLES;`</br>
显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。</br>
`SHOW COLUMNS FROM runoob_tbl;`</br>
显示数据表的详细索引信息，包括PRIMARY KEY（主键）。</br>
`SHOW INDEX FROM runoob_tbl;`
7. 我们可以在登陆 MySQL 服务后，使用`create`命令创建数据库，语法如下:</br>
  `CREATE DATABASE 数据库名;`</br>
  使用`DROP`命令删除数据表`drop database <数据库名>;`

# 图形界面的操作和在线帮助系统的使用

*图形界面的使用不是非常的有用而且不同的图形界面逻辑差异很大，所以不在报告中表述</br>
由于我有自己的在线服务器所以不需要使用在线帮助系统，操作的方式已经在上述报告中展示清楚了*
