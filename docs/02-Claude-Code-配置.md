# Claude Code 小白配置

本页用于把 Claude Code 接到 **Anthropic Messages 兼容** 的平台。示例地址 `https://gate.bibilabu.cc` 是作者当前使用的平台；使用其他平台时，必须整体替换为自己的 API 地址、Key 和模型名。

Claude Code 会自行补上 `/v1/messages`，所以 `ANTHROPIC_BASE_URL` 只填写服务根地址，末尾不要加 `/v1`。

## 先确认认证方式

先向你的平台确认 Key 应放在哪个请求头中：

- 平台要求 `Authorization: Bearer ...`：只使用 `ANTHROPIC_AUTH_TOKEN`，本页默认采用此方式。
- 平台要求 `x-api-key`：只使用 `ANTHROPIC_API_KEY`，并把下方配置中的 `ANTHROPIC_AUTH_TOKEN` 整行替换掉。

两种变量不要同时设置，也不要为了“占位”把另一种变量写成空字符串。

## Windows 10/11

### 1. 安装并验证 Claude Code

打开“终端（管理员）”或“Windows PowerShell（管理员）”，执行：

```powershell
npm install -g @anthropic-ai/claude-code
claude --version
```

看到版本号说明安装成功。

### 2. 创建配置文件

在普通 PowerShell 中依次执行：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude"
New-Item -ItemType File -Force "$env:USERPROFILE\.claude\settings.json"
notepad "$env:USERPROFILE\.claude\settings.json"
```

打开的是 `%USERPROFILE%\.claude\settings.json`。不要把它保存成 `settings.json.txt`。

### 3. 粘贴配置

把文件原有内容全部删除，再粘贴下方 JSON。只替换 `YOUR_API_KEY`；若不是使用本文演示平台，也替换域名。

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY"
  }
}
```

上面是 Bearer 网关的完整配置。若你的平台明确要求 `x-api-key`，不要同时保留两种 Key 变量，改用下方完整配置：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_API_KEY": "YOUR_API_KEY"
  }
}
```

按 `Ctrl + S` 保存并关闭记事本。

### 4. 先验证 Messages 请求

新开一个普通 PowerShell，执行下方命令并替换 Key 与模型名。这一步直接验证 Claude Code 实际使用的 `/v1/messages` 路径。

```powershell
curl.exe -X POST "https://gate.bibilabu.cc/v1/messages" `
  -H "Authorization: Bearer YOUR_API_KEY" `
  -H "anthropic-version: 2023-06-01" `
  -H "Content-Type: application/json" `
  -d '{"model":"YOUR_MODEL_ID","max_tokens":1,"messages":[{"role":"user","content":"."}]}'
```

响应中出现 `id` 和 `content`，说明地址、Key 和 Messages 请求格式均可用。若返回“模型不存在”，也说明认证已经通过，应回到平台控制台或模型列表确认实际模型名。

如果你的平台要求 `x-api-key`，只运行下面这条替代命令，不要继续使用上面的 `Authorization: Bearer` 命令：

```powershell
curl.exe -X POST "https://gate.bibilabu.cc/v1/messages" `
  -H "x-api-key: YOUR_API_KEY" `
  -H "anthropic-version: 2023-06-01" `
  -H "Content-Type: application/json" `
  -d '{"model":"YOUR_MODEL_ID","max_tokens":1,"messages":[{"role":"user","content":"."}]}'
```

### 5. 启动 Claude Code

进入代码项目目录并启动：

```powershell
cd "C:\Users\你的用户名\Documents\我的项目"
claude
```

进入后先输入 `/status`。其中的 `Anthropic base URL` 应显示你的平台地址，认证来源应显示你设置的凭据变量。然后发送一条普通消息。

### 6. 可选：在 `/model` 中显示网关模型

只有当平台确认支持模型发现，且你需要在 `/model` 里选择非内置模型时，才在当前 JSON 的 `env` 对象中加入 `"CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1"`。为避免漏写逗号，Bearer 网关可直接改为以下完整内容；`x-api-key` 网关则将其中的 `ANTHROPIC_AUTH_TOKEN` 换成 `ANTHROPIC_API_KEY`：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://gate.bibilabu.cc",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",
    "CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY": "1"
  }
}
```

保存后完全退出 Claude Code，再重新启动并输入 `/model`。即使模型列表为空，也先以实际消息能否返回作为连通性判断。

## macOS 和 Linux

打开终端，执行：

```bash
npm install -g @anthropic-ai/claude-code
claude --version
mkdir -p ~/.claude
nano ~/.claude/settings.json
```

在 `~/.claude/settings.json` 中粘贴与 Windows 相同的 JSON。按 `Ctrl + O` 保存、回车确认、`Ctrl + X` 退出。

验证请求时，用下方命令替换地址、Key 和模型名：

```bash
curl -X POST "https://gate.bibilabu.cc/v1/messages" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{"model":"YOUR_MODEL_ID","max_tokens":1,"messages":[{"role":"user","content":"."}]}'
```

通过后进入项目目录执行 `claude`，先用 `/status` 确认地址和认证来源，再发送普通消息。

若你的平台要求 `x-api-key`，将配置中的 `ANTHROPIC_AUTH_TOKEN` 换成 `ANTHROPIC_API_KEY`，并把验证命令中的这一行：

```text
-H "Authorization: Bearer YOUR_API_KEY"
```

替换为：

```text
-H "x-api-key: YOUR_API_KEY"
```

需要在 `/model` 中显示网关模型时，按 Windows 部分的“可选：在 `/model` 中显示网关模型”把模型发现变量加入 JSON，然后重新启动 Claude Code。

## 成功标准

- `claude --version` 能显示版本。
- `/status` 显示预期的 Base URL 和认证来源。
- `/v1/messages` 最小请求或 Claude Code 内的普通消息获得正常回复。
- `/model` 仅在平台支持模型发现且已启用时作为模型选择入口；不能只凭它判断消息调用一定成功。

## 常见错误

| 现象 | 先检查什么 |
| --- | --- |
| `401` | Key 是否完整；认证变量是否与平台要求的请求头匹配 |
| `404` | `ANTHROPIC_BASE_URL` 中是否误写了 `/v1`；验证地址是否为 `/v1/messages` |
| `/status` 未显示平台地址 | 是否编辑了 `%USERPROFILE%\.claude\settings.json` 或 `~/.claude/settings.json`；修改后是否重新启动 |
| 看不到预期模型 | 是否已设置 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`；平台是否提供模型发现接口 |
| `/model` 有模型但消息失败 | 用本页的 `/v1/messages` 最小请求检查模型名、认证和请求格式 |

其他 HTTP 状态码见 [API 报错排查](troubleshooting-api-errors.md)。
