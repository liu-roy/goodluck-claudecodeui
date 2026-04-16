# goodluck-claudecodeui

基于 Docker 的 Claude Code UI 运行环境，提供开箱即用的 AI 编程助手服务。

## 镜像结构

```
goodluck-claudecodeui-base   ← 基础环境镜像
        └── goodluck-claudecodeui-server   ← 服务镜像（对外暴露）
```

### Base 镜像 (`DockerFileBase`)

| 组件 | 版本 |
|------|------|
| Ubuntu | 22.04 |
| JDK | 8 (Temurin) / 17 (Temurin，默认) |
| Maven | 3.8.8 |
| Node.js | 22.14.0 |
| Claude Code CLI | 最新 |

### Server 镜像 (`DockerFile`)

- 在 base 基础上创建非 root 用户 `goodluck`
- 安装 `@cloudcli-ai/cloudcli`（Claude Code UI 服务）
- 默认监听端口 `3001`

## 快速开始

### 拉取并运行

```bash
docker pull liuroy/goodluck-claudecodeui-server:latest

docker run -d \
  -p 3001:3001 \
  -e ANTHROPIC_API_KEY=your_api_key \
  --name claudecodeui \
  liuroy/goodluck-claudecodeui-server:latest
```

浏览器访问 `http://localhost:3001`

### 本地构建

```bash
# 1. 构建 base 镜像
docker build -f DockerFileBase -t liuroy/goodluck-claudecodeui-base:latest .

# 2. 构建 server 镜像
docker build -f DockerFile -t liuroy/goodluck-claudecodeui-server:latest .
```

## 切换 Java 版本

容器内提供 `use-java` 快捷命令：

```bash
use-java 8    # 切换到 JDK 8
use-java 17   # 切换到 JDK 17（默认）
```

## CI/CD

| Workflow | 触发方式 | 说明 |
|----------|----------|------|
| `docker-claudecodeui-base.yml` | 手动触发 | 构建并推送 base 镜像 |
| `docker-claudecodeui-server.yml` | push main / 手动触发 | 构建并推送 server 镜像 |

需要在仓库 Secrets 中配置：

| Secret | 说明 |
|--------|------|
| `DOCKER_USERNAME` | Docker Hub 用户名 |
| `DOCKER_PASSWORD` | Docker Hub 密码或 Access Token |

## Skills

| Skill | 说明 |
|-------|------|
| `git-operation` | Git 常用操作指引（克隆、提交、推送等） |
