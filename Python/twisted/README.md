# twisted 入门

- `Twisted`应用的基本问题，可说是 `一个中心，两个基本点`，即： 
    - 以 `事件 event` 为中心。
    - 以 `建立连接 connect` 和 `定义反馈 callback` 为基本点。 

在这些问题上，Twisted有一整套固定的路数，只能照章行事，没有自由发挥的余地。

### 一、一个中心
- Twisted 官方说，“ Twisted is an event-driven networking framework ”。事实的确如此。
- 从其运行机制上看，event 是 Twisted 运转的引擎，是发生各种动作的启动器，是牵一发而动全身的核心部件。
- 从其架构组成上看，它是紧密围绕event设计的；它的具体应用 application，主要是定义、实现各式各样的event，由此完成不同网络协议的连接和输入输出任务，满足用户的实际需求；
- 从其 application的文本形式上，可以直接看到，它的应用程序，基本上由一系列event构成。 
- 由此可见，说它以event为中心，符合实际情况。 
- Twisted 对event 的管理机制，可划分为后台和前台两种形式。 

---

[twisted的入门讲解](http://blog.csdn.net/meiliangdeng1990/article/details/51007143)

[Twisted API 文档](http://twistedmatrix.com/documents/16.4.0/api/twisted.internet.defer.html)  

[接口文档](http://twistedmatrix.com/documents/current/api/twisted.html)

[Twisted 之 reactor](http://blog.csdn.net/chenjiayi_yun/article/details/8885589)
