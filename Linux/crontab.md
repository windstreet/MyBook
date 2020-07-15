# crontab

## 问题一
```
mac 想用 crontab -e 添加任务，但是发现会提示：
"/usr/bin/vi" exited with status 1

查看任务列表，也会发现没有创建成功：
crontab -l

上边命令会出现：no crontab for username
出现上述问题的原因是什么呢？
```

#### 解决方法
原因：系统没有安装vi     
解决方法：设置vim为默认编辑器
1. 打开`vim ~/.bash_profile`文件编辑
    ```
    export EDITOR=vim
    ```

2. 保存好后重新载入配置
    ```bash
    source ~/.bash_profile
    ```

3. 重新执行创建任务命令
    ```bash
    # 创建crontab文件
    sudo touch /etc/crontab 
    
    # 编写自己的计划任务
    crontab -e
    ```

4. 然后查看是否创建成功
    ```bash
    crontab -l
    ```

5. 成功后开启crontab服务
    ```bash
    # 开启
    sudo /usr/sbin/cron start

    # 重启
    sudo /usr/sbin/cron restart

    # 停止
    sudo /usr/sbin/cron stop
    ```

