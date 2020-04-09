# ssh

- 查看公钥内容

```bash
cat ~/.ssh/id_rsa.pub
```

- 记录

```bash
ssh -NR 0.0.0.0:4999:0.0.0.0:8000 root@114.116.23.219 -vv
```

- 密钥权限过大错误  

别人那边拷贝过来的密钥，很多时候无法直接登录ssh，会报错 `permissions are too open`。  
这个时候需要修改id_rsa的权限，一般修改为600就好。  

```bash
chmod 400 ~/.ssh/id_rsa
```
