# ssh

- 查看公钥内容

```bash
cat ~/.ssh/id_rsa.pub
```

- 记录

```bash
ssh -NR 0.0.0.0:4999:0.0.0.0:8000 root@114.116.23.219 -vv
```
