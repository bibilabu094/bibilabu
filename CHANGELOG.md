# 更新记录

## 2026-09-09

- Codex 自定义 Provider 改为当前 `env_key` 环境变量配置，移除 `auth.json`、`preferred_auth_method` 和 `discover_models`，并将 `requires_openai_auth` 修正为 `false`。
- Claude Code 改为单一认证变量、`/v1/messages` 最小请求验证，并补充网关模型发现说明。
- 新增 API 状态码排错页，同步修正接口说明与 FAQ。

## 2026-09-08

- 创建公开接入资料仓库。
- 补充 Claude Code 与 Codex 的逐步配置说明。
- 明确 Claude Code 的 Base URL 不带 `/v1`，Codex 的 Base URL 必须带 `/v1`。
- 明确 Codex 使用 `wire_api = "responses"`、`preferred_auth_method = "apikey"` 和 `auth.json` 读取密钥。
- 明确所有 `https://gate.bibilabu.cc` 都只是作者当前使用的平台演示地址，读者必须替换为自己的平台地址和 Key。

## 2026-09-09

- 对照当前平台 Docs 补齐 Node.js LTS、npm 镜像、Claude Code 和 Codex 的安装步骤。
- 将 Codex 的 `preferred_auth_method` 调整为与平台 Docs 一致的顶层配置，并补齐 `requires_openai_auth = true`。
- 新增 OpenAI / Anthropic 兼容接入、Python、curl、常见客户端和 CC Switch 配置教程。
