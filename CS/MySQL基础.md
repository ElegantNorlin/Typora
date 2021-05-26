---
title: MySQL基础
date: 2021-3-25 20:29:00
categories: 数据库
image: /img/p11.jpg
tags: 博客
---

### DDL操作数据库

* 创建数据库

  ```mysql
  -- 直接创建数据库 db1
  create database db1;
  -- 判断是否存在，如果不存在则创建数据库 db2
  create database if not exists db2;
  -- 创建数据库并指定字符集为 utf-8
  create database db3 default character set utf8;
  ```

* 查看数据库

  ```mysql
  -- 查看所有的数据库
  show databases;
  -- 查看某个数据库的定义信息
  show create database db3;
  show create database db1;
  ```

* 修改数据库

  ```mysql
  -- 将 db3 数据库的字符集改成 utf8
  alter database db3 character set utf8;
  ```

* 删除数据库

  ```mysql
  -- 删除 db2 数据库
  drop database db2;
  ```

* 使用数据库

  ```mysql
  -- 查看正在使用的数据库
  select database();
  -- 改变要使用的数据库
  use db4;
  ```

### DDL操作表结构

* 创建表

  ```mysql
  -- 创建 student 表包含 id,name,birthday 字段
  create table student (
      id int, -- 整数
      name varchar(20), -- 字符串
      birthday date -- 生日，最后没有逗号
  );
  ```

* 查看表

  ```mysql
  -- 查看 day21 数据库中的所有表
  use day21;
  show tables;	
  
  -- 查看 student 表的结构
  desc student;
  
  -- 查看 student 的创建表 SQL 语句
  show create table student;
  
  -- 快速创建一个表结构相同的表: CREATE TABLE 新表名 LIKE 旧表名;
  create table s1 like student;
  desc s1;
  ```

* 删除表

  ```mysql
  -- 直接删除表 s1 表
  drop table s1;
  
  -- 判断表是否存在并删除 s1 表
  drop table if exists `create`;
  ```

* 修改表结构

  ```mysql
  -- 添加表列 ADD
  ALTER TABLE 表名 ADD 列名 类型;
  
  -- 修改列类型 MODIFY
  ALTER TABLE 表名 MODIFY 列名 新的类型;
  -- 将 student 表中的 remark 字段的改成 varchar(100)
  alter table student modify remark varchar(100);
  
  -- 修改列名 CHANGE
  ALTER TABLE 表名 CHANGE 旧列名 新列名 类型;
  -- 将 student 表中的 remark 字段名改成 intro，类型 varchar(30)
  alter table student change remark intro varchar(30);
  
  -- 删除列 DROP
  ALTER TABLE 表名 DROP 列名;
  -- 删除 student 表中的字段 intro
  alter table student change remark intro varchar(30);
  
  -- 修改表名
  RENAME TABLE 表名 TO 新表名;
  -- 将学生表 student 改名成 student2
  rename table student to student2;
  
  -- 修改字符集 character set
  ALTER TABLE 表名 character set 字符集;
  -- 将 student2 表的编码修改成 gbk
  alter table student2 character set gbk;
  ```

### DML操作表中的数据

* 插入记录

  ```mysql
  -- 插入所有的列，向学生表中
  insert into student (id,name,age,sex) values (1, '孙悟空', 20, '男');
  insert into student (id,name,age,sex) values (2, '孙悟天', 16, '男');
  
  -- 插入所有列
  insert into student values (3, '孙悟饭', 18, '男', '龟仙人洞中');
  -- 如果只插入部分列，必须写列名
  insert into student values (3, '孙悟饭', 18, '男');
  select * from student;
  ```

* 更新表记录

  ```mysql
  -- 不带条件修改数据，将所有的性别改成女
  update student set sex = '女';
  -- 带条件修改数据，将 id 号为 2 的学生性别改成男
  update student set sex='男' where id=2;
  -- 一次修改多个列，把 id 为 3 的学生，年龄改成 26 岁，address 改成北京
  update student set age=26, address='北京' where id=3;
  ```

* 删除表记录

  ```mysql
  -- 不带条件删除数据
  DELETE FROM 表名;
  -- 带件删除数据
  DELETE FROM 表名 WHERE 字段名=值;
  -- 使用 truncate 删除表中所有记录
  TRUNCATE TABLE 表名;
  -- truncate 和 delete 的区别：truncate 相当于删除表的结构，再创建一张表。
  
  
  -- 带条件删除数据，删除 id 为 1 的记录
  delete from student where id=1;
  -- 不带条件删除数据,删除表中的所有数据
  delete from student;
  ```

### DQL查询表中的数据

