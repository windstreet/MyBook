# CURL

```bash
# 新建名为 `weather` 的 Index
curl -X PUT 'localhost:9200/weather' | json


# 查看索引信息
curl -X GET 'http://localhost:9200/weather' | json


# 删除以 `weather_` 开头的索引
curl -X DELETE 'http://localhost:9200/weather_*' | json


# 查看当前节点的所有 Index
curl -X GET 'http://localhost:9200/_cat/indices?v'

```