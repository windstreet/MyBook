# sqlalchemy query 

___注意：貌似只有字段类型的属性才可以作为查询条件___

## 1、不为空 `.isnot(None)`
```python
skus = SKU.query.join(Product).filter(Product.reference_cost.isnot(None)).all()
```

## 2、联表 `.join()`

```python
query.join(Address, User.id==Address.user_id)    # explicit condition
query.join(User.addresses)                       # specify relationship from left to right
query.join(Address, User.addresses)              # same, with explicit target
query.join('addresses')				# 表名 

a = PurchaseDemand.query.join(SKU).filter(PurchaseDemand.sku_id == SKU.id, SKU.sku == "TN120492BL").all()
```

联表
```python
# join
# union
# concat
ret1 = session.query(UserInfo, Department).filter(UserInfo.department_code == Department.id).all()
ret2 = session.query(UserInfo).join(Department).all()
ret3 = session.query(UserInfo).join(Department, isouter=True).all()
```


## 3、条件查询（这些都可以混合起来用）

### 3.1、大于小于等于和python语法一样

### 3.2、传入两个值查询介于两个值之间的。`between`    
```python
SKU.query.filter(SKU.id.between(2, 4)).all()    # 含2和4
```                       

### 3.3、传入列表/元组，查询满足在序列中的值。 `in_`       
```python
SKU.query.filter(SKU.id.in_([2, 4])).all()
```                  

### 3.4）取反 `~`     
```python
SKU.query.filter(~SKU.id.in_([2, 4])).all()
```                                                                      

### 3.5）且 `and_ `       
```python
SKU.query.filter(and_(SKU.id.between(2, 4), SKU.id.in_([2, 8]))).all()
```                                       

### 3.6）或 `or_` 

```python
from sqlalchemy import and_, or_

res = session.query(UserInfo).filter(
    or_(
        UserInfo.id < 3,
        and_(UserInfo.username == 'egon', UserInfo.id > 0),
        UserInfo.extra != ""
    )).all()
```

### 3.7）通配符 `like`，模糊搜索

```python
ret1 = session.query(UserInfo).filter(UserInfo.username.like('e%')).all()
ret2 = session.query(UserInfo).filter(~UserInfo.username.like('e%')).all()
```

### 3.8）排序 `order_by`
* asc  正序  
* desc 倒序  
* 多个排序则在前一个排序无法区分时使用下一个排序  

```python
ret1 = session.query(UserInfo).order_by(UserInfo.username.desc()).all()
ret2 = session.query(UserInfo).order_by(UserInfo.username.desc(), UserInfo.id.asc()).all()
```

### 3.9）分组 `group_by`

```python
from sqlalchemy.sql import func

ret1 = session.query(
    func.max(UserInfo.id),
    func.sum(UserInfo.id),
    func.min(UserInfo.id)).group_by(UserInfo.department_code).having(func.min(UserInfo.id) > 0).all()
```

### 3.10）组合 `union`，`union_all`

```python
q1 = session.query(UserInfo.username).filter(UserInfo.id > 2)
q2 = session.query(Department.title).filter(Department.id < 2)
ret4 = q1.union(q2).all()
		
q1 = session.query(Department.title).filter(UserInfo.id > 2)
q2 = session.query(Hosts.addr.concat(Hosts.name)).filter(UserInfo.id < 2)
ret5 = q1.union_all(q2).all()
```


### 关于 lazy

```
https://medium.com/@ns2586/sqlalchemys-relationship-and-lazy-parameter-4a553257d9ef
lazy='select' (or True)
lazy='dynamic'
lazy='joined' (or False)
lazy='subquery'

https://shomy.top/2016/08/11/flask-sqlalchemy-relation-lazy/
lazy 决定了 SQLAlchemy 什么时候从数据库中加载数据:，有如下四个值:(其实还有个noload不常用)
通俗了说，
select就是访问到属性的时候，就会全部加载该属性的数据。
joined则是在对关联的两个表进行join操作，从而获取到所有相关的对象。
dynamic则不一样，在访问属性的时候，并没有在内存中加载数据，而是返回一个query对象, 需要执行相应方法才可以获取对象，比如.all()。

```


## 根据订单号查询该订单行项目的相关信息

```python
from sqlalchemy.dialects.postgresql import aggregate_order_by
order_id_list = ['026-8177772-3397260']
query = OrderItem.query.with_entities(
        db.func.string_agg(OrderItem.sku, aggregate_order_by(', ', OrderItem.id.desc())).label('msku'),
        Order.platform_order_number,
        Order.amount,
        db.func.string_agg(OrderItem.asin, aggregate_order_by(', ', OrderItem.id.desc())).label('asin'),
        db.func.string_agg(Merchandise.sku_alias, aggregate_order_by(', ', OrderItem.id.desc())).label('sku_alias'),
        db.func.string_agg(SKU.sku, aggregate_order_by(', ', OrderItem.id.desc())).label('sku'),
        db.func.string_agg(SKUCustomsInfo.name_zh, aggregate_order_by(', ', OrderItem.id.desc())).label('name_zh')
    ).outerjoin(
        Order, 
        Order.id == OrderItem.order_id
    ).filter(
        Order.platform_order_number.in_(order_id_list)
    ).outerjoin(
        Merchandise,
        db.and_(OrderItem.account_id == Merchandise.account_id, OrderItem.sku == Merchandise.sku_alias)
    ).filter(
        Merchandise._delete.is_(False)
    ).outerjoin(
        AmazonMerchandise, 
        AmazonMerchandise.id == Merchandise.id
    ).outerjoin(
        Product, 
        Product.id == Merchandise.product_id
    ).outerjoin(
        SKU, 
        SKU.id == Product.sku_id
    ).outerjoin(
        SKUCustomsInfo
    ).group_by(
        Order.platform_order_number,
        Order.amount
    )

query.all()
```



