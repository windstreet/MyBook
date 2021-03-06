# 什么是 `docker-compose` ?

用于管理、编排多个容器。

### 1、作用

- 在线上环境中，通常不会将项目的所有组件放到同一个容器中。
- 更好的做法是：**把每个独立的功能装进单独的容器**，这样方便复用。
- 比如：将 Django 代码放到容器A，将 Mysql 数据库放到容器B，以此类推。
- 因此同一个服务器上有可能会运行着多个容器，如果每次都靠一条条指令去启动，未免也太繁琐了。 
- `Docker-compose` 就是解决这个问题的，它用来编排多个容器，将启动容器的命令统一写到 `docker-compose.yml` 文件中。
- 以后每次启动这一组容器时，只需要 `docker-compose up` 就可以了。

---

### 2、确认是否已安装

```bash
docker-compose -v
# docker-compose version 1.24.1, build 4667896b
```
