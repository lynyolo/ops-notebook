### 1. 查看 Docker 和 Docker Compose 版本

以 **root 用户** 或者已经有权限的用户执行：

```
docker -v
docker compose version
```

示例输出可能是：

```
Docker version 24.0.5, build c7f2c0f
Docker Compose version v2.20.2
```

> 注意：Docker Compose V2 是作为 Docker 的子命令 `docker compose` 使用的，而不是独立命令 `docker-compose`。

------

### 2. 让普通用户可以使用 Docker

Docker 默认只有 `root` 用户或 `docker` 组用户能执行。给普通用户授权步骤：

1. **将用户加入 `docker` 组**

假设用户名是 `alice`：

```
sudo usermod -aG docker alice
```

1. **重新登录或刷新组权限**

```
# 刷新当前终端会话
newgrp docker
```

1. **测试普通用户是否能执行 Docker 命令**

```
docker ps
```

如果能列出容器，就说明配置成功。