* 简单查询

  ```mysql
  -- 使用*表示所有列
  SELECT * FROM 表名;
  
  -- 查询所有的学生：
  select * from student;
  
  -- 查询指定列的数据,多个列之间以逗号分隔
  SELECT 字段名 1, 字段名 2, 字段名 3, ... FROM 表名;
  
  -- 查询 student 表中的 name 和 age 列
  select name,age from student;
  ```

* 指定列的别名进行查询

  ```mysql
  -- 使用别名
  select name as 姓名,age as 年龄 from student;
  -- 表使用别名(表使用别名的原因：用于多表查询操作)
  select st.name as 姓名,age as 年龄 from student as st
  ```

* 清楚重复值

  ```mysql
  -- 查询学生来至于哪些地方
  select address from student;
  -- 去掉重复的记录
  select distinct address from student;
  ```

* 查询结果参与运算

  ```mysql
  -- 准备数据：添加数学，英语成绩列,给每条记录添加对应的数学和英语成绩，查询的时候将数学和英语的成绩相加
  select * from student;
  -- 给所有的数学加 5 分
  select math+5 from student;
  -- 查询 math + english 的和
  select * from student;
  select *,(math+english) as 总成绩 from student;
  -- as 可以省略
  select *,(math+english) 总成绩 from student;
  ```

* 条件查询

  

  | 比较运算符           | 说明                                                         |
  | -------------------- | ------------------------------------------------------------ |
  | \>、<、<=、>=、=、<> | <>在 SQL 中表示不等于，在 mysql 中也可以使用!= 没有==        |
  | BETWEEN...AND        | 在一个范围之内，如：between 100 and 200 相当于条件在 100 到 200 之间，包头又包尾 |
  | IN(集合)             | 集合表示多个值，使用逗号分隔                                 |
  | LIKE '张%'           | 模糊查询                                                     |
  | IS NULL              | 查询某一列为 NULL 的值，注：不能写=NULL                      |

  MySQL通配符

  | 通配符 | 说明               |
  | ------ | ------------------ |
  | %      | 匹配任意多个字符串 |
  | _      | 匹配一个字符       |

  

  ```mysql
  -- 准备数据创建一个学生表，包含如下列：
  CREATE TABLE student3 (
      id int, -- 编号
      name varchar(20), -- 姓名
      age int, -- 年龄
      sex varchar(5), -- 性别
      address varchar(100), -- 地址
      math int, -- 数学
      english int -- 英语
  );
  INSERT INTO student3(id,NAME,age,sex,address,math,english) VALUES (1,'马云',55,'男','
  杭州',66,78),(2,'马化腾',45,'女','深圳',98,87),(3,'马景涛',55,'男','香港',56,77),(4,'柳岩
  ',20,'女','湖南',76,65),(5,'柳青',20,'男','湖南',86,NULL),(6,'刘德华',57,'男','香港
  ',99,99),(7,'马德',22,'女','香港',99,99),(8,'德玛西亚',18,'男','南京',56,65);
  
  
  
  
  -- 查询 math 分数大于 80 分的学生
  select * from student3 where math>80;
  -- 查询 english 分数小于或等于 80 分的学生
  select * from student3 where english <=80;
  -- 查询 age 等于 20 岁的学生
  select * from student3 where age = 20;
  -- 查询 age 不等于 20 岁的学生，注：不等于有两种写法
  select * from student3 where age <> 20;
  select * from student3 where age != 20;
  
  -- 查询 id 是 1 或 3 或 5 的学生
  select * from student3 where id in(1,3,5);
  -- 查询 id 不是 1 或 3 或 5 的学生
  select * from student3 where id not in(1,3,5);
  
  -- 查询 english 成绩大于等于 75，且小于等于 90 的学生
  select * from student3 where english between 75 and 90;
  
  -- 查询姓马的学生
  select * from student3 where name like '马%';
  select * from student3 where name like '马';
  -- 查询姓名中包含'德'字的学生
  select * from student3 where name like '%德%';
  -- 查询姓马，且姓名有两个字的学生
  select * from student3 where name like '马_';
  ```

  

| 逻辑运算符 | 说明                                   |
| ---------- | -------------------------------------- |
| and 或 &&  | 与，SQL 中建议使用前者，后者并不通用。 |
| or 或 \|\| | 或                                     |
| not 或 !   | 非                                     |

```mysql
-- 查询 age 大于 35 且性别为男的学生(两个条件同时满足)
select * from student3 where age>35 and sex='男';
-- 查询 age 大于 35 或性别为男的学生(两个条件其中一个满足)
select * from student3 where age>35 or sex='男';
-- 查询 id 是 1 或 3 或 5 的学生
select * from student3 where id=1 or id=3 or id=5;
```

