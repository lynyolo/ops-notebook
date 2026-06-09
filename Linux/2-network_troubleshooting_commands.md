# 网络排障命令教程



| 命令                     | Windows            | Linux           | 功能                             | 常用参数/用法                                                |
| ------------------------ | ------------------ | --------------- | -------------------------------- | ------------------------------------------------------------ |
| **ping**                 | ✅                  | ✅               | 测试目标主机是否可达，测延迟     | Windows: `ping www.baidu.com -n 4`（发送4次）Linux: `ping -c 4 www.baidu.com` |
| **tracert / traceroute** | ✅（tracert）       | ✅（traceroute） | 跟踪数据包经过的路由             | Windows: `tracert www.baidu.com`Linux: `traceroute www.baidu.com`常用参数：`-h` 设置最大跳数 |
| **netstat**              | ✅                  | ✅               | 查看网络连接、端口占用           | Windows: `netstat -ano`（显示PID）Linux: `netstat -tulnp`（显示TCP/UDP端口和进程） |
| **arp**                  | ✅                  | ✅               | 查看/修改本地ARP表（IP→MAC映射） | Windows: `arp -a` 查看Linux: `arp -n` 或 `ip neigh`          |
| **nslookup**             | ✅                  | ✅               | 查询DNS解析                      | Windows/Linux: `nslookup www.baidu.com`可指定DNS服务器：`nslookup www.baidu.com 8.8.8.8` |
| **telnet**               | ✅                  | ✅（需要安装）   | 测试TCP端口连通性                | `telnet 192.168.1.1 80`注意：Linux可能需 `sudo apt install telnet` |
| **curl**                 | ❌ 默认无（需安装） | ✅（通常已安装） | HTTP请求/调试接口/API            | GET: `curl http://example.com`POST: `curl -X POST -d "key=val" http://example.com`常用参数：`-I` 只显示响应头，`-v` 显示详细请求过程 |

## 1. ping
- 功能：测试目标主机是否可达（连通性测试）
- Windows: ping baidu.com -n 4
- Linux: ping -c 4 baidu.com
- 常用参数：
  - -t (Windows) 持续 ping
  - -i (Linux) TTL 设置

## 2. tracert / traceroute
- 功能：跟踪数据包经过路由（怎么去、哪里断了或慢了）
- Windows: tracert baidu.com
- Linux: traceroute baidu.com
- 常用参数：
  - -h 最大跳数
  - -I 使用 ICMP
  - -T 使用 TCP

## 3. netstat
- 功能：查看端口占用、网络连接
- Windows: netstat -ano
- Linux: netstat -tulnp
- 替代命令: ss -tulnp

## 4. arp
- 功能：查看/修改本地 ARP 表
- Windows: arp -a
- Linux: arp -n 或 ip neigh

## 5. nslookup
- 功能：查询 DNS 解析
- Windows/Linux: nslookup www.baidu.com
- 指定 DNS: nslookup www.baidu.com 8.8.8.8

## 6. telnet
- 功能：测试 TCP 端口连通性
- 示例: telnet 192.168.1.1 80
- Linux 可能需安装: sudo apt install telnet

## 7. curl
- 功能：HTTP 请求 / 调试接口
- GET: curl http://example.com
- POST: curl -X POST -d "key=val" http://example.com
- 常用参数:
  - -I 显示响应头
  - -v 显示详细请求过程

## 8. 补充命令
- ipconfig / ifconfig – 查看本机 IP
- route / ip route – 查看路由表
- dig – 高级 DNS 查询
- ping6 / traceroute6 – IPv6 网络测试





