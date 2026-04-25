---
title: Docker
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [devops, docker, container, computer-science]
sources: [raw/sites/javaguide.cn/docs/tools/docker/]
confidence: high
---

# Docker

## 概述
Docker 是开源的容器化平台，将应用及其依赖打包成轻量级、可移植的容器，实现"一次构建，到处运行"。

## 核心概念

| 概念 | 说明 |
|------|------|
| Image | 镜像，只读模板 |
| Container | 容器，运行实例 |
| Registry | 镜像仓库 |
| Dockerfile | 构建脚本 |
| Docker Compose | 多容器编排 |

## 架构

```
┌─────────────────────────────────────┐
│           Docker Client            │
│            (docker CLI)            │
└───────────────┬─────────────────────┘
                │ REST API
┌───────────────▼─────────────────────┐
│           Docker Daemon            │
│         (dockerd)                  │
│  ┌───────��──────────────────────┐  │
│  │        containerd           │  │
│  │  ┌────────────────────────┐  │  │
│  │  │         runc          │  │  │
│  │  └────────────────────────┘  │  │
│  └──────────────────────────────┘  │
└─────────────────────────────────────┘
```

## Dockerfile 指令

| 指令 | 说明 |
|------|------|
| FROM | 基础镜像 |
| RUN | 执行命令 |
| COPY | 复制文件 |
| ADD | 复制+解压 |
| WORKDIR | 工作目录 |
| ENV | 环境变量 |
| EXPOSE | 暴露端口 |
| CMD | 容器启动命令 |
| ENTRYPOINT | 入口点 |
| VOLUME | 数据卷 |
| ARG | 构建参数 |

### 示例
```dockerfile
FROM openjdk:17-jdk-alpine
WORKDIR /app
COPY target/app.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## 常用命令

### 镜像操作
```bash
docker build -t myapp:1.0 .     # 构建镜像
docker images                   # 列出镜像
docker rmi <image>              # 删除镜像
docker pull <image>             # 拉取镜像
docker push <image>             # 推送镜像
```

### 容器操作
```bash
docker run -d -p 8080:80 --name web nginx
docker ps                       # 运行中容器
docker ps -a                    # 所有容器
docker stop <container>         # 停止
docker rm <container>           # 删除
docker logs <container>         # 查看日志
docker exec -it <container> sh  # 进入容器
```

### 网络与存储
```bash
docker network create mynet
docker volume create myvol
docker run -v myvol:/data ...
docker run --network mynet ...
```

## Docker Compose

### docker-compose.yml
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - db_data:/var/lib/mysql
volumes:
  db_data:
```

### 命令
```bash
docker-compose up -d       # 启动
docker-compose down        # 停止
docker-compose logs -f     # 日志
docker-compose ps          # 状态
```

## 最佳实践

### 镜像优化
- 使用 alpine 基础镜像
- 多阶段构建
- .dockerignore 排除文件
- 减少层数

### 安全
- 非 root 用户运行
- 扫描镜像漏洞
- 限制资源

## 相关概念
- [[kubernetes]] - 容器编排
- [[docker-compose]] - 多容器编排
- [[docker-network]] - Docker 网络
- [[docker-volume]] - 数据卷

## 参考文献
- JavaGuide: tools/docker/