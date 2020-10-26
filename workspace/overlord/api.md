# overlord API

### 查询
```
curl -H "Authorization: Bearer 147258369" -X GET http://localhost:8000/api/bases/resources/AsinInfo/subscriptions/ | json
```

### 创建（订阅为例）
```
curl -H "Authorization: Bearer 147258369" -H "Content-Type:application/json" -X POST http://localhost:8000/api/bases/resources/AsinInfo/asin_info_subscriptions/ --data '{"name": "test curl create", "enabled": true, "trigger_type": 100, "trigger_params": {"crontab": "0 0 1 * *"}, "country_code": "US", "asin": "B01LEEWO7C"}'
```

### 批量创建
```
curl -H "Authorization: Bearer 147258369" -H "Content-Type:application/json" -X POST http://localhost:8000/api/bases/resources/AsinInfo/asin_info_subscriptions/ --data '[{"name": "test curl create 1", "enabled": true, "trigger_type": "100", "trigger_params": {"crontab": "0 0 1 * *"}, "country_code": "US", "asin": "B01LEEWO7C"}, {"name": "test curl create 2", "enabled": true, "trigger_type": "100", "trigger_params": {"crontab": "0 0 1 * *"}, "country_code": "US", "asin": "B01LEEWO7C"}]'
```
>注解
- `-H "Content-Type:application/json"`  控制发送的请求类型为JSON
- `celery -A overlord worker` 记得启动，否则无法触发执行

---

# celery 坑
## 问题：
```
Task pipelines.tasks.sync_pipeline[af4df332-f55f-43c1-ab83-f3b7f83f2745] raised unexpected ...... ConnectionRefusedError: [Errno 61] Connection refused
```
                    
## 解决：
1. sync_pipeline 函數用到的地方全注注釋掉
2. 清空MQ队列
3. 重启服务：`brew services restart redis`  和  `brew services restart rabbitmq`
4. 执行：`celery -A overlord beat`  和  `celery -A overlord.celery_app worker` 
>注意
所有 model 里未设置 `null=True` （默认值为False）的字段，在API接口里都需要传递；`null=True` 时，validate_field 会忽略检测不传递改参数的情况
