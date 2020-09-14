# 快速开始

---

# 外部数据源说明

### 环境说明

本地环境需要装ODBC驱动，可以在 https://www.dremio.com/drivers/ 下载

mac还需要

```bash
brew install unixodbc
```

装好以上两个依赖后，将

`/Library/Dremio/ODBC/Setup` 下的两个文件，复制到  `/usr/local/etc` 下覆盖源文件

配置中需增加 `SQLALCHEMY_BINDS`

参考以下配置:

```python
SQLALCHEMY_BINDS = {
    'master': SQLALCHEMY_DATABASE_URI,
    'slave': SQLALCHEMY_DATABASE_URI,
    'dremio': 'xxxxxx'  # 具体值去项目文档里查找
}
```

---

# 启动后台

#### 1、启动服务

```bash
python manage.py runserver  # 默认 http://127.0.0.1:5000/ 
```

![后台](/workspace/weld_backend/img/backend.png)

#### 2、更改用户密码

```bash
python manage.py shell
```
    
```python
obj = db.session.query(User).filter(User.id == 1).one()
obj.email  # admin@admin.com
obj.change_password("admin")
db.session.commit()
# 注意：要 commit 生效；然后重启服务。
```

#### 3、后台登录

![登录](/workspace/weld_backend/img/login.png)
