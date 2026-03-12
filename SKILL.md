---
name: github-upload
description: 从 Trea IDE 上传文件到 GitHub 仓库。当用户需要将本地项目或文件上传到 GitHub 时使用此技能，智能体将自主完成仓库创建、文件上传、认证处理等操作。已配置默认令牌（用户：2023winner），可直接使用或根据需要修改。
compatibility:
  - tools: [Read, Write, RunCommand]
  - dependencies: [requests, base64]
---

# GitHub 上传技能

## 功能描述

此技能允许智能体从 Trea IDE 直接上传文件到 GitHub 仓库，包括以下功能：

- 自动创建新的 GitHub 仓库
- 批量上传单个或多个文件到 GitHub
- 智能处理 GitHub 认证
- 自动解决常见的上传问题
- 支持环境变量、配置文件和 Git 设置等多种认证方式

## 已配置信息

**重要提示**：为了安全起见，此技能文件中的 GitHub 令牌已替换为占位符。

使用前您需要配置自己的 GitHub 认证信息：

```python
GITHUB_TOKEN = "ghp_your_token_here"  # 替换为您的 GitHub 个人访问令牌
GITHUB_OWNER = "your_username"        # 替换为您的 GitHub 用户名
GITHUB_EMAIL = "your@email.com"       # 替换为您的邮箱地址
```

