# Swagger Editor

- `Swagger Editor` 是一个开源的编辑器，并且它也是一个基于Angular的成功案例。
- 在 `Swagger Editor` 中，我们可以基于 `YAML` 等语法定义我们的 `RESTful API`，然后它会自动生成一篇排版优美的API文档，并且提供实时预览。
- 简单说就是可以边编写 API 边预览边测试。
- Swagger官方提供了一个Swagger Editor的在线的web版本，[http://editor.swagger.io/#/](http://editor.swagger.io/#/)，当然我们也可以下载到自己的机器在本地运行。


### 1、本地部署（安装）

1. 需要先安装node和npm（略）

2. node中 `http-server` 安装
    ```bash
    npm install -g http-server
    ```
    
3. 下载
    ```bash
    git clone https://github.com/swagger-api/swagger-editor.git
    cd swagger-editor
    npm install
    ```

4. 本地启动
    ```bash
    cd swagger-editor
    http-server
    ```
    访问：[http://127.0.0.1:8081](http://127.0.0.1:8081)
