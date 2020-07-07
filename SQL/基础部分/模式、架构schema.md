# 模式、架构 `schema`
- 模式(也叫架构)是指定的表的`集合`。      
- 它还可以包含视图，索引，序列，数据类型，运算符和函数。       
- 模式不能嵌套。

##### 创建
```sql
CREATE SCHEMA schema_name;
```

##### 在`schema`中创建表
```sql
CREATE TABLE schema_name.table_name (
    id serial PRIMARY KEY,
    name text,
    age int
);
```

##### 删除
```sql
DROP SCHEMA schema_name;
```

使用架构的优点：
1. 模式有助于多用户使用一个数据库，而不会互相干扰。
2. 它将数据库对象组织成逻辑组，使其更易于管理。
3. 可以将第三方模式放入单独的模式中，以避免与其他对象的名称相冲突。
