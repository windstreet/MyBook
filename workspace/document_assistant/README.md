# 物流关务助手-指南

## 测试环境

```bash
cd code/document_assistant/assistant

sudo git pull

docker-compose up -d --build
docker-compose exec web bash

# 进入容器后
python manage.py migrate

python manage.py shell
```