# AMQP协议

- 一个提供统一消息服务的`应用层`标准高级`消息队列协议`。
- 基于此协议的`客户端`与`消息中间件`可传递消息，并不受客户端/中间件不同产品，不同的开发语言等条件的限制。    
- 是一个高级抽象层消息通信协议，RabbitMQ 是 AMQP协议的实现。 

### 一、简介

应用层协议`application layer protocol`：定义了运行在不同端系统上的应用程序进程如何相互传递报文。

应用层协议的定义：
1. 交换的报文类型：如请求报文和响应报文；
2. 各种报文类型的语法：如报文中的各个字段公共详细描述;
3. 字段的语义：即包含在字段中信息的含义；
4. 进程何时、如何发送报文及对报文进行响应。

##### 有些应用层协议是由`RFC文档`定义的，因此它们位于公共领域。             
例如：web的应用层的协议HTTP(超文本传输协议，RFC 2616) 就作为一个RFC 供大家使用。如果浏览器开发者遵从`HTTP RFC规则`，所开发出的浏览器就能访问任何遵从该文档标准的web，服务器并获取相应的web页面。      

##### 还有很多别的应用层协议是专用的，不能随意应用于公共领域。                
例如：`P2P文件共享系统`使用的是专用应用层协议。     


### 二、AMQP模型

1. `Server(broker)`：接受客户端连接，实现AMQP消息队列和路由功能的进程。客户端为:
    - Publisher Application 生产者
    - Consumer Application 消费者

2. `Virtual Host`：其实是一个虚拟概念，类似于权限控制组。
    - 一个Virtual Host 里面可以有若干个Exchange 和 Queue，但是权限控制的最小粒度是Virtual Host。

3. `Exchange`：接受生产者发送的消息，并根据`Binding规则`将消息路由给服务器中的队列。
    - `ExchangeType` 决定了Exchange路由消息的行为，例如，在RabbitMQ中，Exchange Type有direct、Fanout和Topic三种，不同类型的Exchange路由的行为是不一样的。

4. `Message Queue`：消息队列，用于存储还未被消费者消费的消息。

5. `Message`：由Header 和Body 组成。
    - `Header` 是由生产者添加的各种属性的集合，包括Message是否被持久化、由哪个Message Queue接受、优先级是多少等。
    - `Body` 是真正需要传输的APP数据。

6. `Binding`：Binding 联系了Exchange 与 Message Queue。
    - Exchange 在与多个Message Queue 发生Binding 后会生成一张 `路由表`，路由表中存储着MessageQueue 所需消息的限制条件即 `Binding Key`。
    - 当Exchange 收到Message 时会解析其Header 得到 `Routing Key`，Exchange 根据Routing Key 与Exchange Type 将Message 路由到Message Queue。# 依照路由表
    - Binding Key 由Consumer 在Binding Exchange 与Message Queue 时指定，
    - 而Routing Key 由Producer 发送Message 时指定，两者的匹配方式由Exchange Type决定。 

7. `Connection`：连接，对于RabbitMQ而言，其实就是一个位于客户端和Broker 之间的`TCP连接`。

8. `Channel`：信道。仅仅创建了客户端到Broker 之间的连接Connection 后，客户端还是不能发送消息的。
    - 需要为每一个Connection 创建Channel，AMQP协议规定只有通过Channel 才能执行AMQP的命令。
    - 一个Connection 可以包含多个Channel。之所以需要Channel，是因为TCP连接的建立和释放都是十分昂贵的，
    - 如果一个客户端每一个线程都需要与Broker 交互，如果每一个线程都建立一个TCP连接，暂且不考虑TCP连接是否浪费，就算操作系统也无法承受每秒建立如此多的TCP连接。
    - RabbitMQ 建议客户端`线程之间不要共用Channel`，至少要保证共用Channel 的线程发送消息必须是串行的，但是建议尽量共用Connection。
 
9. `Command`：AMQP的命令，客户端通过Command完成与AMQP服务器的交互来实现自身的逻辑。
    - 例如：在RabbitMQ中，客户端可以通过 publish命令发送消息，txSelect开启一个事务，txCommit提交一个事务。



### 三、AMQP的协议栈

AMQP协议本身包括三层：

1. `Module Layer 模块层`：位于协议最高层，主要定义了一些供客户端调用的命令，客户端可以利用这些命令实现自己的业务逻辑。
    - 例如：客户端可以通过`queue.declare`声明一个队列，利用`consume命令`获取一个队列中的消息。

2. `Session Layer 会话层`：主要负责将客户端的命令发送给服务器，再将服务器端的应答返回给客户端。主要为客户端与服务器之间通信提供可靠性、同步机制和错误处理。

3. `Transport Laye 传输层`：主要传输二进制数据流，提供帧的处理、信道复用、错误检测和数据表示。
