

# 一、文件操作

```
ls
cd
cp
mv
rm
cat
grep
find
tail
```

------

# 二、权限管理

```
chmod：调整权限
chown：调整归属
sudo
```

重点：

- rwx
- 755
- 777

------

# 三、进程与端口

```
ps -ef
top
netstat -tunlp
ss -tunlp
kill
```

## 1. **ps** – 查看进程（process status）

**功能**：列出系统中运行的进程。

**常用用法**：

```
ps -ef
```

- `-e`：显示所有进程（every process）
- `-f`：显示完整格式，包括 UID、PID、PPID、启动时间、命令（full format）

示例输出：

```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 10:00 ?        00:00:02 systemd
user      1234  1200  0 10:05 pts/0    00:00:00 bash
```

**其他参数**：

- `ps aux`：BSD 风格，显示所有进程和 CPU/内存占用（a-all user、u-面向用户格式输出、x包含无控制终端的进程）
- `ps -ef | grep python`：查找特定进程

**常见用法**：
- ps -ef | grep 某一服务
- ps -p pid -f：查看指定进程

------

## 2. **top** – 实时查看进程

**功能**：动态监控系统资源和进程。

```
top
```

**常用操作**：

- `P`：按 CPU 使用率排序
- `M`：按内存使用率排序
- `k`：终止进程，输入 PID
- `q`：退出 top

**显示信息**：

- PID：进程号
- %CPU：CPU 占用率
- %MEM：内存占用率
- COMMAND：命令

------

## 3. **netstat** – 网络连接和端口

**功能**：查看网络连接、监听端口、路由等

```
netstat -tunlp
```

参数说明：

- `-t`：显示 TCP
- `-u`：显示 UDP
- `-n`：数字显示（不解析域名）
- `-l`：显示监听端口
- `-p`：显示进程 PID/程序名

示例：

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22             0.0.0.0:*               LISTEN      1234/sshd
```

> Windows 类似命令：`netstat -ano`

------

## 4. **ss** – 新版网络工具（替代 netstat）

**功能**：查看套接字连接，比 netstat 更快。

```
ss -tunlp
```

参数与 netstat 类似：

- `-t`：TCP
- `-u`：UDP
- `-n`：数字显示
- `-l`：监听端口
- `-p`：显示 PID/进程名

**补充参数**：

- `ss -s`：显示网络摘要信息
- `ss -tuna`：显示所有 TCP/UDP 连接

------

## 5. **kill** – 终止进程

**功能**：向进程发送信号（默认 SIGTERM）

```
kill PID
```

常用信号：

| 信号         | 说明             | 用法           |
| ------------ | ---------------- | -------------- |
| 15 (SIGTERM) | 优雅终止（默认） | `kill 1234`    |
| 9 (SIGKILL)  | 强制终止         | `kill -9 1234` |
| 1 (SIGHUP)   | 重新加载配置     | `kill -1 1234` |

**常用操作**：

- 杀掉特定进程：

```
ps -ef | grep python
kill -9 5678
```

- 杀掉所有某类进程：

```
pkill python
killall python
```

------

# 四、日志查看

重点：

```
tail -f(follow)
grep
journalctl
```

日志位置：

```
/var/log/
```

## （一）`journalctl` 

journal是 Linux（systemd 系统）中用于查看 **系统日志和服务日志** 的命令，相当于替代传统 `/var/log` 下的日志文件查看。

------

### 1. 基本用法

```
journalctl
```

- 默认显示 **从最早到最新** 的系统日志
- 包括内核消息、服务输出、用户进程日志

------

### 2. 常用参数

| 参数             | 作用                                                         |
| ---------------- | ------------------------------------------------------------ |
| `-f`             | 实时跟踪日志（类似 `tail -f`）                               |
| `-n NUM`         | 显示最新 NUM 行日志                                          |
| `-u 服务名`      | 查看指定 systemd 服务日志，例如 `-u nginx`                   |
| `-p 优先级`      | 按日志级别过滤，0=emerg, 1=alert, 2=crit, 3=err, 4=warning, 5=notice, 6=info, 7=debug |
| `--since "时间"` | 从指定时间开始显示日志                                       |
| `--until "时间"` | 显示到指定时间为止                                           |
| `-k`             | 只显示内核消息                                               |
| `-o 输出格式`    | 输出格式，可选 `short`, `json`, `cat` 等                     |

------

### 3. 示例

#### 查看最新 50 行系统日志

```
journalctl -n 50
```

#### 实时跟踪日志

```
journalctl -f
```

#### 查看 nginx 服务日志

```
journalctl -u nginx
```

#### 查看过去一天的日志

```
journalctl --since "1 day ago"
```

#### 查看错误日志

```
journalctl -p 3 -xb
```

- `-p 3` → 只显示 `err` 及以上级别
- `-xb` → 显示当前启动（boot）的日志

------

### 4. 与 `tail`/`cat` 的区别

- `journalctl` 读取 **systemd 日志数据库**，不是普通文本文件
- 支持 **按服务、优先级、时间** 精确过滤
- 输出可自定义格式，比单纯查看 `/var/log/*.log` 更灵活

------

# 五、用户管理

```
useradd
passwd
su - username
usermod -aG groupname username
```

可能考：

- 最小权限原则
- 为什么不能多人共用root