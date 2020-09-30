# `jdy`通知消息到本地`switch`

### 第一步：
![01](/workspace/switch/img/jdy通知消息到本地switch-01.png)

### 第二步：
![02](/workspace/switch/img/jdy通知消息到本地switch-02.png)

```
http://cecp-test.eastwestec.com:4999/notification/jiandaoyun/index

http://cecp-test.eastwestec.com:8089/notification/jiandaoyun/index  # 测试环境的switch
```

### 第三步：

```bash
# 获取我本机的 `ssh key` 给管理员
cat ~/.ssh/id_rsa.pub

# 将测试环境的 4999 端口映射到本机 switch 服务器的 8000 端口
ssh -NR 0.0.0.0:4999:0.0.0.0:8000 root@114.116.23.219 -vv   
```
