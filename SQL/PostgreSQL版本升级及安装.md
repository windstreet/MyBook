# PostgreSQL版本升级及安装

### 一、升级
```bash
# 查看使用 brew运行的程序有哪些
brew services ls


# 停止运行postgresql服务
brew services stop postgresql


# 先升级brew
brew update


# 升级postgresql到最新版本
brew upgrade postgresql && brew postgresql-upgrade-database
# 出现报错。。


# 删除重装为9.6最高版本
brew uninstall postgres
cd /usr/local/Cellar/postgresql
ls
rm -rf 9.6.3
ls
brew install postgresql@9.6
```


### 二、将`postgresql`服务路径添加到系统环境变量中
```bash
# 1.编辑
sudo vim /etc/paths
# Password: 开机密码


# 2.写入的内容
/usr/local/Cellar/postgresql@9.6/9.6.10/bin    # 将路径加入系统


# 3.保存并退出：（按q键切换模式, 输入）
:x!


# 4.检查是否正确保存路径
source /etc/paths

```


### 三、其他操作
1. 重启电脑

2. 启动 postgresql 服务
    ```bash
    brew services start postgresql@9.6
    ```

3. 新装的数据库默认只有 `postgres` 和 `template1` 这两个数据库，自己可以手动创建一个 `admin` 数据库。

4. 以用户 `admin` 访问 `postgres` 数据库
    ```bash
    psql -U admin -d postgres
    ```


### 四、导入数据库
```bash
pg_restore --jobs=6 ~/Downloads/dump-2018-08-21-155041.pg_dump -d aide_test_environment -U admin -O
```
>其中：
- `~/Downloads/dump-2018-08-21-155041.pg_dump`：下载的测试环境数据库。
- `aide_test_environment`：导入到本地数据库后的新名字为 `aide_test_environment`。
- `admin`：用户名。        


### 五、补充
```bash
# 安装
brew install postgresql

# 升级到最新
brew upgrade postgresql
```
