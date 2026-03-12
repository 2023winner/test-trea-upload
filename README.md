# GitHub Upload Skill 🔼

一个强大的 Trea IDE 技能，用于将文件上传到 GitHub 仓库。

## ✨ 功能特性

- 🚀 **一键上传**：智能体自动完成仓库创建和文件上传
- 📦 **批量上传**：支持上传单个文件或整个目录
- 🔐 **安全认证**：支持环境变量、配置文件等多种认证方式
- 🎯 **智能过滤**：自动跳过敏感文件和缓存文件
- 🔄 **更新支持**：支持更新现有文件
- 📝 **详细日志**：清晰的上传进度和结果反馈

## 📋 前置要求

- Python 3.6+
- GitHub 账号
- GitHub 个人访问令牌
- `requests` 库

## 🔧 安装步骤

### 1. 安装依赖

```bash
pip install requests
```

### 2. 获取 GitHub 令牌

1. 访问 [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. 点击 "Generate new token (classic)"
3. 填写备注（如 "Trea IDE"）
4. 勾选 `repo` 权限（完整仓库控制权限）
5. 点击生成并**立即复制令牌**

### 3. 配置认证信息

**方式一：使用环境变量（推荐）**

```bash
# Windows PowerShell
$env:GITHUB_TOKEN="ghp_your_token_here"
$env:GITHUB_OWNER="your_username"

# Windows CMD
set GITHUB_TOKEN=ghp_your_token_here
set GITHUB_OWNER=your_username
```

**方式二：创建配置文件**

创建 `config.py` 文件：

```python
# GitHub 个人访问令牌
GITHUB_TOKEN = "ghp_your_token_here"

# GitHub 用户名
GITHUB_OWNER = "your_username"

# 邮箱地址
GITHUB_EMAIL = "your@email.com"
```

⚠️ **重要**：`config.py` 已添加到 `.gitignore`，不会被上传到 GitHub

## 📖 使用方法

### 方法一：在 Trea IDE 中使用智能体

在 Trea IDE 中对智能体说：

```
请帮我将当前项目上传到 GitHub，仓库名称为 "my-project"
```

智能体会自动：
1. 检测您的配置
2. 创建仓库（如不存在）
3. 上传所有文件（自动跳过敏感文件）
4. 返回仓库链接

### 方法二：使用命令行脚本

**上传单个文件：**
```bash
python scripts/upload_to_github.py --repo my-repo --file main.py
```

**上传整个目录：**
```bash
python scripts/upload_to_github.py --repo my-repo --directory .
```

**创建仓库并上传：**
```bash
python scripts/upload_to_github.py --repo new-repo --file hello.py --create-repo
```

**完整参数示例：**
```bash
python scripts/upload_to_github.py ^
  --token ghp_your_token ^
  --owner your_username ^
  --repo my-project ^
  --branch main ^
  --file main.py ^
  --commit-message "添加主程序" ^
  --create-repo ^
  --repo-description "我的项目"
```

## 📚 使用场景

### 场景 1：创建个人网站
```
用户：我想创建一个展示我作品的网站
智能体：好的，请告诉我仓库名称和需要上传的文件...
```

### 场景 2：备份代码
```
用户：帮我备份这个项目的代码到 GitHub
智能体：没问题！建议使用什么仓库名称？...
```

### 场景 3：每日自动备份
创建定时任务，每天自动上传代码到 GitHub。

## 🛡️ 安全提示

- ⚠️ **永远不要**将包含真实令牌的 `config.py` 上传到 GitHub
- ✅ 使用 `.gitignore` 排除敏感文件
- 🔐 建议使用环境变量存储令牌
- 🔄 定期更新您的 GitHub 令牌

## 📝 文件说明

```
github-upload/
├── SKILL.md              # 技能说明文件（可安全分享）
├── README.md             # 项目说明文档
├── .gitignore            # Git 忽略文件配置
├── scripts/
│   └── upload_to_github.py   # 通用上传脚本
├── evals/
│   └── evals.json        # 测试用例
└── config.py             # 本地配置文件（不包含在 Git 中）
```

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 许可证

MIT License

## 🔗 相关链接

- [GitHub API 文档](https://docs.github.com/en/rest)
- [Trea IDE 文档](https://trae.dev)
- [GitHub 令牌管理](https://github.com/settings/tokens)

## 💡 提示

如果您在使用过程中遇到任何问题，请查看 [SKILL.md](SKILL.md) 中的详细故障排除指南。
