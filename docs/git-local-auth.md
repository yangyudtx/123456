# 本地 Git 账号授权指南

本文介绍如何在已安装 Git 的本地环境中，完成 GitHub 账号的授权配置，以便正常执行 `push`、`pull` 等需要身份验证的操作。

---

## 第一步：配置 Git 用户信息

在终端中运行以下命令，将 `用户名` 和 `邮箱` 替换为你的 GitHub 账号信息：

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

验证配置是否生效：

```bash
git config --global --list
```

---

## 方式一：SSH 密钥授权（推荐）

### 1. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "你的GitHub注册邮箱"
```

一路按回车使用默认路径（`~/.ssh/id_ed25519`），也可以设置密码短语（passphrase）以增加安全性。

### 2. 将公钥添加到 GitHub

复制公钥内容：

```bash
# Linux / macOS
cat ~/.ssh/id_ed25519.pub

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub
```

然后：

1. 登录 GitHub → 右上角头像 → **Settings**
2. 左侧菜单选择 **SSH and GPG keys**
3. 点击 **New SSH key**
4. 填写标题（如 `my-laptop`），将复制的公钥粘贴到 **Key** 文本框，点击 **Add SSH key**

### 3. 测试 SSH 连接

```bash
ssh -T git@github.com
```

看到类似 `Hi 用户名! You've successfully authenticated...` 的提示即表示授权成功。

### 4. 使用 SSH 地址克隆或修改远程地址

克隆时使用 SSH 地址：

```bash
git clone git@github.com:用户名/仓库名.git
```

已有本地仓库，修改远程地址为 SSH：

```bash
git remote set-url origin git@github.com:用户名/仓库名.git
```

---

## 方式二：HTTPS + 个人访问令牌（Personal Access Token）

### 1. 生成 Personal Access Token（PAT）

1. 登录 GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. 点击 **Generate new token**，勾选 `repo` 权限（以及其他需要的权限）
3. 生成后**立即复制**，页面关闭后无法再次查看

### 2. 使用令牌进行认证

克隆或推送时，将令牌作为密码输入：

```
用户名：你的 GitHub 用户名
密码：粘贴上面生成的 PAT（不是 GitHub 登录密码）
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

## 常见问题

| 问题 | 解决方法 |
|------|---------|
| `Permission denied (publickey)` | 检查 SSH 公钥是否正确添加到 GitHub，或重新运行 `ssh -T git@github.com` 测试 |
| `remote: Support for password authentication was removed` | GitHub 已停止支持密码登录，请改用 SSH 密钥或 PAT |
| `fatal: Authentication failed` | HTTPS 方式下，确认使用的是 PAT 而不是 GitHub 账号密码 |

---

> 整理：@yangyudtx
