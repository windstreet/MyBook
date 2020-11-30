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


```sql
-- 也可以指定一些默认条件，比如创建一个所有者为postgres、编码使用utf8、连接数限制为1000的数据库。
CREATE DATABASE my_db
  WITH OWNER = postgres
       ENCODING = 'UTF8'
       CONNECTION LIMIT = 1000;
```

##### 3、修改
```sql
CREATE DATABASE my_db;
ALTER DATABASE my_db RENAME TO my_test; -- 改名
ALTER DATABASE my_test OWNER TO postgres;  -- 改数据库所有者
ALTER DATABASE my_test WITH CONNECTION LIMIT = 100;  -- 改连接数
```

##### 4、删除
```sql
DROP DATABASE database_name;
```
>注意：
- DROP语句只能删除空闲状态的数据库，如果数据库下在使用或正在恢复或处理其它工作状态，则会删除失败。    
- 使用SQL语句删除时不会有任命提示，所以使用DROP语句时要非常小心，确保相关数据库已无效或已备份。
- 删除所有数据库后会导致数据库服务器无法使用，所以不要删除所有数据库。


##### 5、切换
```psql
\c database_name
\connect database_name
```