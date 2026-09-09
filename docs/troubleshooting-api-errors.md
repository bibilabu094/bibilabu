# API 报错排查

遇到报错时不要同时改地址、Key、模型和配置文件。先记录错误码、客户端、请求路径和模型名，然后每次只改一项并重新验证。

示例中的 `https://gate.bibilabu.cc`、`YOUR_API_KEY` 和 `YOUR_MODEL_ID` 都是演示值。使用其他平台时，必须替换为自己的地址、Key 和模型名。

## 先做四项快速检查

1. **客户端和路径是否匹配**：Claude Code 使用服务根地址并请求 `/v1/messages`；Codex 使用带 `/v1` 的 `base_url` 并请求 `/v1/responses`。
2. **认证方式是否唯一**：Bearer 网关只用 `ANTHROPIC_AUTH_TOKEN`；`x-api-key` 网关只用 `ANTHROPIC_API_KEY`。Codex 使用 `env_key` 指向的环境变量。
3. **模型是否匹配接口**：模型名以平台控制台为准。能出现在列表中，不代表一定支持 `/v1/messages` 或 `/v1/responses`。
4. **修改是否已生效**：Claude Code 修改 `settings.json` 后完全退出并重启；Windows 设置 Codex 环境变量后打开新 PowerShell；macOS/Linux 在运行 Codex 的同一终端先执行 `export`。

## Claude Code 最小验证

不要只调用 `/v1/models`。Claude Code 实际使用 Messages 协议，应先验证 `/v1/messages`。

### Windows PowerShell

```powershell
curl.exe -X POST "https://gate.bibilabu.cc/v1/messages" `
  -H "Authorization: Bearer YOUR_API_KEY" `
  -H "anthropic-version: 2023-06-01" `
  -H "Content-Type: application/json" `
  -d '{"model":"YOUR_MODEL_ID","max_tokens":1,"messages":[{"role":"user","content":"."}]}'
```

### macOS / Linux

```bash
curl -X POST "https://gate.bibilabu.cc/v1/messages" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{"model":"YOUR_MODEL_ID","max_tokens":1,"messages":[{"role":"user","content":"."}]}'
```

响应包含 `id` 和 `content` 即表示基本链路可用。若返回“未知模型”，URL 和 Key 仍可能正确，应确认模型 ID。

以上命令适用于要求 `Authorization: Bearer` 的网关。若你的平台要求 `x-api-key`，配置文件中只保留 `ANTHROPIC_API_KEY`，并将命令里的 `Authorization: Bearer YOUR_API_KEY` 请求头替换为 `x-api-key: YOUR_API_KEY`。不要在两种认证变量之间来回混用。

通过后启动 `claude`，输入 `/status`。其中应显示预期的 `Anthropic base URL` 和认证来源。只有平台支持模型发现且你需要在 `/model` 里选择非内置模型时，才在 `settings.json` 的 `env` 对象中设置 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`，然后完全退出并重新启动 Claude Code。

## Codex 快速检查

打开 `%USERPROFILE%\.codex\config.toml`（macOS/Linux 为 `~/.codex/config.toml`），确认核心内容为：

```toml
model_provider = "bibilabu"

[model_providers.bibilabu]
base_url = "https://gate.bibilabu.cc/v1"
env_key = "BIBILABU_API_KEY"
wire_api = "responses"
requires_openai_auth = false
```

然后确认启动 Codex 的进程能读取 `BIBILABU_API_KEY`：

```powershell
# Windows：先设置后关闭当前 PowerShell，再打开新窗口运行 codex
[Environment]::SetEnvironmentVariable("BIBILABU_API_KEY", "YOUR_API_KEY", "User")
```

```bash
# macOS/Linux：在运行 codex 的同一个终端执行
export BIBILABU_API_KEY="YOUR_API_KEY"
```

不要创建 `auth.json`，不要添加 `preferred_auth_method` 或 `discover_models`；`requires_openai_auth` 必须为 `false`。若仍失败，确认所选模型确实支持 `/v1/responses`。

## 按状态码处理

| 状态码或现象 | 常见原因 | 先做什么 |
| --- | --- | --- |
| `400` | JSON 格式、必填参数、模型名或请求体不符合接口要求 | 用最小请求重试；检查 `model`、`messages`、`max_tokens` 和 `anthropic-version` |
| `401` | Key 错误、认证变量和请求头不匹配、环境变量未生效 | Claude Code 核对单一认证变量；Codex 核对 `env_key` 与环境变量名，再开新终端 |
| `403` | Key 无权限、账户或模型权限受限、网络访问限制 | 到平台控制台确认 Key 状态、模型授权和账户限制；不要连续更换 Key |
| `404` | Base URL 或路径写错 | Claude Code 的 Base URL 不带 `/v1`；验证请求使用 `/v1/messages`；Codex 的 Base URL 必须带 `/v1` |
| `408`、超时 | 网络不稳定、上游处理较慢、流式连接中断 | 保留错误时间、模型名和请求 ID；先用 `max_tokens: 1` 的非流式请求缩小范围 |
| `429` | 频率或额度限制 | 降低并发和重试频率，确认余额、速率限制或平台配额 |
| `5xx`、`503` | 平台或上游暂时不可用、指定模型故障 | 不要删除本地配置；记录时间和模型，稍后用最小请求重试或换已确认兼容的模型 |
| `/model` 没有模型 | 平台未提供模型发现、发现未开启、模型权限不足 | 先用实际消息验证；确认 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`，再检查平台说明 |

## 仍无法解决时，提供这些脱敏信息

- 客户端名称和版本，例如 `claude --version` 或 `codex --version`；
- 操作系统；
- 状态码和完整错误文字；
- 请求路径，例如 `/v1/messages` 或 `/v1/responses`；
- 模型名；
- 已检查的配置文件路径。

不要提供完整 API Key、`settings.json` 原文、环境变量截图、订单号或支付信息。
