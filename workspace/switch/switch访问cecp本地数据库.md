# switch访问cecp本地数据库

### 1、权限问题

- 数据库中执行:
```sql
INSERT INTO "public"."oauth2_client"("id","name","description","client_id","client_secret","_redirect_uris","_default_scopes","is_confidential","user_id")
VALUES
(37,E'SWITCH',NULL,E'aYh5U1dFMAoUImciUHGAtsPx4jpa6atKpkpHgGEI',E'IRgrVNA7nYRJNpuj6zI0buG10wf0tkvGXTkVHCJw',NULL,NULL,TRUE,20);
```

- 手动修改用户权限

![01](/workspace/switch/img/switch访问cecp本地数据库-01.png)
![02](/workspace/switch/img/switch访问cecp本地数据库-02.png)

- switch的配置文件设置    

```python
# api/settings/local_development.py

# cecp
CECP_DOMAIN = 'http://0.0.0.0:8080/'

# aide
AIDE_OAUTH_BASE_URL = ''
AIDE_OAUTH_CLIENT_ID = ''
AIDE_OAUTH_CLIENT_SECRET = ''

# cecp
CECP_OAUTH_BASE_URL = "http://0.0.0.0:8080/"
CECP_OAUTH_CLIENT_ID = "aYh5U1dFMAoUImciUHGAtsPx4jpa6atKpkpHgGEI"
CECP_OAUTH_CLIENT_SECRET = "IRgrVNA7nYRJNpuj6zI0buG10wf0tkvGXTkVHCJw"

```