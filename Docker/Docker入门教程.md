# Docker入门教程

## 一、环境配置的难题（了解）
- 软件开发最大的麻烦事之一，就是环境配置。

- 用户必须保证两件事：1、操作系统的设置，2、各种库和组件的安装。都正确，软件才能运行。

- 软件可以带环境安装？即安装的时候，把原始环境一模一样地复制过来。


---


## 二、虚拟机（了解）

##### 简介
- 虚拟机（`virtual machine`）就是带环境安装的一种解决方案。

- 它可以在一种操作系统里面运行另一种操作系统，比如在 Windows 系统里面运行 Linux 系统。

- 应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。

##### 缺点

- `资源占用多`：虚拟机会独占一部分内存和硬盘空间。

- `冗余步骤多`：虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。

- `启动慢`：启动操作系统需要多久，启动虚拟机就需要多久。


---


## 三、Linux 容器（了解）

##### 简介
- `Linux` 发展出了另一种虚拟化技术：`Linux 容器`（Linux Containers，缩写为 LXC）。

- Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。或者说，在正常进程的外面套了一个保护层。

- 对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离。

- 容器是进程级别的。

##### 优点
- `启动快`：容器里面的应用，直接就是底层系统的一个进程。启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多。

- `资源占用少`：容器只占用需要的资源，不占用那些没有用到的资源。多个容器可以共享资源，而虚拟机都是独享资源。

- `体积小`：容器只要包含用到的组件即可，而虚拟机是整个操作系统的打包。

>总之，容器有点像轻量级的虚拟机，能够提供虚拟化的环境，但是成本开销小得多。


---


## 四、Docker 是什么？
- Docker 属于 Linux 容器的一种`封装`，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。

- Docker 将`应用程序`与`该程序的依赖`，打包在一个文件里面。

- 运行这个文件，就会生成一个`虚拟容器`。

- 程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。

- 总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。


---

## 五、Docker 的用途  

1. `提供一次性的环境`。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。

2. `提供弹性的云服务`。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。

3. `组建微服务架构`。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。


---


## 六、Docker 的安装

安装过程略

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
