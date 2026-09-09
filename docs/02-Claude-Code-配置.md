# Claude Code 小白配置

本页按照平台 Docs 的 Anthropic 兼容方式配置 Claude Code。Claude Code 使用 `/v1/messages`，所以 `ANTHROPIC_BASE_URL` 是服务根地址，末尾**不要**加 `/v1`。

示例使用 `https://gate.bibilabu.cc`。这是作者当前使用的平台演示地址；使用其他平台时，请替换成你自己的 API 地址和 Key。

## 先完成 Node.js 前置环境

若你还没有安装 Node.js 和 npm，先看 [Node.js 与 npm 前置环境](00-Node.js与npm前置环境.md)。完成后再按当前页面执行。

## Windows 10/11

### 1. 安装 Claude Code

打开“终端（管理员）”或“Windows PowerShell（管理员）”，执行：

```powershell
npm install -g @anthropic-ai/claude-code
```

### 2. 验证安装

```powershell
claude --version
```

看到版本号说明安装成功。

### 3. 创建并打开配置文件

在文件资源管理器地址栏输入下面路径并回车：

```text
%USERPROFILE%\.claude
```

若系统提示该文件夹不存在，就在 `%USERPROFILE%` 下手动新建名为 `.claude` 的文件夹。进入后，新建一个文本文件，并把文件名完整改为：

```text
settings.json
```

不要让文件变成 `settings.json.txt`。用记事本打开它。

### 4. 粘贴配置

把文件原有内容全部删除，再粘贴下面内容。只把 `YOUR_API_KEY` 换成自己的 Key；若不是使用本文演示平台，也把域名改为自己的平台地址。

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "ANTHROPIC_API_KEY": ""
  }
}
```

按 `Ctrl + S` 保存并关闭记事本。`ANTHROPIC_API_KEY` 必须保持空字符串；填入其他值时，Claude Code 可能回退到官方 Anthropic。

### 5. 启动并选模型

新开一个普通 PowerShell，进入你的代码项目目录，例如：

```powershell
cd "C:\Users\你的用户名\Documents\我的项目"
claude
```

进入 Claude Code 后输入：

```text
/model
```

客户端会通过 `/v1/models` 读取当前 Key 可用的模型。选择模型后发送一条普通对话测试。

## macOS 和 Linux

打开终端，依次执行：

```bash
npm install -g @anthropic-ai/claude-code
claude --version
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

你正在编辑的文件是 `~/.claude/settings.json`。粘贴下面内容，只替换 `YOUR_API_KEY`：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "ANTHROPIC_API_KEY": ""
  }
}
```

在 nano 中按 `Ctrl + O` 保存、按回车确认文件名、按 `Ctrl + X` 退出。接着进入项目目录并启动：

```bash
cd ~/你的项目目录
claude
```

输入 `/model` 选择当前 Key 可用的模型，再发送一条普通对话测试。

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
| 使用了官方账号而不是中转站 | `ANTHROPIC_API_KEY` 是否仍为 `""` |
| 看不到预期模型 | 模型清单以你的平台 `/v1/models` 实时返回为准 |

其他问题见 [常见问题](05-常见问题.md)。
