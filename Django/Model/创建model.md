# 创建model

```markdown
# django model field
Field.null:    如果设置为 True， 当该字段为空时，Django 会将数据库中该字段设置为 NULL，默认为 False。
Field.blank:   如果设置为 True ，该字段允许为空。默认为 False 。
```

## 详解 `blank` 和 `null`

- blank - 用于表单的认证，被设为 `blank=False（默认为False）` 的字段在填写表单时不能为空。  
- null - 用于规定数据库中的列的非空性，被设为 `null=False（默认为False）` 的字段在数据库中对应的列不能为空（用SQL来说明就是为该列添加了NOT NULL的约束)。


#### `blank` 和 `null` 总共有四种设定组合

| blank | null | 意义 |
|-------|-------|-------|
| True | True | 该字段可以为空。 |
| False | False | 该字段不可以为空。 |
| True | False | 某些字段并不希望用户在表单中创建（如slug），而是通过在save方法中根据其他字段生成。 |
| False | True | 不允许表单中该字段为空，但是允许在更新时或者通过shell等非表单方式插入数据该字段为空。 |


#### 注意
为什么在只设定了 `blank=True` 而没有设定 null=True 的时候，通过表单创建模型实例并且表单在该字段上没有值时数据库不报错呢
（当没有设定null=True时，该列在数据库中就存在NOT NULL的约束，如果插入数据时这一列没有值，按理说数据库应该会报错才对）

> 出现这种情况的原因在于，django在处理某些在数据库中实际的存储值为字符串的Field时（如CharField, TextField, ImageField（图片文件的路径）），永远不会向数据库中填入空值。   
如果表单中某个CharField或者TextField字段为空，那么django会在数据库中填入""，而不是null. 

