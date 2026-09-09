# bibilabu API 接入指南

这里是 bibilabu API 中转站的公开配置资料。内容按平台 Docs 的实际接入方式整理，面向第一次配置 AI 工具的用户，说明文件放在哪里、要编辑什么、配置完成后怎样判断成功。

## 先读这一段

本文所有示例都使用 `https://gate.bibilabu.cc` 作为演示 API 地址。这是作者当前使用的平台地址，用于展示格式。

你使用自己的中转站或其他 API 平台时，必须把示例中的 API 地址、API Key、可用模型和认证要求替换成你的平台实际提供的内容。不要复制别人的 Key，也不要把自己的 Key 发到群聊、工单、截图或 GitHub。

## 选择你的工具

| 你要配置的工具 | 直接看这里 | Base URL 写法 | 已验证的请求路径 | 模型选择 |
| --- | --- | --- | --- | --- |
| Claude Code | [Claude Code 小白配置](docs/02-Claude-Code-配置.md) | `https://gate.bibilabu.cc`，末尾不要加 `/v1` | `/v1/messages` | 启动后在 Claude Code 中输入 `/model` |
| Codex | [Codex 小白配置](docs/03-Codex-配置.md) | `https://gate.bibilabu.cc/v1`，必须带 `/v1` | `/v1/responses` | 启动后在 Codex 中输入 `/model` |
| ChatBox、Cherry Studio、Cline、Roo Code、LangChain 或代码调用 | [OpenAI / Anthropic 兼容接入](docs/06-OpenAI与Anthropic兼容接入.md) | 按客户端协议选择根地址或带 `/v1` 的地址 | `/v1/chat/completions` 或 `/v1/messages` | 以 `/v1/models` 实时列表为准 |
| CC Switch | [CC Switch 配置](docs/07-CC-Switch-配置.md) | `https://gate.bibilabu.cc`，末尾不要加 `/v1` | Anthropic 兼容配置 | 在 CC Switch 中填入可用模型 |

Claude Code 和 Codex 的配置不能混用：目录不同、密钥保存方式不同、Base URL 写法也不同。

## 阅读顺序

1. [开始前必读](docs/01-开始前必读.md)：准备 API 地址和 Key，了解哪些内容不能公开。
2. 若要使用 Claude Code 或 Codex，先完成 [Node.js 与 npm 前置环境](docs/00-Node.js与npm前置环境.md)。只需安装一次，两个 CLI 共用。
3. 按你使用的客户端完成对应教程。Claude Code、Codex 和 CC Switch 的配置不能混用。
4. [接口与模型说明](docs/04-接口与模型.md)：理解 `/v1/models`、`/v1/messages`、`/v1/responses` 和 `/v1/chat/completions` 的用途。
5. 出错时看 [常见问题](docs/05-常见问题.md)。

## 已验证范围

- Claude Code 通过 Anthropic Messages 兼容路径 `/v1/messages` 接入，GPT 系列与 Claude 系列请求已验证可用。
- Codex 使用 `wire_api = "responses"` 通过 `/v1/responses` 接入，GPT 系列请求已验证可用。
- Codex 的 Responses 路径不用于 Claude 模型；需要 Claude 模型时，请使用 Claude Code 的配置方式。
- 可用模型以平台实时返回的 `/v1/models` 为准，不要把教程中的模型名当作固定清单。

## 安全规则

- API Key 只应保存在你的本机配置文件或平台认可的密钥管理工具中。
- `~/.codex/auth.json`、`%USERPROFILE%\\.codex\\auth.json`、`.env` 这类文件不得提交到 GitHub；本仓库的 `.gitignore` 已忽略常见密钥文件。
- 截图前先检查 Key 是否露出。若已泄露，请立即在 API 平台删除或轮换该 Key。

## 更新

配置规则和已验证范围见 [更新记录](CHANGELOG.md)。
