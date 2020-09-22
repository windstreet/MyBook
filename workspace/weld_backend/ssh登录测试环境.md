# ssh登录测试环境

### 1)
```bash
# 前提 cat ~/.ssh/id_rsa.pub 公钥已提交
ssh linrenwei@cecp-test.eastwestec.com
```

### 2)
```bash
docker ps | grep shell
docker exec -it efa2791e8b3f bash  # efa2791e8b3f 来自上一步骤
```

### 3）python manage.py shell
```bash
root@efa2791e8b3f:/code# python manage.py shell
```
