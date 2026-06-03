## **1️⃣ 安装 VSCode 插件**

- **Remote - SSH**：用于连接远程 ECS。
- **Docker**：可在远程环境管理 Docker 容器和镜像。
- **YAML**（可选）：便于编辑 `docker-compose.yml`。

------

## **2️⃣ 连接远程 ECS**

1. 打开 VSCode → 左下角点击 **Remote-SSH: Connect to Host…**

2. 配置 `~/.ssh/config`，例如：

   ```
   Host ecs
       HostName <ECS公网IP或内网IP>
       User root
       IdentityFile ~/.ssh/id_rsa
   ```

3. 选择 `ecs` 连接。连接成功后，VSCode 界面左下角会显示远程环境。

------

## **3️⃣ 在 ECS 上安装 Docker 和 Docker Compose**

在 VSCode Remote-SSH 终端执行（假设使用 Ubuntu）：

```
# 安装 Docker

# 安装 Docker Compose（v2）

# 启动 Docker
sudo systemctl enable docker
sudo systemctl start docker

# 添加当前用户到 docker 组（避免每次用 sudo）
sudo usermod -aG docker $USER
newgrp docker
```

> 注意：Docker Compose v2 使用 `docker compose`，旧版本是 `docker-compose`。

------

## **4️⃣ 配置 VSCode Docker 插件**

1. 在 Remote-SSH 打开的环境里安装 **Docker** 插件。
2. 打开 Docker 插件 → 点击 **“Add Docker Host”** → 选择 **Remote**（自动识别远程 Docker）。
3. 此时 VSCode 可以显示远程 Docker 的镜像、容器和 Compose 项目。

------

## **5️⃣ 编辑 & 启动容器**

1. 在 VSCode Remote-SSH 编辑 `docker-compose.yml` 文件。
2. 打开终端（在远程环境中），执行：

```
docker compose up -d
```

1. 查看容器状态：

```
docker ps
```

1. 停止容器：

```
docker compose down
```

------

✅ **提示**

- 推荐用 VSCode **Remote-SSH 打开的终端**，避免本地 Docker 和远程 Docker 混淆。
- 每次修改 `.yml` 后，先运行：

```
docker compose config
```

校验语法是否正确。