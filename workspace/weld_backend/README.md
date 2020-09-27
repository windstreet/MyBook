# 快速开始

---

# 项目与环境

Python 版本: 2.7

### 1、克隆项目
```bash
cd ~/Desktop/workplace/
git clone https://*********/weld_backend.git 
cd weld_backend/api
```

### 2、环境与依赖
```bash
# mkvirtualenv weld_backend --python=/System/Library/Frameworks/Python.framework/Versions/2.7		# 系统自带的		
conda create --name weld_backend python=2.7 
pip install -r requirements.txt
# conda install
```

### 3、添加 `local_development.py` 文件
```python
# ~/Desktop/workplace/weld_backend/api/bapi/settings/local_development.py	

# local_development 文件中修改为你本地的数据库（数据库没有的话要自己先创建）
SQLALCHEMY_DATABASE_URI = 'postgresql://:@127.0.0.1:5432/weld_backend'	
```

---

# 数据库相关


### 1、导入数据库到本地
```bash
pg_restore --job=4  /Users/admin/Desktop/weld_0326_local.pg_dump  -d weld_backend -O
```
>`/Users/admin/Desktop/weld_0326_local.pg_dump` 为数据库文件（含所在目录）     
`weld_backend` 为本地数据库名称（自己起，先创建一个空的数据库）     


### 2、shell中操作数据库
```bash
python manage.py shell
```

```python
db.session.query(User.id).all()
db.session.rollback()  # 回滚（报错记得回滚）
User.query.filter(User.id==1).first()
db.session.rollback()

# 创建修改账号密码
u = User.query.filter(User.id ==1).one()
u.email  # 账号
u.change_password('admin')  # 新密码
u.status = 9  # 设为超级管理员账号
db.session.commit()  # 提交后才生效
```


### 3、数据库迁移
```bash
# 生成migration
python manage.py db migrate -m "test....."


# 数据库升级
python manage.py db upgrade head  # 升到最新版本


# 数据库降级
python manage.py db downgrade -1  # 降一个版本
```
>数据迁移注意事项
> * 新的分支一般从 `master` 切：首先切到 master 分支，`git pull` 最新代码；
> * 然后 `python manage.py db upgrade head` 将数据库升级到最新版本；
> * 最后再从 master 切出你待开发的分支：`git checkout -b feature/test`
> * 假如你的新分支涉及到数据库表单的修改，可以先在自己的分支里创建数据库迁移脚本：`python manage.py db migrate -m "test....."`（注意，这个新建的迁移脚本不要提交上去！！！）
> * 然后再升级你的数据库：`python manage.py db upgrade head`
> * 开发完后记得给你数据库降级：`python manage.py db downgrade -1`


---

# Pycharm 配置

#### `Run/Debug Configurations`配置
```
1.1）8080

name: 8080
Script path: /Users/admin/Desktop/workplace/weld_backend/api/manage.py
parameters: runserver -h 0.0.0.0 -p 8000
Environment variables: PYTHONUNBUFFERED=1
Python interpreter: 选择相应虚拟环境的python
Working directory: /Users/admin/Desktop/workplace/weld_backend/api



1.2）8080 gunicorn

name: 8080 gunicorn
Script path: /Users/admin/.virtualenvs/weld_backend/bin/gunicorn
parameters: -b 0.0.0.0:8080 --reload --log-level=debug wsgi:app -c guniconf.debug.py
Environment variables: PYTHONUNBUFFERED=1
Python interpreter: 选择相应虚拟环境的python
Working directory: /Users/admin/Desktop/workplace/weld_backend/api
```


---

# 启用 celery 服务

```bash
# 异步任务相关
cd ~/Desktop/workplace/weld_backend/api
celery -A celery_app.celery worker


# 导出任务相关
celery -A celery_app.celery worker -Q celery.export_task

# 注意 celey 依赖 rabbitmq 和 redis
brew services restart rabbitmq 
brew services restart redis
```


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

---

# 对接前端

#### 1、获取自己的ip给前端
```bash
ifconfig en0

#en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
#	options=6b<RXCSUM,TXCSUM,VLAN_HWTAGGING,TSO4,TSO6>
#	ether e0:d5:5e:1e:bd:ba
#	inet6 fe80::c89:1a4e:9c1:107c%en0 prefixlen 64 secured scopeid 0x5
#	inet 10.1.1.118 netmask 0xfffffe00 broadcast 10.1.1.255
#	nd6 options=201<PERFORMNUD,DAD>
#	media: autoselect (1000baseT <full-duplex,flow-control>)
#	status: active

# 这里 `10.1.1.118` 就是
```


#### 2、将本地的 cecp 服务端口给前端
```
http://0.0.0.0:8080/		# 8080 端口
```

#### 3、修改数据库配置
表 `oauth2_client` 的 `_redirect_uris` 字段修改为 目标 `前端的ip` 地址：

![对接前端](/workspace/weld_backend/img/front.png) 
