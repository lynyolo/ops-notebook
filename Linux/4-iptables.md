# iptables 简介

`iptables` 是 **Linux 内核提供的通用包过滤和 NAT 框架**、Linux 中用于配置 **Netfilter 防火墙规则** 的工具。

作用：

- 允许/拒绝网络包
- 端口控制
- NAT
- 转发
- 防火墙策略

------

# 一、基本结构

iptables 命令核心结构：

```
iptables [-t 表] 操作 链 匹配条件 -j 动作
```

例如：

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

含义：

```
向 INPUT 链追加规则：
允许 TCP 22 端口（SSH）
```

------

# 二、核心概念

------

## 1. 表（table）

iptables 有多个“表”。

常见：

| 表     | 作用           |
| ------ | -------------- |
| filter | 过滤（最常用） |
| nat    | 地址转换       |
| mangle | 修改数据包     |
| raw    | 原始包处理     |

默认：

```
filter
```

所以很多命令不写 `-t filter`。

------

## 2. 链（chain）

每个表中有多个链。

------

### filter 表常用链

| 链      | 作用     |
| ------- | -------- |
| INPUT   | 进入本机 |
| OUTPUT  | 本机发出 |
| FORWARD | 转发     |

------

数据流：

```
外部 --> INPUT --> 本机
本机 --> OUTPUT --> 外部
```

路由转发：

```
外部 --> FORWARD --> 外部
```

------

# 三、操作参数

| 参数 | 含义            |
| ---- | --------------- |
| `-A` | append 追加     |
| `-I` | insert 插入     |
| `-D` | delete 删除     |
| `-L` | list 查看       |
| `-F` | flush 清空      |
| `-P` | policy 默认策略 |

------

# 四、匹配条件

------

## 1. 协议

```
-p tcp
-p udp
-p icmp
```

------

## 2. 源 IP

```
-s 192.168.1.10
```

------

## 3. 目标 IP

```
-d 8.8.8.8
```

------

## 4. 端口

```
--dport 80
--sport 22
```

需要配合：

```
-p tcp
```

------

# 五、动作（target）

使用：

```
-j 动作
```

常见动作：

| 动作   | 含义       |
| ------ | ---------- |
| ACCEPT | 允许       |
| DROP   | 丢弃       |
| REJECT | 拒绝并回应 |
| LOG    | 记录日志   |

------

# 六、常见示例

------

## 1. 查看规则

```
iptables -L -n -v
```

参数：

| 参数 | 含义     |
| ---- | -------- |
| `-L` | 列表     |
| `-n` | 数字显示 |
| `-v` | 详细     |

------

## 2. 允许 SSH

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

------

## 3. 禁止某 IP

```
iptables -A INPUT -s 1.2.3.4 -j DROP
```

------

## 4. 允许 ping

```
iptables -A INPUT -p icmp -j ACCEPT
```

------

## 5. 默认拒绝

```
iptables -P INPUT DROP
```

表示：

```
INPUT 默认全部拒绝
```

危险：

- 可能把自己 SSH 踢掉

------

## 6. 放行本地回环

```
iptables -A INPUT -i lo -j ACCEPT
```

------

## 7. 放行已建立连接

非常重要：

```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

否则：

- 返回包可能被拦截。

------

# 七、删除规则

------

## 方法1：按编号删除

查看编号：

```
iptables -L --line-numbers
```

删除：

```
iptables -D INPUT 3
```

------

## 方法2：按完整规则删除

```
iptables -D INPUT -p tcp --dport 22 -j ACCEPT
```

------

# 八、NAT 简介

开启地址伪装：

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

常用于：

- 路由器
- Docker
- 虚拟机 NAT

------

# 九、规则保存（Ubuntu）

通常安装：

```
iptables-persistent
```

------

# 十、现代替代品

现在很多系统实际已经转向：

```
nftables //netfiltertables
```

以及：

- firewalld
- ufw

它们底层可能仍兼容 iptables。

但：

```
iptables 仍然非常重要
```

因为：

- Docker
- Kubernetes
- 云服务器
- 运维面试

大量使用。