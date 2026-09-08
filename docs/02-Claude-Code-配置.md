# Claude Code 小白配置

这篇教程配置的是 Claude Code。Claude Code 走 Anthropic 兼容路径，Base URL 末尾**不要**加 `/v1`。

示例使用 `https://gate.bibilabu.cc`。这是作者当前使用的平台演示地址；你使用其他平台时，请替换成自己的 API 地址和 Key。

## 配置前检查

先在终端执行下面命令：

```text
claude --version
```

能看到版本号，说明 Claude Code 已安装，可以继续。若命令不存在，请先按 Claude Code 官方安装说明完成安装，再回到本页执行下面的配置步骤。

## Windows 10/11

### 1. 创建配置目录和文件

按 `Win + R`，输入 `powershell` 并回车。把下面两行逐行粘贴到 PowerShell，执行后会创建配置目录并用记事本打开配置文件：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude" | Out-Null
notepad "$env:USERPROFILE\.claude\settings.json"
```

你正在编辑的文件是：

```text
%USERPROFILE%\.claude\settings.json
```

### 2. 粘贴配置

把记事本中的旧内容全部删除，再粘贴下面内容。只把 `YOUR_API_KEY` 替换成你自己的 Key；若你不是使用本文演示平台，也把地址替换成自己的平台地址。

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "ANTHROPIC_API_KEY": ""
  }
}
```

按 `Ctrl + S` 保存，然后关闭记事本。

### 3. 启动并选模型

在 PowerShell 中执行：

```powershell
claude
```

进入 Claude Code 后输入：

```text
/model
```

从列表中选择你平台当前提供的模型。看到模型列表并能发出一条普通对话，即表示配置成功。

## macOS 和 Linux

打开终端，执行：

```bash
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

这会编辑文件 `~/.claude/settings.json`。粘贴下面内容，只替换 `YOUR_API_KEY`，然后按 `Ctrl + O` 保存、按回车确认文件名、按 `Ctrl + X` 退出：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "ANTHROPIC_API_KEY": ""
  }
}
```

继续执行：

```bash
claude
```

进入后输入 `/model`，选中你的平台提供的模型，再发送一条普通对话测试。

## 成功标准

- `claude --version` 能显示版本。
- 输入 `/model` 后能看到模型列表。
- 发送普通对话后得到正常回复，而不是认证或地址错误。

## 常见错误

| 现象 | 先检查什么 |
| --- | --- |
| 认证失败或 401 | `ANTHROPIC_AUTH_TOKEN` 是否已替换为你自己的有效 Key；Key 前后是否多了空格或引号 |
| 404、路径不存在 | `ANTHROPIC_BASE_URL` 最后不能写 `/v1` |
| 改完仍使用旧配置 | 完全退出 Claude Code 后重新执行 `claude` |
| 看不到预期模型 | 模型清单以你的平台 `/v1/models` 实时返回为准 |

其他问题见 [常见问题](05-常见问题.md)。
