# docker本地和容器之间的文件传输

## 获取容器id全称

```bash
docker inspect -f '{{.id}}' 容器名称
```

## 本地文件传输到容器

```bash
docker cp 本地文件路径 ID全称:容器路径
```

## 容器文件传输到本地

```bash
docker cp ID全称:容器路径 本地文件路径
```

# docker容器创建

## docker容器随docker启动而启动

```bash
docker  --restart=always
```

# Redis容器创建(最简单)

```bash
docker run --name env-redis -d -p 6379:6379 redis --requirepass Redis.123
```

# Mysql容器创建(最简单)

```bash
docker run --name env-mysql -e MYSQL_ROOT_PASSWORD=Mysql.123 -d -p 3306:3306 mysql
```

# Nginx容器创建

```bash
docker run --name env-nginx -d -p 8080:80 nginx:stable-perl
```

# LobeChat容器创建（内部包含Ollama）

```bash
docker run --name env-lobechat -e OLLAMA_PROXY_URL=http://host.docker.internal:11434/v1 -d -p 3210:3210 lobehub/lobe-chat
```

# dify容器创建

```bash
git clone https://github.com/langgenius/dify.git
cd dify/docker
docker compose up -d
```