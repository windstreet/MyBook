# ElasticSearch

- **Api**  
删除所有索引 - 慎用 !!!  
```bash
curl -XDELETE 'localhost:9200/*?pretty&pretty'
```

- **安装下载信息** 

```bash
brew info elasticsearch

Data:    /usr/local/var/lib/elasticsearch/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_linrenwei.log
Plugins: /usr/local/var/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/
```

- **local**  
http://127.0.0.1:9200   
http://0.0.0.0:9200/   


- **将远程的 es 映射到本地端口**  
   
```bash
proxychains4 ssh -NL 9600:172.31.0.48:9500 root@47.96.95.35

```

>其中：   
    proxychains4 嵌套一层代理，访问境外地址会更快速、稳定。     
    映射到本地 `9600` 端口
