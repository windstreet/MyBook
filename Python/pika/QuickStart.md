# QuickStart

## 1、创建`connection`对象
```python
import os
import pika

RABBITMQ_USER = os.getenv('RABBITMQ_MASTER_USER', 'guest')
RABBITMQ_PASSWORD = os.getenv('RABBITMQ_MASTER_PASS', 'guest')
RABBITMQ_CREDENTIALS = pika.PlainCredentials(username=RABBITMQ_USER, password=RABBITMQ_PASSWORD)
RABBITMQ_HOST = os.getenv('RABBITMQ_MASTER_HOST', '127.0.0.1')
RABBITMQ_PORT = int(os.getenv('RABBITMQ_MASTER_PORT', 5672))
RABBITMQ_VHOST = os.getenv('RABBITMQ_VHOST', 'demo')  # 需要手动去创建一个名为 `demo` 的 `vhost`
RABBITMQ_USE_SSL = False
RABBITMQ_SSL_OPTIONS = {
    # 'certfile': '/etc/hmt/certs/client/cert.pem',
    # 'keyfile': '/etc/hmt/certs/client/key.pem'
}
RABBITMQ_CONFIGS = dict(host=RABBITMQ_HOST,
                        port=RABBITMQ_PORT,
                        credentials=RABBITMQ_CREDENTIALS,
                        ssl_options=RABBITMQ_SSL_OPTIONS,
                        ssl=RABBITMQ_USE_SSL,
                        virtual_host=RABBITMQ_VHOST
                        )
                        
parameters = pika.ConnectionParameters(**RABBITMQ_CONFIGS)
connection = pika.BlockingConnection(parameters)
```

## 2、生产者
```python
# 创建对象
channel = connection.channel()

# 声明一个名为 `hello` 的队列
channel.queue_declare(queue='hello')

# 插数据
channel.basic_publish(exchange='',          # 交换机
                      routing_key='hello',  # 指定的队列名称
                      body='Hello Yuan!')   # 值

print(" [x] Sent 'Hello World!'")
connection.close()  # TODO 或者 `channel.close()` ?
```
>注意：    
在简单模式中（`channel.basic_publish`），是没有交换机的。所以`exchange`参数的值为空。
