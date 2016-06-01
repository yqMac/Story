---
title: Mysql学习 
tags: Mysql,基础,yqmac
grammar_cjkRuby: true
---



## 一、**数据库安装【解压版--命令行操作】**：

`详细命令可以在数据库官方文档中查询，此处只是常用的`
### 1、安装

` 在官网下载某版本的压缩版，根据自己需求选择版本 ` 

### 2、修改my.ini和环境变量

`my.ini`中找到`[mysqld]`节点

在`[mysqld]`节点中添加`basedir=...datadir=..`

将`... `替换成自己的数据库程序目录和数据库目录

如果配置后启动失败，可能是此处数据库目录有误，偶尔可能会有/的问题* 
### 3、生成默认库
` 清空配置的数据库的目录  

进入数据库`bin`目录,有`mysqld.exe`的目录  

使用命令行执行: `mysqld --initialize --user=mysql --console`  

以上命令会在配置的数据库目录下生成默认库*  
    
### 4、安装和启动服务 ### 
**在bin目录下**
    *安装服务：*
    `mysqld install`
    *卸载服务：*
    `mysqld remove`
    *启动服务：*
    `net start mysql`
    *停止服务：*
    `net stop mysql`
    
### 5、进入mysql

`mysql -u root -p`

### 6、修改密码

*第一种：*

`set password = password('root')`

*第二种：*

`GRANT ALL ON *.* TO 'root'@'localhost' IDENTIFIED BY 'root'`

### 7、連接遠程數據庫

 `mysql -h127.0.0.1 -p3306 -uroot -proot`

## 二**、数据库的操作**：
1、显示所有数据库
`show databases`
        
2、创建数据库
`create database xxx`
        
3、使用数据库
`use xxx`
        
 4、删除数据库
`drop database if exists xxx`

5、导入外部脚本
        `source D:/a/mysql.sql`
        
6、为用户授权

*可以参考上面为root用户修改密码的第二种方式*

`GRANT ALL ON testdatabase.* TO 'yqmac'@'localhost' IDENTIFIED BY '123456'`

7、备份数据库  

退出登录后，
`mysqldum --no-defaults -uroot -proot dahua > d:/dahua.sql`
即将 dahua数据库 备份到D盘下的dahua.sql内
        
## 三、**数据表的基本操作**：
***以下基于当前库【需要使用某个库，使之成为当前库】***

1、显示所有表

`show tables;`
        
2、创建表结构

`create table xxx(`  
            `id  int(10) primary key auto_increment,`  
            `name varchar(50),`  
            `password varchar(50),`  
            `born date,`  
            `nickname varchar(50)`  
        );`  
        
3、查询表结构

        `desc xxx`
        
4、修改表结构
        
*增加字段*

`alter table xxx add nickname varchar(50)`
        
*删除字段*
        
`alter table xxx drop cloumn nickname`
        
*修改字段*
        
`alter table xxx change nickname nname varchar(50)`
        
*更改字段顺序【after .../first】*
        
`alter table xxx modify nname varchar(50) after password`

5、创建关联表

*创建一个已经存在的表*

*创建新表与上表关联*

*在创建表结构的最后一个字段后面添加：*

`Constraint foreign key (本表某字段) reference 上表(上表的某字段)`

*就是将本表的某字段与商标的某字段相关联*
        
6、删除表【注意关联表的删除顺序是什么】
        
`drop xxx`
*有关联表的表删除时，要先删除被关联的表*
        
        
## 四、**数据表中数据--sql语句基础**
1、插入数据

*输入要插入的字段*

`insert into xxx(xxx1,xx2,xx3) value(vv1,vv2,vv3);`

*插入该表的全部字段*

`insert into xxx values(null,v1,v2,3)`

*插入其他表中已经存在的数据*

`insert into t_user(username,nickname) select Sno,Sname from t_Strudent`

2、查询记录

*全部查询*

`select * from xxx [where ...]`

*字段查询*

`select x,x,x,x from xxx [where ...]`

3、更新记录

`update xxx set x1=v1,x2=v2 where id=2`

4、删除记录

`delete from  xxx where id=3`

*清空表中的全部信息，包括重置id*

`truncate table xxx`

5、简单函数和部分关键字

`max()、min()、count()、like、as、in、between、is null、is not null、desc、asc、order by、 group by 、having`
        
## 五、**关联表的操作**

`select * from table1 t1 join table2 t2 on (t1. t2id = t2 .id ) where t2. id in (1,2,3);`

1、内连接

 `  join`
          
2、左连接
        
`left join`
            
 *左连接 指的是 用左边的表作为主表来连接右边的表，如果右边的表没有对应的数据，则自动拿NULL填充*
            
3、右连接
        
`right join`
            
*右连接 指的是 用右边的表作为主表来连接左边的表，如果左的表没有对应的数据，则自动拿NULL填充*
            
六、JDBC
