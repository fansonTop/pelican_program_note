Title: mysql简介
Date: 2021-11-09 20:17
Category: mysql
Tags: java, mysql
Authors: Fanson
Summary: 对mysql基础知识的一些总结


---------------------------------

### 数据库数据类型

1. 整数： int 和 bigint（相当于long）
2. 浮点数： double(m, d) m代表总长度， d代表小数点长度  23.564 m=5 d=3
3. 超高精度浮点数： decimal(m, d)
4. 字符串： 
   - char(m): 固定长度 char(10)
   - varchar(m): 可变长度 varchar(10)
   - text(m): 可变长度
5. 日期
   - date: 包存年月日 "2019-12-10"  
   - time: 保存时分秒 "17:30:28"
   - datetime: 默认null， 最大值为 9999-12-31
   - timestamp: 默认值为当前系统时间，最大值为 2038-1-19
   - ex:  create table t_table(t1 date, t2 time, t3 datetime, t4 timestamp);
   - ex:  insert into t_table values('2019-11-8', '17:30:18', '2019-11-8 10:10:10', null);

---------------------------------

### 数据库（database）相关SQL语句

1. 查找所有数据库
   - show databases;
2. 创建数据库
   - create database 数据库名 character set utf8/gbk;
   - ex: create database mydatabase character set utf8;
3. 查询数据库详情
   - show create database 数据库名;
   - ex: show create database mydatabase;
4. 删除数据库
   - drop database 数据库名;
   - ex: drop database mydatabase;
5. 使用数据库
   - use 数据库名;
   - ex: use mydatabase;

---------------------------------

### 表（table）相关SQL语句

    操作表和数据时， 一定要保证使用了某个数据库
    create database mydatabase character set utf8;
    use mydatabase;

1. 创建表
   - create table 表名(字段 类型, 字段 类型, 字段 类型) charset=utf8/gbk;
   - ex: create table myclass(name varchar(10), age int, salary double) charset=utf8;
2. 查询所有表
   - show tables;
3. 查看表详情
   - show create table 表名;
   - ex: show create table mytable;
4. 查看表字段
   - desc 表名;
   - ex: desc mytable;
5. 删除表
   - drop table 表名;
   - ex: drop table mytable;
6. 添加表字段
   - alter table 表名 add 字段名 类型;
   - ex: alter table mytable add gender varchar(10);
   - 在前面添加字段：alter table 表名 add 字段名 类型 first;
   - ex: alter table mytable add gender varchar(10) first;
   - 在后面添加字段：alter table 表名 add 字段名类型 after 字段;
   - ex: alter table mytable add gender varchar(10) after name;
7. 删除表字段
   - alter table 表名 drop 字段名;
   - ex: alter table mytable drop name;
8. 修改表字段
   - alter table 表名 change 原名 新名 新类型;
   - ex: alter table mytable change oldName newName varchar(10);

---------------------------------

### 数据（data）操作相关SQL语句
1. 插入
   - 全表插入格式: insert into 表名 values(值1, 值2, 值3);
   - ex: insert into mytable values(value1, value2, value3);
   - 指定字段插入格式: insert into 表名(字段1, 字段2) valus(值1, 值2);
   - ex: insert into mytable(name1, name2) values(value1, value2);
   - 批量插入格式: insert into 表名 values(值1, 值2, 值3),(值1, 值2, 值3), (值1, 值2, 值3);
   - ex: insert into mytable values(value1, value2, value3),(value1, value2, value3),(value1, value2, value3);

2. 查询
   - 格式: select 字段信息 from 表名 where 条件;
   - ex: 
       1. select name form person;
       2. select name, age from person;
       3. select * from person;
       4. select name from person where id = 1;

3. 修改
   - update 表名 set 字段名=值 where 条件;
   - ex: update mytable set name1=value1 where id=2;

4. 删除
   - delete from 表名 where 条件
   - ex:
       1. delete from person where id = 1;
       2. delete from person where id > 10;
       3. delete from person;


