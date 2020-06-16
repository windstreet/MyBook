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

## 5、使用 `新用户` 登录服务器

```bash
# ssh 新用户名@公有ip
ssh blue@xx.xxx.xxx.xxx
```