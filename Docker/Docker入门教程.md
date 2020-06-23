# Docker入门教程

## 一、环境配置的难题（了解）
- 软件开发最大的麻烦事之一，就是 __环境配置__。

- 用户必须保证两件事：1、操作系统的设置，2、各种库和组件的安装。都正确，软件才能运行。

- 软件可以带环境安装？即安装的时候，把原始环境一模一样地复制过来。


---


## 二、虚拟机（了解）

##### 简介
- __虚拟机（`virtual machine`）__ 就是带环境安装的一种解决方案。

- 它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。

- 应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。

##### 缺点

- __`资源占用多`__：虚拟机会独占一部分内存和硬盘空间。

- __`冗余步骤多`__：虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。

- __`启动慢`__：启动操作系统需要多久，启动虚拟机就需要多久。


---


## 三、Linux 容器（了解）

##### 简介
- `Linux` 发展出了另一种虚拟化技术：__`Linux 容器`（Linux Containers，缩写为 LXC）__。

- `Linux容器`不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。

- 对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

- 容器是进程级别的。

##### 优点
- __`启动快`__：容器里面的应用，直接就是底层系统的一个进程。启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多。

- __`资源占用少`__：容器只占用需要的资源，不占用那些没有用到的资源。多个容器可以共享资源，而虚拟机都是独享资源。

- __`体积小`__：容器只要包含用到的组件即可，而虚拟机是整个操作系统的打包。

>总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。


---


## 四、`Docker`是什么？
- `Docker`属于`Linux容器`的一种`封装`，提供简单易用的容器使用接口。它是目前最流行的`Linux容器`解决方案。

- `Docker`将 __`应用程序`__ 与 __`该程序的依赖`__，打包在一个文件里面。

- 运行这个文件，就会生成一个`虚拟容器`。

- 程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了`Docker`，就不用担心环境问题。

- 总体来说，`Docker`的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。


---

## 五、Docker 的用途  

1. __`提供一次性的环境`__。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

2. __`提供弹性的云服务`__。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

3. __`组建微服务架构`__。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。


---


## 六、Docker 的安装

（安装过程略）

##### 验证是否安装成功
```bash
docker version
# 或者
docker info
```


---


## 七、image 文件

- Docker 把应用程序及其依赖，打包在 image 文件里面。

- 只有通过这个文件，才能生成 Docker 容器。

- image 文件可以看作是容器的模板。

- Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

- image 是二进制文件。

- 实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

```bash
# 列出本机的所有 image 文件。
docker images
docker image ls


# 删除 image 文件
docker image rm [imageName]
```

