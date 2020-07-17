# exchange模型

## 1、发布订阅 `fanout`

- `发布订阅`和`简单的消息队列`区别在于，发布订阅会将消息发送给所有的订阅者，而简单的消息队列中的数据被消费一次便消失（先到先得）。
- 所以，`RabbitMQ`实现发布和订阅时，会为每一个订阅者分别创建一个队列，而发布者发布消息时，会将消息放置在所有相关队列中。

##### 1.1、生产者
```python
import sys

channel = connection.channel()
channel.exchange_declare(exchange='logs', exchange_type='fanout')  # 关键点

message = ' '.join(sys.argv[1:]) or "info: Hello World!"
channel.basic_publish(exchange='logs',
                      routing_key='',
                      body=message)
print(" [x] Sent %r" % message)
connection.close()
```

##### 1.2、消费者
```python
channel = connection.channel()
channel.exchange_declare(exchange='logs', exchange_type='fanout')  # 也要创建一下交换器，以防消费者先于生产者执行时，导致exchange 未创建而引发异常

result = channel.queue_declare(exclusive=True)
queue_name = result.method.queue

channel.queue_bind(exchange='logs', queue=queue_name)  # 该channel的队列绑定到名为 'logs' 的交换器上；交换器上一有消息就会传给这个消费者的队列（未知名）
# c2.queue_bind(exchange='logs', queue=queue_name)  # 其他消费者的 channel的队列，也可以监听这个交换器，这就是 `fanout - 广播模式`

print(' [*] Waiting for logs. To exit press CTRL+C')

def callback(ch, method, properties, body):
    print(" [x] %r" % body)

channel.basic_consume(callback,
                      queue=queue_name,
                      no_ack=True)

channel.start_consuming()
```
