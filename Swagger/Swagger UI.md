# Swagger UI

- `Swagger UI`是一个API在线文档生成和测试的框架。
- 源码：[https://github.com/swagger-api/swagger-ui](https://github.com/swagger-api/swagger-ui)

### 1、本地部署 or 环境搭建

1. 下载Swagger UI        
    ```bash
    git clone https://github.com/swagger-api/swagger-ui.git
    ```
    - 主要用到的是`dist目录`，可以进入dist目录打开`index.html`看下界面，可以发现基本的模式是有了，但都是静态的文件。
    - 下面我们要进行`nodejs配置`，使其可以进行端口访问，直接使用node命令访问`index.js`没有反应，接下来手动配置ui环境。

2. 创建一个空文件夹（node app 文件夹）
    ```bash
    mkdir swagger
    ```

3. 初始化，会自动创建`package.json`文件
    ```bash
    npm init
    ```

4. 安装express      
    ```bash
    npm install express --save
    ```

5. 在swagger中创建`目录public`，并将刚才clone下来的Swagger UI中`dist目录`下的所有文件全部复制到`public目录`下面。

6. 创建并修改`vim swagger.js`
    ```js
    var express = require('express');
    var app = express();
    var http = require('http');

    // 接口显示页面
    app.use('/static', express.static('public'));
    app.listen(3000, function () {
        console.log('app listening on port 3000!');
    });
    
    ```

7. 启动服务
    ```bash
    cd swagger/
    node swagger.js
    ```
    打开[http://127.0.0.1:3000/static/index.html](http://127.0.0.1:3000/static/index.html)，可以看到在线的官方的Demo已经在本地搭建好了。

---

参考：    
[swagger-ui生成api文档并进行测试](https://www.cnblogs.com/shamo89/p/7681085.html)

[让接口测试成为合格的桥梁——本地搭建 Swagger-UI 环境搭建](https://testerhome.com/topics/8168)