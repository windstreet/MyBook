# 快速入门

```bash
# 1、创建项目
pip install django
mkdir /Users/linrenwei/workspace/HelloDjango
django-admin startproject blog_project /Users/linrenwei/workspace/HelloDjango
cd /Users/linrenwei/workspace/HelloDjango


# 2、创建超级用户
python manage.py createsuperuser
# Username: admin
# Email address: admin@admin.com
# Password: admin


# 3、新建app
python manage.py startapp blog


# 4、迁移数据库
python manage.py makemigrations  # 生成迁移脚本
python manage.py migrate
python manage.py sqlmigrate blog 0001  # 查看迁移脚本的sql


# 5、操作数据库
python manage.py shell


# 6、启动
python manage.py runserver 0.0.0.0:8000

```
