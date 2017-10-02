---
title: 管理Mysql常用指令
comment: true
date: 2016-04-08 16:57:30
categories: Mysql
tags:
- Mysql
---

![Mysql](https://labs.mysql.com/common/logos/mysql-logo.svg?v2)

_知识会更新，数据库系统也一样，本文只保证对Mysql 5.7以及MariaDB 10有效。_

### 编码篇
* 展示当前默认的编码和字符集
{% highlight mysql %}
SHOW VARIABLES LIKE  'char%';
{% endhighlight %}
* 修改服务器默认编码,通过修改配置文件*.cnf
{% highlight cnf %}
skip-character-set-client-handshake
collation-server=utf8_unicode_ci  #特定字符集，如果没有特殊要求可以注释
character-set-server=utf8
{% endhighlight %}
### 统一表名大小写
修改配置文件*.cnf，加入
{% highlight cnf %}
lower_case_table_names=1
{% endhighlight %}
这将强制保存的表名具体文件为小写，但在逻辑层面表名不再大小写敏感；
为什么这么干？因为阿里云RDS这么干？好处还是蛮多的，想了解更细节的规则或者有定制化的需求还是应该参考[官方手册](https://dev.mysql.com/doc/refman/5.7/en/identifier-case-sensitivity.html)。

### 用户和授权篇
* 获取当前用户
{% highlight mysql %}
select user();
{% endhighlight %}
* 获取用户列表
{% highlight mysql %}
SELECT CONCAT(QUOTE(user),'@',QUOTE(host)) UserAccount FROM mysql.user;
{% endhighlight %}
* 创建一个可远程登录的用户root,并且设定明文密码
{% highlight mysql %}
CREATE USER 'root'@'%' IDENTIFIED BY 'rawPasswd';
{% endhighlight %}
* 授予可远程登录的root所有权限
{% highlight mysql %}
GRANT ALL on * to 'root'@'%' WITH GRANT OPTION;
{% endhighlight %}
* 将dbname的所有权限赋予dbuser
{% highlight mysql %}
GRANT ALL on dbname.* to 'dbuser'@'%';
{% endhighlight %}
* 修改密码
{% highlight mysql %}
SET PASSWORD FOR 'dbuser'@'%' = PASSWORD('rawPasswd');
{% endhighlight %}
* 删除用户
{% highlight mysql %}
DROP USER 'dbuser'@'%';
{% endhighlight %}
### 导出与导入
mysql现有热备份，dump，数据表文件，文本SQL记录，增量备份，组群备份，文件系统快照等多种导出导入的方式；从易用和实用角度考虑只记录dump的导出和导入。

* 以user的身份连接到host:port导出database数据库到dump.sql
{% highlight shell %}
mysqldump -h [host] --port=[port] -p -u [user] --default-character-set=utf8 [database] > dump.sql
{% endhighlight %}
* 以user的身份连接到host:port导入dump.sql到database(需已存在)
{% highlight shell %}
mysql -h [host] --port=[port] -p -u [user] --default-character-set=utf8 [database] < dump.sql
{% endhighlight %}
* 或者以user的身份登录到host:port,再导入dump.sql
{% highlight shell %}
mysql -h [host] --port=[port] -p -u [user] --default-character-set=utf8 
use [database]
source dump.sql
{% endhighlight %}
