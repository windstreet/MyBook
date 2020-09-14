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
