# SQL教程

### 一、什么是SQL？

- Structured Query Language，结构化查询语言。

- 简单地说，SQL就是访问和处理关系数据库的计算机标准语言。

- 不同的数据库，都支持SQL，通过SQL这一种语言，就可以操作各种不同的数据库。

- 无论用什么编程语言（Java、Python……）编写程序，只要涉及到操作关系数据库，都必须通过SQL来完成。


### 二、SQL语言定义了这几种操作数据库的能力

- DDL：Data Definition Language
    - DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。
    - 通常，DDL由`数据库管理员`执行。

- DML：Data Manipulation Language
    - DML为用户提供添加、删除、更新数据的能力。
    - 这些是`应用程序`对数据库的日常操作。

- DQL：Data Query Language
    - DQL允许`用户`查询数据，这也是通常最频繁的数据库日常操作。


### 三、语法特点
- SQL语言关键字`不区分`大小写。

- 但是，针对不同的数据库，对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写。同一个数据库，有的在Linux上区分大小写，有的在Windows上不区分大小写。

- 本教程约定：SQL关键字总是大写，以示突出；表名和列名均使用小写。


### 二、NoSQL

- 非SQL的数据库，包括MongoDB、Cassandra、Dynamo等等，它们都不是关系数据库。

- NoSQL数据库是作为SQL数据库的补充。



---
[廖雪峰-SQL教程](https://www.liaoxuefeng.com/wiki/1177760294764384)

[参考](https://www.yiibai.com/postgresql)
