# 数据库`database`

##### 1、查看
```psql
\l
```

##### 2、创建
```sql
CREATE DATABASE database_name;
```
>注意：     
1）database_name 要创建的数据库的名称。      
2）数据库名字不能以数字开头。

##### 3、删除
```sql
DROP DATABASE database_name;
```


##### 4、切换
```psql
\c database_name
\connect database_name
```