**获取 GitHub 令牌的步骤**：
1. 访问 [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. 点击 "Generate new token (classic)"
3. 选择权限：至少勾选 `repo`（完整仓库控制权限）
4. 生成令牌并**立即复制保存**（关闭页面后将无法再次查看）
5. 将令牌填入上方的 `GITHUB_TOKEN` 位置

**安全提醒**：
- ⚠️ 永远不要将包含真实令牌的配置文件上传到 GitHub
- ✅ 使用 `.gitignore` 文件排除 `config.py` 等敏感文件
- 🔐 建议使用环境变量存储令牌

## 前置条件

在使用此技能之前，需要：

1. **GitHub 个人访问令牌**：用于认证 GitHub API 请求（已配置默认令牌）
2. **Python 环境**：需要安装 requests 库
3. **文件准备**：准备好要上传的文件

## 生成 GitHub 个人访问令牌

如果您需要使用自己的账号，请按以下步骤生成令牌：

1. 访问 [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. 点击 "Generate new token"
3. 选择适当的权限（至少需要 `repo` 权限）
4. 生成令牌并保存好

## 配置方式

### 方式一：使用环境变量（推荐）

在运行脚本前设置环境变量：

```bash
# Windows PowerShell
$env:GITHUB_TOKEN="ghp_your_token"
$env:GITHUB_OWNER="your_username"
$env:GITHUB_EMAIL="your@email.com"

# Windows CMD
set GITHUB_TOKEN=ghp_your_token
set GITHUB_OWNER=your_username
set GITHUB_EMAIL=your@email.com
```

### 方式二：使用配置文件

创建 `config.py` 文件（与脚本同目录）：

```python
# GitHub 个人访问令牌
GITHUB_TOKEN = "ghp_your_token"

# GitHub 用户名
GITHUB_OWNER = "your_username"

# 邮箱地址
GITHUB_EMAIL = "your@email.com"
```

### 方式三：直接修改脚本

在脚本中直接修改认证信息（仅用于测试）：

```python
GITHUB_TOKEN = "ghp_your_token"
OWNER = "your_username"
BRANCH = "main"
```

## 使用方法

### 方法一：智能体自主上传（推荐）

**智能体将自动完成以下步骤**：

1. **检测用户需求**：识别用户想要上传文件到 GitHub 的请求
2. **收集必要信息**：
   - GitHub 个人访问令牌（从环境变量、配置文件或用户输入获取）
   - GitHub 用户名（从环境变量、配置文件、Git 设置或用户输入获取）
   - 目标仓库名称（从用户输入获取）
   - 要上传的文件或目录路径（从用户输入或当前目录获取）
3. **执行上传操作**：
   - 自动创建 GitHub 仓库（如果不存在）
   - 批量上传文件到 GitHub
   - 处理上传过程中的错误和异常
4. **返回结果**：
   - 上传成功的文件列表和 GitHub 链接
   - 遇到的问题和解决方案

**用户只需提供**：
- GitHub 个人访问令牌（首次使用时）
- 目标仓库名称
- 要上传的文件或目录路径

### 方法二：使用 Python 脚本上传

1. **使用通用上传脚本**：
   ```bash
   # 上传单个文件
   python scripts/upload_to_github.py --repo 仓库名 --file 要上传的文件
   
   # 上传目录
   python scripts/upload_to_github.py --repo 仓库名 --directory 目录路径
   
   # 创建仓库并上传
   python scripts/upload_to_github.py --repo 仓库名 --file 要上传的文件 --create-repo
   ```

2. **配置选项**：
   - `--token`：GitHub 个人访问令牌
   - `--owner`：GitHub 用户名
   - `--repo`：仓库名称（必填）
   - `--branch`：分支名称，默认 main
   - `--file`：要上传的文件路径
   - `--directory`：要上传的目录路径
   - `--commit-message`：提交信息
   - `--create-repo`：是否创建新仓库
   - `--repo-description`：仓库描述

### 方法三：使用 Git 命令行上传

1. **初始化 Git 仓库**：
   ```bash
   git init
   ```

2. **设置 Git 用户信息**：
   ```bash
   git config user.name "你的 GitHub 用户名"
   git config user.email "你的邮箱地址"
   ```

3. **添加文件到暂存区**：
   ```bash
   git add .
   ```

4. **提交文件**：
   ```bash
   git commit -m "从 Trea IDE 上传文件"
   ```

5. **创建 GitHub 仓库**：
   在 GitHub 网站上创建一个新仓库

6. **添加远程仓库**：
   ```bash
   git remote add origin https://github.com/你的用户名/仓库名称.git
   ```

7. **推送文件**：
   ```bash
   git push https://你的 GitHub 令牌@github.com/你的用户名/仓库名称.git 分支名称
   ```

## 常见问题及解决方案

### 1. SSL 证书验证错误

**错误信息**：
```
ssl.SSLCertVerificationError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed
```

**解决方案**：
在 requests 请求中添加 `verify=False` 参数：
```python
response = requests.post(url, headers=headers, json=data, verify=False)
```

### 2. 仓库不存在

**错误信息**：
```
remote: Repository not found.
fatal: repository 'https://github.com/用户名/仓库名.git/' not found
```

**解决方案**：
先创建仓库，然后再推送文件。可以使用 GitHub API 或在 GitHub 网站上手动创建。

### 3. 认证失败

**错误信息**：
```
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/用户名/仓库名.git/'
```

**解决方案**：
确保使用正确的 GitHub 个人访问令牌，并且令牌具有足够的权限。

### 4. MCP GitHub 工具认证问题

**问题**：
Trea IDE 的内置 GitHub MCP 工具可能遇到认证问题，导致 API 调用失败。

**解决方案**：
使用 Python 脚本通过 GitHub API 直接上传文件，或使用 Git 命令行工具。

### 5. 命令解析问题

**问题**：
在使用 trae-sandbox 执行包含空格或特殊字符的命令时，可能会遇到解析错误。

**解决方案**：
- 使用更简单的命令格式
- 避免在命令中使用特殊字符和空格
- 对于复杂命令，使用 Python 脚本替代

### 6. 配置文件路径问题

**问题**：
脚本可能无法找到配置文件，特别是当脚本在子目录中时。

**解决方案**：
在脚本中添加路径处理，确保能够找到配置文件：
```python
import sys
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
import config
```

### 7. PowerShell 命令语法问题

**问题**：
PowerShell 不支持 `&&` 等 bash 风格的命令连接符。

**解决方案**：
分别执行每个命令，或使用 PowerShell 的语法：
```powershell
command1; command2; command3
```

### 8. 敏感文件上传问题

**问题**：
配置文件中包含敏感信息（如 GitHub 令牌），可能被误上传到 GitHub。

**解决方案**：
- 创建 `.gitignore` 文件，添加敏感文件到忽略列表
- 在上传脚本中添加逻辑，跳过敏感文件的上传
- 使用环境变量或加密的配置管理方案

### 9. 文件夹权限问题

**问题**：
在搬运其他项目后，可能遇到文件夹权限问题，导致无法写入 skill 文件。

**解决方案**：
- 以管理员身份运行 Trea IDE
- 检查文件夹权限设置，确保有写入权限
- 尝试在其他目录中创建和使用 skill
- 使用环境变量存储配置信息，避免写入文件

### 10. 本地设置检测问题

**问题**：
无法得知本地的 GitHub 设置，如用户名、邮箱等，导致上传时出现问题。

**解决方案**：
- 上传脚本会自动检测本地 Git 设置：
  ```bash
  # 查看当前 Git 设置
  git config user.name
  git config user.email
  
  # 设置 Git 信息
  git config --global user.name "你的用户名"
  git config --global user.email "你的邮箱"
  ```
- 使用环境变量存储配置：
  ```bash
  # Windows
  set GITHUB_TOKEN=你的令牌
  set GITHUB_OWNER=你的用户名
  set GITHUB_EMAIL=你的邮箱
  
  # Linux/Mac
  export GITHUB_TOKEN=你的令牌
  export GITHUB_OWNER=你的用户名
  export GITHUB_EMAIL=你的邮箱
  ```
- 脚本会按以下顺序查找配置：
  1. 命令行参数
  2. 环境变量
  3. 配置文件（config.py）
  4. Git 本地设置

### 11. 文件大小限制

**问题**：
GitHub API 对文件大小有限制（通常为 100MB）。

**解决方案**：
- 对于大文件，使用 Git 命令行上传
- 考虑使用 Git LFS（Large File Storage）
- 拆分大文件为多个小文件

### 12. 网络连接问题

**问题**：
网络连接不稳定或防火墙限制可能导致上传失败。

**解决方案**：
- 检查网络连接
- 尝试使用代理服务器
- 重试上传操作
- 确保 GitHub API 的访问权限

### 13. GitHub 推送保护错误

**问题**：
推送时遇到 GitHub 推送保护错误，提示"Push cannot contain secrets"，因为代码中包含了 GitHub 个人访问令牌等敏感信息。

**错误信息**：
```
remote: error: GH013: Repository rule violations found for refs/heads/master.
remote: 
remote: - GITHUB PUSH PROTECTION 
remote:   ————————————————————————————————————————— 
remote:     Resolve the following violations before pushing again 
remote: 
remote:     - Push cannot contain secrets 
remote: 
remote: 
remote:      (?) Learn how to resolve a blocked push 
remote:      `https://docs.github.com/code-security/secret-scanning/working-with-secret-scanning-and-push-protection/working-with-push-protection-from-the-command-line#resolving-a-blocked-push` 
remote: 
remote: 
remote:       —— GitHub Personal Access Token —————————————————————— 
remote:        locations: 
remote:          - commit: 113a24d1c9608cfc2caec271b39ae5566778df42 
remote:            path: create_repo.ps1:1
```

**解决方案**：

#### 方法一：移除最新提交中的密钥
1. 从代码中移除密钥
2. 运行 `git commit --amend --all` 更新原始提交
3. 使用 `git push` 推送更改

#### 方法二：移除早期提交中的密钥
1. 查看错误消息中列出的包含密钥的所有提交
2. 运行 `git log` 查看分支上的完整提交历史
3. 确定最早包含密钥的提交
4. 运行 `git rebase -i <最早提交 ID>~1` 开始交互式变基
5. 在编辑器中，将最早提交的 `pick` 改为 `edit`
6. 保存并关闭编辑器开始变基
7. 从代码中移除密钥
8. 运行 `git add .` 将更改添加到暂存区
9. 运行 `git commit --amend` 提交更改
10. 运行 `git rebase --continue` 完成变基
11. 使用 `git push` 推送更改

#### 方法三：绕过推送保护（不推荐）
1. 访问 GitHub 返回的 URL
2. 选择最能描述为什么应该允许推送的选项
3. 点击 "Allow me to push this secret"
4. 在三小时内重新尝试推送

**预防措施**：
- 使用环境变量存储敏感信息
- 使用配置文件并将其添加到 `.gitignore`
- 在上传前检查代码中是否包含敏感信息
- 使用 GitHub Actions 或其他 CI/CD 工具进行密钥检测

## 快速入门指南

### 5 分钟快速开始

**步骤 1：配置认证信息**

选择以下任一方式：

```bash
# 方式 A：使用环境变量（推荐）
set GITHUB_TOKEN=ghp_your_token_here
set GITHUB_OWNER=your_username
```

```python
# 方式 B：创建 config.py 文件
# GitHub 个人访问令牌
GITHUB_TOKEN = "ghp_your_token_here"
# GitHub 用户名
GITHUB_OWNER = "your_username"
# 邮箱地址
GITHUB_EMAIL = "your@email.com"
```

**步骤 2：使用技能上传文件**

在 Trea IDE 中对智能体说：
```
请帮我将当前项目上传到 GitHub，仓库名称为 "my-awesome-project"
```

智能体会自动：
- 检测您的配置
- 创建仓库（如不存在）
- 上传所有文件
- 返回仓库链接

---

## 详细使用示例

### 示例 1：智能体自主上传（最简单）

**用户请求**：
```
请帮我将当前项目上传到 GitHub，仓库名称为 "my-project"
```

**智能体操作**：
1. ✅ 检测用户需求：需要将当前项目上传到 GitHub
2. ✅ 收集信息：
   - 从环境变量或配置文件获取 GitHub 令牌和用户名
   - 确认仓库名称："my-project"
   - 确定要上传的目录：当前目录
3. ✅ 执行操作：
   - 检查是否存在名为 "my-project" 的仓库
   - 如果不存在，创建新仓库
   - 批量上传当前目录中的所有文件（自动跳过 `.git`、`__pycache__` 等）
   - 处理上传过程中的错误
4. ✅ 返回结果：
   ```
   成功将项目上传到 GitHub 仓库 "my-project"！
   
   上传的文件：
   - README.md → https://github.com/your-username/my-project/blob/main/README.md
   - main.py → https://github.com/your-username/my-project/blob/main/main.py
   - requirements.txt → https://github.com/your-username/my-project/blob/main/requirements.txt
   
   仓库地址：https://github.com/your-username/my-project
   ```

---

### 示例 2：上传指定文件

**用户请求**：
```
请帮我将 "data.csv" 文件上传到 GitHub 仓库 "my-datasets"
```

**智能体操作**：
1. ✅ 检测用户需求：需要上传指定文件到 GitHub
2. ✅ 收集信息：
   - 从环境变量或配置文件获取 GitHub 令牌和用户名
   - 确认仓库名称："my-datasets"
   - 确认要上传的文件："data.csv"
3. ✅ 执行操作：
   - 检查是否存在名为 "my-datasets" 的仓库
   - 如果不存在，创建新仓库
   - 上传 "data.csv" 文件
   - 处理上传过程中的错误
4. ✅ 返回结果：
   ```
   成功将文件 "data.csv" 上传到 GitHub 仓库 "my-datasets"！
   
   文件地址：https://github.com/your-username/my-datasets/blob/main/data.csv
   仓库地址：https://github.com/your-username/my-datasets
   ```

---

### 示例 3：使用 Python 脚本上传（命令行方式）

**场景 1：上传单个文件**
```bash
python scripts/upload_to_github.py --repo test-repo --file hello.py
```

**场景 2：创建仓库并上传**
```bash
python scripts/upload_to_github.py --repo new-repo --file hello.py --create-repo --repo-description "测试仓库"
```

**场景 3：上传整个目录**
```bash
python scripts/upload_to_github.py --repo my-project --directory .
```

**场景 4：指定所有参数**
```bash
python scripts/upload_to_github.py ^
  --token ghp_your_token ^
  --owner your_username ^
  --repo my-awesome-project ^
  --branch main ^
  --file main.py ^
  --remote-path src/main.py ^
  --commit-message "添加主程序文件" ^
  --create-repo ^
  --repo-description "我的超棒项目"
```

---

### 示例 4：批量上传项目文件

**用户请求**：
```
我想把这个 Python 项目上传到 GitHub，但排除配置文件和缓存文件
```

**智能体操作**：
1. ✅ 自动识别项目类型（Python 项目）
2. ✅ 智能过滤文件：
   - ✅ 上传：`.py` 文件、`README.md`、`requirements.txt`
   - ❌ 跳过：`__pycache__/`、`*.pyc`、`.env`、`config.py`、`.git/`
3. ✅ 创建仓库并上传
4. ✅ 返回结果：
   ```
   成功上传 15 个文件到 GitHub 仓库 "python-project"！
   
   已跳过敏感文件：
   - .env（包含密钥）
   - config.py（包含配置信息）
   - __pycache__/（缓存目录）
   
   仓库地址：https://github.com/your-username/python-project
   ```

---

### 示例 5：更新现有仓库文件

**用户请求**：
```
帮我更新 GitHub 仓库中的 README.md 文件
```

**智能体操作**：
1. ✅ 检测本地文件变化
2. ✅ 连接 GitHub API
3. ✅ 获取远程文件的 SHA 值
4. ✅ 更新文件内容
5. ✅ 返回结果：
   ```
   成功更新 README.md 文件！
   
   查看更新：https://github.com/your-username/my-project/commit/abc123
   ```

---

### 示例 6：多场景对话式上传

**对话 1：创建个人网站**
```
用户：我想创建一个展示我作品的网站
智能体：好的，我可以帮您创建一个 GitHub 仓库来托管您的作品集网站。
       请告诉我：
       1. 您想使用什么仓库名称？（例如：portfolio）
       2. 您有哪些文件需要上传？
       
用户：仓库叫 "my-portfolio"，我有 index.html、style.css 和 script.js
智能体：好的，正在为您创建仓库并上传文件...
       ✅ 仓库创建成功：https://github.com/your-username/my-portfolio
       ✅ 上传 3 个文件
       🎉 完成！您的网站已可以通过 GitHub Pages 访问
```

**对话 2：备份代码**
```
用户：帮我备份这个项目的代码到 GitHub
智能体：没问题！我会将当前项目的所有源代码文件上传到 GitHub。
       建议使用什么仓库名称？
       
用户：叫 "backup-2024" 吧
智能体：好的，正在创建私有仓库 "backup-2024" 并上传代码...
       ✅ 已上传 42 个文件
       ✅ 跳过 15 个非代码文件（图片、文档等）
       🔒 仓库设为私有，只有您可以访问
```

---

### 示例 7：自动化工作流

**场景：每日代码备份**

您可以创建定时任务，每天自动上传代码：

```python
# daily_backup.py
import os
import subprocess
from datetime import datetime

# 获取当前日期
today = datetime.now().strftime("%Y-%m-%d")

# 自动上传到 GitHub
repo_name = f"daily-backup-{today}"
command = f"""
python scripts/upload_to_github.py ^
  --repo {repo_name} ^
  --directory . ^
  --create-repo ^
  --repo-description "每日自动备份 {today}"
"""

subprocess.run(command, shell=True)
```

配合 Windows 任务计划程序，实现全自动备份！

---

## 常见问题解答

### Q1: 第一次使用，不知道如何获取令牌？
**A:** 按照以下步骤：
1. 登录 GitHub
2. 访问 [https://github.com/settings/tokens](https://github.com/settings/tokens)
3. 点击 "Generate new token (classic)"
4. 填写备注（如 "Trea IDE"）
5. 勾选 `repo` 权限
6. 点击生成并**立即复制令牌**

### Q2: 上传失败怎么办？
**A:** 检查以下几点：
- ✅ 令牌是否正确且未过期
- ✅ 网络连接是否正常
- ✅ 仓库名称是否合法（不能包含特殊字符）
- ✅ 文件大小是否超过 100MB

### Q3: 可以上传私有仓库吗？
**A:** 可以！在创建仓库时添加参数：
```python
data = {
    "name": repo_name,
    "private": True  # 设为私有仓库
}
```

### Q4: 如何上传大文件（>100MB）？
**A:** GitHub API 限制单个文件最大 100MB。大文件建议使用 Git 命令行：
```bash
git lfs install
git lfs track "*.zip"
git add .
git commit -m "添加大文件"
git push origin main
```

## 注意事项

1. **安全**：不要在代码中硬编码 GitHub 个人访问令牌，建议使用环境变量或配置文件
2. **权限**：确保 GitHub 个人访问令牌具有足够的权限
3. **文件大小**：GitHub API 对文件大小有限制，大文件建议使用 Git 命令行上传
4. **SSL 验证**：在生产环境中，建议启用 SSL 验证，而不是使用 `verify=False`

## 故障排除

如果遇到上传问题，请检查：

1. GitHub 个人访问令牌是否有效
2. 网络连接是否正常
3. 仓库名称是否正确
4. 文件路径是否存在
5. 权限是否足够

如果问题仍然存在，请参考 GitHub API 文档或联系 GitHub 支持。
