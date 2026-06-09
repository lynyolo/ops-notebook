# Linux下新建普通用户

## 1、新建普通用户

- 创建用户

```bash
# 创建用户 user1 并自动创建主目录 /home/user1
useradd -m user1
```

- 设置密码

```bash
passwd user1
```

## 2、给用户sudo权限

```BASH
# append group
usermod -aG sudo user1
```

## 3、切换用户测试

```BASH
su - user1
```

- 查看

```BASH
whoami
```

## 4、需要管理员权限时

```BASH
sudo apt update
```

## 5、SSH设置默认使用普通用户登录

- xshell上新建连接

```BASH
ssh user1@主机名
```

