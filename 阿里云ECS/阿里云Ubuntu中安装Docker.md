# 阿里云Ubuntu中安装Docker


## 1、查看linux系统内核
```bash
uname -r
# 4.15.0-101-generic
```


---


## 2、修改阿里云服务器的源
我在执行`apt-get install docker.io`的时候，总是提示找不到，
说明我的安装源出了问题，所以有必要说明如何搞定安装源。

- 2.1、首先备份源列表
```bash
cp /etc/apt/sources.list /etc/apt/sources.list_backup
```

- 2.2、然后用`vim`打开`源列表文件` 
```bash
vim /etc/apt/sources.list
```

- 2.3、清空源列表文件的所有内容   
在`vim`编辑器里执行 `ggdG`
>注解      
（1）`gg`为跳转到文件首行；       
（2）`dG`为删除光标所在行以及其下所有行的内容；      
（3）再细讲，`d`为删除，`G`为跳转到文件末尾行。      

- 2.4、然后选择阿里云列表     
在`vim`编辑器里输入 
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```


---


## 3、执行更新命令
```bash
apt-get update
```

- 3.1、出现问题
```
W: GPG error: http://mirrors.cloud.aliyuncs.com/docker-ce/linux/ubuntu bionic InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 7EA0A9C3F273FCD8
```
貌似是公钥不可以用了，需要获取新的key

- 3.2、解决办法
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7EA0A9C3F273FCD8                          
#Executing: /tmp/apt-key-gpghome.KwYzoyuX84/gpg.1.sh --keyserver keyserver.ubuntu.com --recv-keys 7EA0A9C3F273FCD8
#gpg: key 8D81803C0EBFCD88: 8 signatures not checked due to missing keys
#gpg: key 8D81803C0EBFCD88: public key "Docker Release (CE deb) <docker@docker.com>" imported
#gpg: Total number processed: 1
#gpg:               imported: 1
```


---


## 4、安装docker
```bash
apt-get install docker.io
```


---

[转载](https://www.jianshu.com/p/76d41b3a078e)
