# OpenAI 与 Anthropic 兼容接入

平台 Docs 同时提供 OpenAI 和 Anthropic 兼容接口。本页给 ChatBox、Cherry Studio、Cline、Roo Code、LangChain 和代码调用使用。

示例中的 `https://gate.bibilabu.cc` 是作者当前使用的平台演示地址；你使用其他平台时，请整体替换为自己的 API 地址。`YOUR_API_KEY` 和 `YOUR_MODEL_ID` 都是占位文字，必须替换成自己的实际值。

## 先选协议和 Base URL

| 用法 | 协议 | Base URL | 认证 |
| --- | --- | --- | --- |
| ChatBox、Cherry Studio、LangChain | OpenAI | `https://gate.bibilabu.cc/v1` | API Key |
| Cline、Roo Code | OpenAI 或 Anthropic | OpenAI 用带 `/v1` 的地址；Anthropic 用不带 `/v1` 的根地址 | 依客户端表单填写 |
| Claude Code | Anthropic | `https://gate.bibilabu.cc` | Bearer 网关用 `ANTHROPIC_AUTH_TOKEN`；`x-api-key` 网关用 `ANTHROPIC_API_KEY` |
| Codex | OpenAI Responses | `https://gate.bibilabu.cc/v1` | `BIBILABU_API_KEY` 环境变量 |

模型名称不要从示例中猜测。优先使用平台控制台给出的模型 ID；若平台实现 `/v1/models`，可将其作为辅助检查。

## OpenAI 格式

在支持 OpenAI 的客户端中填入：

```text
Base URL: https://gate.bibilabu.cc/v1
API Key: YOUR_API_KEY
```

### Python 示例

先安装 SDK：

```bash
pip install openai
```

新建文件 `example.py`，粘贴下面内容后替换两个占位符：

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://gate.bibilabu.cc/v1",
)

response = client.chat.completions.create(
    model="YOUR_MODEL_ID",
    messages=[{"role": "user", "content": "你好"}],
)

print(response.choices[0].message.content)
```

在 `example.py` 所在目录执行：

```bash
python example.py
```

### curl 示例

macOS 或 Linux 终端执行：

```bash
curl https://gate.bibilabu.cc/v1/chat/completions \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"YOUR_MODEL_ID","messages":[{"role":"user","content":"你好"}]}'
```

Windows PowerShell 执行：

```powershell
curl.exe https://gate.bibilabu.cc/v1/chat/completions `
  -H "Authorization: Bearer YOUR_API_KEY" `
  -H "Content-Type: application/json" `
  -d '{"model":"YOUR_MODEL_ID","messages":[{"role":"user","content":"你好"}]}'
```

成功时会返回 JSON，其中包含模型的回复内容。不要在截图或聊天记录中暴露 `Authorization` 里的 Key。

## Anthropic 格式

在支持 Anthropic 的客户端中填入：

```text
Base URL: https://gate.bibilabu.cc
API Key: YOUR_API_KEY
anthropic-version: 2023-06-01
```

Anthropic 类型客户端使用服务根地址，因此末尾不要写 `/v1`。Claude Code 的完整文件配置请看 [Claude Code 小白配置](02-Claude-Code-配置.md)。

## 成功后怎样确认

- API 返回正常 JSON，而不是 `401`、`404` 或 `429`。
- Claude Code 的 `/v1/messages` 最小请求或客户端实际消息能够成功返回。
- 改用你的实际 API 地址和 Key 后仍然能请求成功。
