# Docker

```bash

# 罗列所有docker服务  
docker service ls 
>>>
ID                  NAME                               MODE                REPLICAS            IMAGE                                  PORTS
pjl8dhnaovha        goblin_ESInsertTask                replicated          1/1                 basement/goblin:74ebbaa2b69b5124e4a6   
zhz87cnqwn3r        goblin_ProxyCrawlHttpRequest       replicated          1/1                 basement/goblin:74ebbaa2b69b5124e4a6   

# 查看某一docker服务的日志
docker service logs zhz87cnqwn3r		

# 拉取网上镜像
docker pull ubuntu:16.04		

# 查看本地镜像
docker images		

# 查看本地正在跑的服务
docker ps
```
