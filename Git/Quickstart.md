# 快速入门

### 例一：创建本地的版本控制

- 首先在 GitHub 上创建一个仓库  
    譬如：https://github.com/windstreet/MyBook.git

- 最后在本地创建版本控制并提交至远程    

    ```bash
    # 进入指定的根目录
    cd ~/funny/MyBook
    
    # 始化空的 Git 仓库
    git init
    
    # 创建 gitignore 文件
    touch gitignore
    
    # 编辑 gitignore 文件，设置不追踪记录的文件
    vi gitignore
    
    # 改变名称
    mv gitignore .gitignore
    
    # 查看哪些文件可以添加到版本控制中
    git status
    
    # 添加文件
    git add README.md
    
    # 保存
    git commit -m "init"
    
    # 绑定远程仓库
    git remote add origin https://github.com/windstreet/MyBook.git
    
    # 提交
    git push --set-upstream origin master
    
    ```

### 例二：clone 他人轮子并移除其版本控制，创建自己的版本控制  

- 移除：
    ```bash
    cd /根目录
    ls -a  # 查看
    rm -rf .git  # 删除
    ```


