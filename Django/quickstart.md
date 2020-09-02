# 快速入门

## 1、创建项目

```bash
pip install django
mkdir /Users/linrenwei/workspace/HelloDjango
django-admin startproject blogproject /Users/linrenwei/workspace/HelloDjango
```

新项目目录：
```markdown
HelloDjango\
    manage.py
    blogproject\
        __init__.py
        settings.py
        urls.py
        wsgi.py
```


## 2、启动

```bash
python manage.py runserver 0.0.0.0:8001
```


## 3、新建app

```bash
python manage.py startapp blog
# blog 为 app name
```