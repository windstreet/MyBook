# 一个简单的例子

1. 在 GitHub 上创建一个仓库

   譬如：https://github.com/windstreet/MyBook.git



```bash
# 进入指定的根目录
cd ~/funny/MyBook

# 创建 gitignore 文件
touch gitignore

# 编辑 gitignore 文件
vi gitignore

# 改变名称
mv gitignore .gitignore

# 添加文件
git add README.md

# 保存
git commit -m "init"

# 绑定远程仓库
git remote add origin https://github.com/windstreet/MyBook.git

# 提交
git push --set-upstream origin master

```



