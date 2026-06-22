# Docker 常用命令速查

## 镜像管理
```bash
docker build -t myapp:v1 .
docker images
docker rmi <image_id>
docker pull nginx:latest
```

## 容器操作
```bash
docker run -d -p 8080:80 --name web nginx
docker ps -a
docker logs -f web
docker exec -it web /bin/sh
docker stop web && docker rm web
```

## Docker Compose
```bash
docker-compose up -d
docker-compose logs -f
docker-compose down -v
```

## 清理
```bash
docker system prune -a    # 清理所有未使用资源
docker volume prune       # 清理未使用的卷
```
