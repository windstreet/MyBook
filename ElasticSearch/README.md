# ElasticSearch

- Api  
删除所有索引 - 慎用 !!!  
```bash
curl -XDELETE 'localhost:9200/*?pretty&pretty'
```

* 安装下载信息 

```bash
brew info elasticsearch

Data:    /usr/local/var/lib/elasticsearch/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_linrenwei.log
Plugins: /usr/local/var/elasticsearch/plugins/
Config:  /usr/local/etc/elasticsearch/
```

* local  
http://127.0.0.1:9200   
http://0.0.0.0:9200/   

* 下载csv     

```bash
es2csv -u http://192.168.1.10:19200 -i crawler-amazon_bad_review_ca -q '*' -o /Users/linrenwei/Desktop/ca.csv
```
