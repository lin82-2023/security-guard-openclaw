# 🔒 Security Guard for OpenClaw

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Security-brightgreen.svg)](https://github.com/openclaw-io/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**中文 | [English](#-security-guard-for-openclaw-1)**

带密码保护、防暴力破解、自动上锁的 OpenClaw 全局安全守卫，防止共享部署的 AI Agent 泄露敏感信息。

## ✨ 特性

- 🔐 **密码保护**：默认锁定，只有输入正确密码才能解锁查看敏感信息
- 🛡️ **安全存储**：密码仅保存 SHA-256 哈希，不存明文，配置文件泄露也不怕
- 🚫 **防暴力破解**：连续错误 N 次自动锁定（默认 3 次）
- ⏰ **自动上锁**：解锁后闲置一段时间自动切回安全模式（默认 10 分钟）
- 🔑 **安全改密**：修改密码必须验证原密码，防止他人私自篡改
- 🧹 **零侵入**：标准 OpenClaw Skill 格式，热插拔即用，不修改主程序
- 🎯 **精准过滤**：仅拦截含敏感关键词的回复，不影响正常聊天，对用户透明

## 📦 安装

1. 克隆到你的 OpenClaw 技能目录：
```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/你的用户名/security_guard.git
```

2. 重启 OpenClaw / QClaw，技能会自动加载

3. 首次启动会引导你设置密码，按照提示操作即可

## 📖 使用说明

### 基本操作

| 操作 | 指令 | 说明 |
|------|------|------|
| 解锁 | 直接输入密码 | 解锁后可以访问敏感信息 |
| 上锁 | 输入 `锁定` / `上锁` / `lock` | 立刻切回安全模式 |
| 修改密码 | 解锁后输入 `修改密码` / `change password` | 需要验证原密码才能修改 |
| 忘记密码 | 手动操作 | 清空技能文件中 `PASSWORD_HASH = ""`，保存重启重新设置 |

### 配置说明

打开 `security_guard.skill.json`，修改头部配置：

```python
# ========== 🔐 安全配置 ==========
# PASSWORD_HASH: SHA-256 哈希值，不是明文密码，安全!
# 生成方法: 在线 SHA-256 工具 -> 输入密码 -> 复制哈希粘贴到这里
# 真忘记密码: 直接清空 PASSWORD_HASH = "" 保存重启，相当于出厂重置
PASSWORD_HASH = ""                  # 密码 SHA-256 哈希，空=未初始化
AUTO_LOCK_SECONDS = 600             # 自动上锁时间（秒），默认 10 分钟
MAX_WRONG_TIMES = 3                 # 最多允许错误次数
# ================================
```

### 敏感词过滤

默认拦截包含以下关键词的回复，你可以在代码中修改 `SENSITIVE` 列表添加自定义敏感词：

```python
SENSITIVE = [
    '系统提示', 'system prompt', '技能列表', '你的技能', '技能目录',
    '思考过程', '内部规则', '配置文件', '路径', 'MEMORY.md',
    '你的指令', '你的限制', '工具调用', 'workflow', 'prompt',
    'SOUL.md', 'IDENTITY.md', 'USER.md', 'BOOTSTRAP.md'
]
```

## 🧩 配套技能：debug_info

推荐搭配 [`debug_info`](../debug_info/) 技能使用，解锁后输入 `debug` 即可查看：
- 所有已安装技能列表
- 当前安全守卫状态（是否锁定、剩余解锁时间）
- 当前会话信息

## 🔧 适用场景

- 👥 **共享机器人**：多人共用一个机器人，只有管理员能解锁调试
- 🚀 **公共服务器部署**：防止未授权访问拿到系统提示和敏感配置
- 🔒 **隐私保护**：避免 AI 不小心泄露内部配置和记忆文件路径

## 📝 安全提示

1. 请使用强度足够的密码（至少 8 位混合字符）
2. 如果发现配置文件泄露，请及时修改密码
3. 忘记密码只能通过清空 `PASSWORD_HASH` 重置，这是设计上的安全保证

## 📜 License

MIT License - see [LICENSE](LICENSE) for details

---

# 🔒 Security Guard for OpenClaw

**English | [中文](#-security-guard-for-openclaw)**

A global security guard for OpenClaw with password protection, brute-force prevention and auto-lock, prevents sensitive information leakage on shared-deployment AI Agents.

## ✨ Features

- 🔐 **Password Protection**: Locked by default, only correct password can unlock to access sensitive information
- 🛡️ **Secure Storage**: Only stores SHA-256 hash of password, plaintext never saved, safe even if config is leaked
- 🚫 **Brute-force Protection**: Auto-lock after N failed attempts (default 3)
- ⏰ **Auto-lock**: Automatically return to security mode after idle (default 10 minutes)
- 🔑 **Secure Password Change**: Must verify old password to change, prevents unauthorized modification
- 🧹 **Non-intrusive**: Standard OpenClaw Skill format, hot-plug and play, no changes to main code
- 🎯 **Precise Filtering**: Only blocks responses containing sensitive keywords, no impact on normal chat

## 📦 Installation

1. Clone to your OpenClaw skills directory:
```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/your-username/security_guard.git
```

2. Restart OpenClaw / QClaw, skill will be loaded automatically

3. First boot will guide you to set password, follow the prompt

## 📖 Usage

### Basic Commands

| Action | Command | Description |
|--------|---------|-------------|
| Unlock | Input your password directly | Unlock to access sensitive information |
| Lock | Input `锁定` / `上锁` / `lock` | Immediately return to security mode |
| Change Password | Input `修改密码` / `change password` after unlock | Must verify old password to change |
| Forgot Password | Manual | Empty `PASSWORD_HASH = ""` in skill file, save and restart to reset |

### Configuration

Open `security_guard.skill.json`, modify header config:

```python
# ========== 🔐 Security Config ==========
# PASSWORD_HASH: SHA-256 hash of your password, NOT plaintext, safe!
# How to generate: Use any online SHA-256 tool -> input password -> copy hash here
# If you forgot password: Just empty PASSWORD_HASH = "", save and restart to reset
PASSWORD_HASH = ""                  # Password SHA-256 hash, empty = not initialized
AUTO_LOCK_SECONDS = 600             # Auto-lock after (seconds), default 10 minutes
MAX_WRONG_TIMES = 3                 # Maximum allowed failed attempts
# ========================================
```

### Sensitive Words Filtering

By default blocks responses containing these keywords. You can modify the `SENSITIVE` list to add custom sensitive words:

```python
SENSITIVE = [
    '系统提示', 'system prompt', '技能列表', 'your skills', ...
]
```

## 🧩 Companion Skill: debug_info

Recommended to use with [`debug_info`](../debug_info/) skill. After unlocking, input `debug` to see:
- List of all installed skills
- Current security guard status (locked/unlocked, remaining unlock time)
- Current session information

## 🔧 Use Cases

- 👥 **Shared Bots**: Multiple people share one robot, only admin can unlock for debugging
- 🚀 **Public Server Deployment**: Prevents unauthorized access to system prompts and sensitive configurations
- 🔒 **Privacy Protection**: Avoids AI accidentally leaking internal configuration and memory file paths

## 📝 Security Notes

1. Please use a strong password (at least 8 mixed characters)
2. If your config file is leaked, change password immediately
3. Forgot password can only be reset by emptying `PASSWORD_HASH`, this is a security design

## 📜 License

MIT License - see [LICENSE](LICENSE) for details
