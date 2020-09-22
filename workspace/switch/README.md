# 快速开始

`switch`作为中间件对接`简道云`和`cecp`

---

## 太久没更新项目可能遇到奇怪的异常

```
InvalidRequestError: Table 'auth_system' is already defined for this MetaData instance.  
Specify 'extend_existing=True' to redefine options and columns on an existing Table object.
```

```bash
# 清一下 *.pyc 文件
find ~/workplace/switch/api -name "*.pyc" | xargs rm -rf
```

---

## 关于一些配置
- 在 switch 的配置中
    ```python
    # auth info 加密
    AUTH_INFO_KEY = 'test'
    ```

- 在 cecp 的配置中
    ```python
    # 简道云标识key
    MIDDLEWARE_JIANDAOYUN_AUTH_KEY = 'jianw*************XA'
    ```
    
    要和 `switch` 数据库中的 `auth_system` 表的某一对象的 `auth_token` 字段值一致，       
    且该对象所关联的 `application` 表的对象的 `name` 值要为 `cecp`。           
    假如不存在关联，就自己创建一下。                   
    `1	cecp			2019-11-12 02:44:01.10714-08	2019-11-12 02:44:01.10714-08`

- 修改 switch 数据库         
    上述的 `auth_system` 实例对象，需要进一步修改。     
    ```bash
    cd ~/workplace/switch/api && conda activate switch
    python manage.py shell
    ```
    
    ```python
    obj = AuthSystem.query.all()[-1]
    obj.data = {u'api_key': u'DvSZ5************PXX'}
    db.session.add(obj) 
    db.session.commit()
    ```

---

## 启动celery服务

```bash
cd cd ~/workplace/switch/api && conda activate switch


# 开4个进程分别执行
celery -A celery_app.celery worker -c 1 -Q celery

celery -A celery_app.celery worker -c 1 -Q celery_high

celery -A celery_app.celery worker -c 1 -Q celery.watchdog --prefetch-multiplier=1 -n celery-watch.%h

celery -A celery_app.celery beat --pidfile= -s /tmp/celery-schedule
```




