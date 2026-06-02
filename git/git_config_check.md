## 1. 检查 Git 配置

打开 Git Bash：

```
git --version
```

输出类似：

```
git version 2.42.0
```

说明 Git 正常安装。

查看用户名和邮箱配置：

```
git config --global user.name
git config --global user.email
```

如果没有配置，需要重新设置：

```
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的邮箱"
```

------

## 2. 检查 SSH 或 HTTPS 连接

如果之前配置过 SSH Key：

```
ssh -T git@github.com
```

- 成功会显示：

```
Hi username! You've successfully authenticated...
```

之前没有配置的话：

```BASH
# 1、本地添加ssh配置
nano ~/.ssh/config
# 在文件中添加
Host github.com
    HostName github.com
    User git
    IdentityFile C:\Users\Administrator\.ssh\id_rsa
# ctrl+o--保存，ctrl+x--退出

# 2、GitHub的设置-ssh与pgp中添加ssh密钥
cat ~/.ssh/id_rsa.pub
# 复制并粘贴到GitHub的ssh密钥框
```



------

## 4. GitHub 上新建仓库

1. 登录 GitHub → 点击 **New repository** ，填写对应内容后，点击 **Create repository**
2. 页面会显示仓库地址，例如：

```
https://github.com/username/my-notes.git
```

## 5. 提交本地仓库

假设你有一个文件夹 `~/notes`，里面有 `create_normal_user.md`

```
cd ~/notes       # 进入文件夹
git init         # 初始化本地仓库
git add .        # 添加所有文件到暂存区
git commit -m "first commit"  # 提交到本地仓库
```

## 6. 关联远程仓库

```
git remote add origin https://github.com/username/my-notes.git
```

- `origin` = 远程仓库名字，约定俗成
- URL = GitHub 仓库地址

查看是否成功：

```
git remote -v
```

## 7. 上传到 GitHub

第一次上传：

```
git push -u origin main
```

说明：

- `origin` → 远程仓库
- `main` → 本地主分支
- `-u` → 设置默认上游分支，以后 `git push` 可以省略 `origin main`

以后更新文件流程：

```
git add .
git commit -m "update notes"
git push
```

## 8. 其他-检查仓库状态

进入本地项目目录：

```
cd /path/to/your/project
```

查看状态：

```
git status
```

- `nothing to commit, working tree clean` → 没有未提交的修改
- 有修改或新文件 → 会显示 `modified` 或 `untracked`

