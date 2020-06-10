# 视图 view 
- 在PostgreSQL中，视图（VIEW）是一个`伪表`。   
- 它不是物理表，而是作为普通表选择查询。   
- 视图也可以表示连接的表。    
- 它可以包含表的所有行或来自一个或多个表的所选行。

## 视图主要用途
- 它以自然和直观的方式构建数据，并使其易于查找。
- 它限制对数据的访问，使得用户只能看到有限的数据而不是完整的数据。
- 它归总来自各种表中的数据以生成报告（多表合一）。

## 创建视图

可以从单个表，多个表以及另一个视图创建它： 
```sql
-- 加 TEMP | TEMPORARY 表示该视图是临时的，关闭数据库后不会保存到数据库
CREATE [TEMP | TEMPORARY] VIEW view_name AS  
SELECT column1, column2.....  
FROM table_name  
WHERE [condition];

```

示例：
```sql
CREATE VIEW view_of_quote_table AS  
SELECT author, quote 
FROM quote_table;
```

像正常表一样查询视图：
```sql
SELECT * FROM view_of_quote_table;
```


## 删除视图
```sql
DROP VIEW view_name;
```