>补充：   
image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。  
一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。   
即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。   
为了方便共享，image 文件制作完成后，可以上传到网上的仓库。   
Docker 的官方仓库 [Docker Hub](https://hub.docker.com/) 是最重要、最常用的 image 仓库。   
此外，出售自己制作的 image 文件也是可以的。


---


## 八、实例：hello world   
以最简单的 image 文件 ["hello world"](https://hub.docker.com/_/hello-world)，感受一下 Docker。

>特别说明：  
国内连接 Docker 的官方仓库很慢，还会断线。   
解决办法：(1) 将默认仓库改成国内的镜像网站; (2)使用终端代理工具 `proxychains4`

##### 8.1、将`image`文件从仓库抓取到本地

```bash
docker image pull library/hello-world
# 或者
docker image pull hello-world
```

>注解：   
`docker image pull` 是抓取 image 文件的命令。     
`library/hello-world` 是 image 文件在仓库里面的位置。      
`library` 是 image 文件所在的组。    
`hello-world` 是 image 文件的名字。     
由于 Docker 官方提供的 image 文件，都放在library组里面，它是默认组，所以可以省略。

##### 8.2、运行这个`image`文件   
```bash
docker container run hello-world

# Hello from Docker!
# This message shows that your installation appears to be working correctly.
#
# ... ...
```

>注解：   
`docker container run` 命令会从 image 文件，生成一个正在运行的`容器实例`。 
`docker container run` 命令具有自动抓取 image 文件的功能。     
如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的`docker image pull`命令并不是必需的步骤。

输出这段提示以后，hello world就会停止运行，容器`自动终止`。

##### 8.3、有些容器不会自动终止，因为提供的是服务。  
比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

```bash
docker container run -it ubuntu bash

# 先前本地获取的旧image
docker container run -it ubuntu:16.04 bash
```

>注解：      
`ubuntu:16.04` 即表示 `REPOSITORY:TAG`。       
若只填 `ubuntu` 则默认使用最新的image `ubuntu:latest`，会从线上仓库下载。

##### 8.4、强制终止容器

对于那些不会自动终止的容器，必须使用 `docker container kill [containID]` 命令手动终止。（貌似输入 `exit` 也能退出）

```bash
╭─xxx@xx ~/funny/django-example ‹master›
╰─$ docker container run -it ubuntu:16.04 bash             
root@551ecb75f44e:/#
root@551ecb75f44e:/#
root@551ecb75f44e:/#

# 另外起一个终端
docker container kill 551ecb75f44e
```


---


## 九、容器文件
- image 文件生成的`容器实例`，本身也是一个文件，称为`容器文件`。     
- 也就是说，一旦容器生成，就会同时存在两个文件： `image 文件` 和 `容器文件`。      
- 而且关闭容器并不会删除容器文件，只是容器停止运行而已。

##### 9.1、列出容器文件
```bash
# 列出本机正在运行的容器
docker container ls

# 列出本机所有容器，包括终止运行的容器
docker container ls --all
```

>上面命令的输出结果之中，包括容器的 ID（`CONTAINER ID`）。


##### 9.2、删除容器文件
终止运行的容器文件，依然会占据硬盘空间。

```bash
docker container rm [containerID]
```


---


## 十、`Dockerfile` 文件   
- 如果你要推广自己的软件，势必要自己制作 image 文件。

- 如何可以生成 image 文件？这就需要用到 `Dockerfile` 文件。

- `Dockerfile` 是一个文本文件，用来`配置 image`。Docker 根据 该文件生成二进制的 image 文件。

下面通过一个实例，演示如何编写 `Dockerfile` 文件。


---


## 十一、实例：制作自己的 Docker 容器
下面以 [koa-demos](http://www.ruanyifeng.com/blog/2017/08/koa.html) 项目为例，介绍怎么写 Dockerfile 文件，实现让用户在 Docker 容器里面运行 Koa 框架。

作为准备工作，请先下载源码。    
```bash
git clone https://github.com/ruanyf/koa-demos.git

cd koa-demos
```

##### 11.1、编写`Dockerfile`文件
+ 首先，在项目的`根目录`下，新建一个文本文件`.dockerignore`，写入下面的内容。   
```
.git
node_modules
npm-debug.log
```
>注解：   
上面代码表示，这三个路径要排除，不要打包进入 image 文件。   
如果你没有路径要排除，这个文件可以不新建。


+ 然后，在项目的`根目录`下，新建一个文本文件 `Dockerfile`，写入下面的内容。        
```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```
>含义：     
(1)`FROM node:8.4`：该`image`文件继承官方的`node image`，冒号后表示标签，即`8.4版本的node`。            
(2)`COPY . /app`：将`当前目录下的所有文件`（除了`.dockerignore`排除的路径），都拷贝进入 `image 文件的/app目录`。        
(3)`WORKDIR /app`：指定接下来的`工作路径`为`/app`。         
(4)`RUN npm install`：在`/app目录`（上一步指定的工作路径）下，运行`npm install`命令安装依赖。注意，安装后所有的依赖，都将打包进入`image`文件。       
(5)`EXPOSE 3000`：将容器`3000`端口暴露出来，允许外部连接这个端口。      

##### 11.2、创建`image`文件
有了`Dockerfile`文件以后，就可以使用`docker image build`命令创建`image`文件了。

```bash
docker image build -t koa-demo .
# 或者
docker image build -t koa-demo:0.0.1 .
```
>注解：      
(1)`-t`参数：用来指定 image 文件的名字，后面还可以`用冒号指定标签`。如果不指定，`默认的标签就是latest`。      
(2)`最后的那个点`：表示`Dockerfile`文件所在的路径，上例是当前路径，所以是一个点。   

如果运行成功，就可以看到新生成的`image`文件`koa-demo`了。    
```bash
docker image ls
```

>可能遇到的问题：   
macOS: [Docker Mac OS 下链接不上docker hub错误](https://www.jianshu.com/p/646610c781b1)    
国内的`image`仓库的镜像网址："https://registry.docker-cn.com"

##### 11.3、生成容器

- 执行命令生成容器
```bash
docker container run -p 8000:3000 -it koa-demo /bin/bash
# 或者
docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
```
>注解：    
(1)`docker container run`命令会从`image`文件生成容器。      
(2)`-p`参数：容器的`3000`端口映射到本机的`8000`端口。         
(3)`-it`参数：容器的`Shell`映射到当前的`Shell`，然后你在本机窗口输入的命令，就会传入容器。       
(4)`koa-demo:0.0.1`：`image`文件的名字（如果有标签，还需要提供标签，默认是`latest`标签）。     
(5)`/bin/bash`：容器启动以后，内部第一个执行的命令。这里是`启动Bash`，保证用户可以使用`Shell`。       

- 如果一切正常，运行上面的命令以后，就会返回一个命令行提示符。   
```bash
root@66d80f4aaf1e:/app#
```

- 这表示你已经在容器里面了，返回的提示符就是容器内部的`Shell`提示符。执行下面的命令。
```bash
root@66d80f4aaf1e:/app# node demos/01.js
```
>注解：   
(1)这时，`Koa`框架已经运行起来了。打开本机的浏览器，访问`http://127.0.0.1:8000`，网页显示"Not Found"，这是因为这个demo没有写路由。   
(2)这个例子中，`Node进程`运行在`Docker容器的虚拟环境`里面，进程接触到的文件系统和网络接口都是虚拟的，与本机的文件系统和网络接口是隔离的，因此需要定义`容器与物理机的端口映射`（map）。


- 停止/退出容器运行
在容器的命令行，按下 `Ctrl + c` 停止 Node 进程，然后按下 `Ctrl + d` （或者输入 `exit`）退出容器。此外，也可以用如下命令：        

```bash
# 在本机的另一个终端窗口，查出容器的 ID
docker container ls

# 停止指定的容器运行
docker container kill [containerID]
```


- 删除容器文件  

```bash
# 容器停止运行之后，并不会消失，需要自己删除。

# 查出容器的 ID
docker container ls --all

# 删除指定的容器文件
docker container rm [containerID]


# 也可以使用docker container run命令的--rm参数，在容器终止运行后自动删除容器文件。
docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
```

##### 11.4、 `CMD`命令       
- 上一节的例子里面，容器启动以后，需要手动输入命令`node demos/01.js`。
- 我们可以把这个命令写在`Dockerfile文件`里面，这样容器启动以后，这个命令就已经执行了，不用再手动输入了。

```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```
>注解：    
`CMD node demos/01.js`，表示容器启动后自动执行 `node demos/01.js`

- `RUN命令`与`CMD命令`的区别在哪里？     
    - `RUN命令`在 `image` 文件的构建阶段执行，执行结果都会打包进入 `image` 文件；    
    - `CMD命令` 则是在容器启动后执行。    
    - 另外，一个 `Dockerfile` 可以包含多个`RUN`命令，但是只能有一个`CMD`命令。    
    - 注意，指定了`CMD`命令以后，`docker container run`命令就不能附加命令了（比如前面的`/bin/bash`），否则它会覆盖`CMD`命令。    
    
现在，启动容器可以使用下面的命令：    
```bash
docker container run --rm -p 8000:3000 -it koa-demo:0.0.1
```


##### 11.5、发布`image`文件（略）    
如何把`image文件`分享到网上，让其他人使用？

首先，去 [hub.docker.com](https://hub.docker.com/) 或 [cloud.docker.com](https://cloud.docker.com/) 注册一个账户。
   
```bash
# 然后，用下面的命令登录： 
docker login


# 接着，为本地的 image 标注用户名和版本。
docker image tag [imageName] [username]/[repository]:[tag]
# 实例
docker image tag koa-demos:0.0.1 ruanyf/koa-demos:0.0.1


# 也可以不标注用户名，重新构建一下 image 文件。
docker image build -t [username]/[repository]:[tag] .


# 最后，发布 image 文件。
docker image push [username]/[repository]:[tag]


# 发布成功以后，登录 `hub.docker.com`，就可以看到已经发布的 image 文件。
```


---


## 十二、其他有用的命令

##### （1）`docker container start`   
- 前面的`docker container run`命令是`新建容器`，每运行一次，就会新建一个容器。    
- 同样的命令运行两次，就会生成两个一模一样的容器文件。      
- 如果希望重复使用容器，就要使用`docker container start`命令，它用来启动已经生成、已经停止运行的容器文件。
```bash
docker container start [containerID]
```
            
#####（2）`docker container stop`

- 前面的`docker container kill`命令`终止容器运行`，相当于向容器里面的主进程发出`SIGKILL`信号。
- 而`docker container stop`命令也是用来终止容器运行，相当于向容器里面的主进程发出`SIGTERM`信号，然后过一段时间再发出`SIGKILL`信号。
```bash
bash container stop [containerID]
```
>注意：    
（1）这两个信号的差别是，应用程序收到`SIGTERM信号`以后，可以自行进行收尾清理工作，但也可以不理会这个信号。  
（2）如果收到`SIGKILL信号`，就会强行立即终止，那些正在进行中的操作会全部丢失。    

#####（3）`docker container logs`   
- `docker container logs`命令用来查看`docker`容器的输出，即容器里面`Shell`的标准输出。   
- 如果`docker run`命令运行容器的时候，没有使用`-it参数`，就要用这个命令查看输出。
```bash
docker container logs [containerID]
```


#####（4）`docker container exec`

- `docker container exec`命令用于进入一个正在运行的 `docker` 容器。    
- 如果`docker run`命令运行容器的时候，没有使用`-it参数`，就要用这个命令进入容器。    
- 一旦进入了容器，就可以在容器的`Shell`执行命令了。
```bash
docker container exec -it [containerID] /bin/bash
```
 
#####（5）`docker container cp`

- `docker container cp`命令用于从正在运行的`Docker`容器里面，将文件拷贝到本机。下面是拷贝`到当前目录`的写法。
```bash
docker container cp [containID]:[/path/to/file] .
```

---

[转载](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)
