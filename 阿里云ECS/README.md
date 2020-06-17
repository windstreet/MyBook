# 阿里云ECS

## 1、购买服务器  

购买后可以 `重置实例密码`

## 2、mbp终端登录   
```bash

# ssh 主机名@公有ip
ssh root@xx.xxx.xxx.xxx

# 然后输入密码
```

## 3、常用工具安装

```bash

# 安装 `Zsh`（有时候无法安装，需要升级一下 `apt`）
apt update && apt install zsh


# 安装 `git`
sudo apt-get install git


# 安装 `oh my zsh`
# 安装 `oh my zsh` 之前需要安装 `git` 工具，因为 `oh my zsh` 托管在 `Github` 上
wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

# 修改 zsh 的界面风格
vim ~/.zshrc  # ZSH_THEME="bira"

# 然后修改 `shell` 为 `zsh`
chsh -s `which zsh`

# 重启服务器后修改才能生效
sudo shutdown -r 0  # 再重新登录就可以看到变化了


# 后续工具的安装，需要 `pip` 工具的支持
sudo apt-get install python-pip


# python 虚拟环境工具 `Virtualenv`
pip install virtualenv

```

## 4、创建新用户

```bash
# 新用户 blue
useradd blue -d /home/blue -m -s /bin/zsh

# 为新用户设置密码
passwd blue
# Enter new UNIX password:
# Retype new UNIX password:

```

## 5、使用 `新用户` 登录服务器 并 进行环境搭建

##### 5.1、登录服务器
```bash
# ssh 新用户名@公有ip
ssh blue@xx.xxx.xxx.xxx

# 紧接着你会遇到一大串的英文，按 `2` 即可。按 `2` 的意思是，使用常用配置项设置好Zsh，这样Zsh就生效可用了。

# 最好新用户自己安装一下 zsh，然后设置主题
```

##### 5.2、创建网站根目录
```bash
# 确定在用户号目录下 `/home/blue`
cd /home/blue
pwd


# 创建工作区域 `/home/blue/workspace`
mkdir workspace  


# 创建网站根目录 `/home/blue/workspace/blog`
cd workspace blog
mkdir blog 
cd blog

# 根目录下新建一个 `logs` 的文件夹，存放日志
mkdir logs

```

##### 5.3、上传网站文件包。

我使用的是 `Filezilla`，直接`SFTP`到服务器，注意：使用`blue用户` SFTP，别用`root主账号`。

##### 5.3、创建Python的虚拟环境及安装依赖
```bash
# 创建虚拟环境
virtualenv -p /usr/bin/python3 venv

# 进入虚拟环境
source venv/bin/activate

# 检测环境
python -V

# 安装上传项目的依赖
pip install -r requirements.txt

# 安装 Gunicorn
pip install gunicorn
```

> 备注：  
`Gunicorn` 是一个开源Python `WSGI` 服务，另一个选择是 `uWSGI`。       
后者指不定你见过多少次了，Python或者Flask的入门书里最后的那部分总会提及 `uWSGI`。       
至于为什么用 `Gunicorn` 而不是后者，因为 `Gunicorn` 简单，好用。    

##### 5.4、配置 `Gunicorn`    
`Gunicorn` 有很多种配置方式，这里使用的是加载配置文件的方案。

新建 gunicorn.conf 配置文件（在 `/home/blue/workspace/blog` 下）
```bash
vim gunicorn.conf   
```

在编辑文本中输入以下内容  
```
# 进程为3，小网站这个数就够了。
workers = 3

# 监听本地8000端口，之后这个端口来的就是看这个网站的。
bind = '127.0.0.1:8000'
```

保存文件，这样最后在网站根目录下有一个gunicorn.conf 的文件。  
```bash
pwd
# /home/blue/workspace/blog

ls  
# gunicorn.conf  logs  venv
```


---


## 6、使用`root账号`安装、配置`Nginx` 

##### 6.1、安装`Nginx`   
```bash
sudo apt-get install nginx
```

>注意：   
在刚注册阿里云的时候，一般的安全组是系统自动设置的，规则是关闭了80端口。  
所以在安全组中对80端口放行就可以了。     

[解决阿里云服务器安装nginx不能访问的问题](https://www.jianshu.com/p/0328acae26b0)    

然后就可以访问服务器的 `nginx`服务地址：`共用ip:80`    


##### 6.2、配置`Nginx`

```bash
cd /etc/nginx/sites-available/

# 新建并打开名为blog的配置文件
touch blog
vim blog
```

输入以下内容：   
```
   server {
        listen   80;
        server_name www.xxx.com xxx.com;

        root /home/blue/workspace/blog;
        access_log /home/blue/workspace/blog/logs/access.log;
        error_log /home/blue/workspace/blog/logs/access.log;

        location / {
            proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            if (!-f $request_filename) {
                proxy_pass http://127.0.0.1:8000;
                break;
            }
        }
    }
```

