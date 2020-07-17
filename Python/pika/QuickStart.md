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


## 4、相关参数

##### 4.1、`no_ack=False`
如果消费者遇到情况（`its channel is closed, connection is closed, or TCP connection is lost`）挂掉了，那么如何使得`RabbitMQ`重新将该任务添加到队列中？

只需2步：
- 回调函数中添加：`ch.basic_ack(delivery_tag=method.delivery_tag)`
- `channel.basic_comsume`方法中的参数设置为：`no_ack=False`

例如：
```python
def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)
    
    # 这里故意停止10秒使得断开连接
    import time
    time.sleep(10)
    print 'ok'
    
    ch.basic_ack(delivery_tag = method.delivery_tag)

channel.basic_consume(callback, queue='hello', no_ack=False)
```

##### 4.2、`durable`消息不丢失
```python
# 对于生产者 or 消费者
channel.queue_declare(queue='hello', durable=True)  # make message persistent

# 对于消费者
channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello World!',
                      properties=pika.BasicProperties(
                          delivery_mode=2, # make message persistent
                      ))
```

##### 4.3、消息获取顺序
- 默认消息队列里的数据是按照顺序被消费者拿走。
- 例如：`消费者1` 去队列中获取 `奇数` 序列的任务，`消费者2` 去队列中获取 `偶数` 序列的任务。
- `channel.basic_qos(prefetch_count=1)` 表示谁来谁取，不再按照奇偶数排列。

```python
# 消费者
channel = connection.channel()
channel.queue_declare(queue='hello')
channel.basic_qos(prefetch_count=1)
# .
# .
# .
```
