# 本地 Git 授权 Gitee 账号指南

本文介绍如何在已安装 Git 的本地环境中，完成 Gitee（码云）账号的授权配置，以便正常执行 `push`、`pull` 等需要身份验证的操作。

---

## 第一步：配置 Git 用户信息

在终端中运行以下命令，将 `用户名` 和 `邮箱` 替换为你的 Gitee 账号信息：

```bash
git config --global user.name "你的Gitee用户名"
git config --global user.email "你的Gitee注册邮箱"
```

验证配置是否生效：

```bash
git config --global --list
```

---

## 方式一：SSH 密钥授权（推荐）

### 1. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "你的Gitee注册邮箱"
```

一路按回车使用默认路径（`~/.ssh/id_ed25519`），也可以设置密码短语（passphrase）以增加安全性。

> 如果本机已有 SSH 密钥（如已配置过 GitHub），可跳过此步，直接使用已有的公钥。

### 2. 将公钥添加到 Gitee

复制公钥内容：

```bash
# Linux / macOS
cat ~/.ssh/id_ed25519.pub

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub
```

然后：

1. 登录 [Gitee](https://gitee.com) → 右上角头像 → **设置**
2. 左侧菜单选择 **SSH公钥**
3. 填写标题（如 `my-laptop`），将复制的公钥粘贴到 **公钥** 文本框
4. 点击 **确定**，根据提示输入 Gitee 账号密码完成验证

### 3. 测试 SSH 连接

```bash
ssh -T git@gitee.com
```

看到类似 `Hi 用户名! You've successfully authenticated...` 的提示即表示授权成功。

### 4. 使用 SSH 地址克隆或修改远程地址

克隆时使用 SSH 地址：

```bash
git clone git@gitee.com:用户名/仓库名.git
```

已有本地仓库，修改远程地址为 SSH：

```bash
git remote set-url origin git@gitee.com:用户名/仓库名.git
```

---

## 方式二：HTTPS + 私人令牌（Private Token）

Gitee 支持通过 **私人令牌** 代替密码进行 HTTPS 认证，更安全。

### 1. 生成私人令牌

1. 登录 Gitee → 右上角头像 → **设置**
2. 左侧菜单选择 **私人令牌**
3. 点击 **生成新令牌**，填写描述，勾选所需权限（至少勾选 `projects`）
4. 点击 **提交**，根据提示输入账号密码
5. 生成后**立即复制并妥善保存**，页面关闭后无法再次查看

### 2. 使用令牌进行认证

克隆或推送时，将令牌作为密码输入（或直接嵌入 URL）：

```
用户名：你的 Gitee 用户名
密码：粘贴上面生成的私人令牌（不是 Gitee 登录密码）
```

也可以将令牌直接嵌入远程地址（仅限本地使用，勿提交到仓库）：

```bash
git remote set-url origin https://用户名:私人令牌@gitee.com/用户名/仓库名.git
```

### 3. 保存凭据（避免每次都输入）

**macOS**（使用系统钥匙串）：

```bash
git config --global credential.helper osxkeychain
```

**Windows**（使用凭据管理器）：

```bash
git config --global credential.helper manager
```

**Linux**（缓存一段时间，默认 15 分钟）：

```bash
git config --global credential.helper cache
# 或永久存储（明文，注意安全）
git config --global credential.helper store
```

---

## 同时使用 GitHub 和 Gitee（多平台 SSH 配置）

如果本机同时使用 GitHub 和 Gitee，可以在 `~/.ssh/config` 中分别指定密钥：

```
# Gitee
Host gitee.com
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_ed25519_gitee

# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github
```

对应生成两对密钥时指定不同文件名：

```bash
ssh-keygen -t ed25519 -C "gitee邮箱" -f ~/.ssh/id_ed25519_gitee
ssh-keygen -t ed25519 -C "github邮箱" -f ~/.ssh/id_ed25519_github
```

---

## 常见问题

| 问题 | 解决方法 |
|------|---------|
| `Permission denied (publickey)` | 检查 SSH 公钥是否正确添加到 Gitee，或重新运行 `ssh -T git@gitee.com` 测试 |
| `fatal: Authentication failed` | HTTPS 方式下，确认使用的是私人令牌而不是 Gitee 登录密码；或改用 SSH 方式 |
| SSH 密钥冲突（同时使用 GitHub/Gitee） | 参考上方"多平台 SSH 配置"章节，为不同平台分别指定密钥文件 |

---

> 整理：@yangyudtx
