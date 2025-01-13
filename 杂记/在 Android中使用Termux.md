

在 **Termux** 中安装和配置 Git，可以按照以下步骤操作：

---

### **2. 更新 Termux 包管理器**

在 Termux 中，首次使用前需要更新软件包管理器。

#### **命令**

```bash
pkg update && pkg upgrade
```

- `pkg update`：更新包列表。
- `pkg upgrade`：升级已安装的包。

---

### **4. 配置 Git**

在安装 Git 后，你需要设置用户信息，这是进行 Git 提交时必须的配置。

#### **配置用户名和邮箱**

```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

#### **验证配置**

运行以下命令查看当前的 Git 配置：

```bash
git config --list
```
---

### **5. 配置 SSH（可选）**

如果你需要使用 GitHub、GitLab 或其他远程仓库，建议配置 SSH 密钥以方便操作。

#### **安装 OpenSSH**

```bash
pkg install openssh
```

#### **生成 SSH 密钥**

1. 生成密钥：
    
    ```bash
    ssh-keygen -t ed25519 -C "你的邮箱"
    ```
    
    - 按提示保存密钥（默认路径为 `~/.ssh/id_ed25519`）。
2. 查看生成的公钥：
    
    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```
    
    - 复制公钥内容。
3. 将公钥添加到远程仓库（如 GitHub）：
    
    - 登录 GitHub，进入 **Settings > SSH and GPG keys > New SSH Key**。
    - 将公钥粘贴到对应位置。

#### **测试 SSH 连接**

测试是否成功连接到 GitHub：

```bash
ssh -T git@github.com
```
---

### **7. 访问 Android 公共存储**

如果你需要将 Git 仓库存储在 Android 的公共存储目录（如 `/storage/emulated/0/`），需要授予 Termux 存储权限。

#### **授予存储权限**

运行以下命令：

```bash
termux-setup-storage
```

- Termux 会创建一个符号链接 `~/storage`，映射到 Android 公共存储。
- 目录结构：
    
    ```
    ~/storage/shared/        # 映射到 /storage/emulated/0/
    ~/storage/downloads/     # 映射到 /storage/emulated/0/Download/
    ~/storage/dcim/          # 映射到 /storage/emulated/0/DCIM/
    ~/storage/music/         # 映射到 /storage/emulated/0/Music/
    ```
    

#### **将 Git 仓库克隆到公共存储**

1. 进入公共存储目录：
    
    ```bash
    cd ~/storage/shared
    ```
    
2. 克隆仓库：
    
    ```bash
    git clone git@github.com:username/repository.git
    ```

---

### **总结步骤**

1. 在 Termux 中运行以下命令安装 Git：
    
    ```bash
    pkg update && pkg upgrade
    pkg install git
    ```
    
2. 配置用户名和邮箱：
    
    ```bash
    git config --global user.name "你的用户名"
    git config --global user.email "你的邮箱"
    ```
    
3. 配置 SSH 密钥（如需远程仓库操作）。
    
4. 克隆、提交或推送代码到远程仓库。
    