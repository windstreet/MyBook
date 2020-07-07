# 表 `table`

##### 查看都有那些表
```psql
\dt
```

##### 创建
```sql
CREATE TABLE student (
    id serial PRIMARY KEY,
    name text,
    age int
);
```
>注意：    
默认`schema`为`public`

##### 删除
```sql
DROP TABLE student;
```
>注意：  
删除有关联的表的时候要两个表同时删除，例如：`DROP TABLE category, post;` 
