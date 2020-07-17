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
# 可以使用 connection 创建多个 channel
# c1 = connection.channel()
# c2 = connection.channel()
```

## 2、生产者（简单模式）
producer.py
```python
# 创建对象
channel = connection.channel()

# 声明一个名为 `hello` 的队列
channel.queue_declare(queue='hello')

# 插数据
channel.basic_publish(exchange='',          # 交换机
                      routing_key='hello',  # 指定的队列名称
                      body='Hello World!')   # 值

print(" [x] Sent 'Hello World!'")
connection.close()  # TODO 或者 `channel.close()` ?
```
>注意：    
在简单模式中（`channel.basic_publish`），是没有交换机的。所以`exchange`参数的值为空。

## 3、消费者（简单模式）
consumer.py
```python
channel = connection.channel()

# 声明一个名为 `hello` 的队列
channel.queue_declare(queue='hello')


# 回调函数
def callback(ch, method, properties, body):
    print(" Received %r" % body)


channel.basic_consume(callback,
                      queue='hello',
                      no_ack=True)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```
>注意：     
这里生产者和消费者都创建了同名的队列，其实谁来创建队列，都无所谓，只要保证队列存在就可以了！    
这样无论两者谁先执行都不会报错。
