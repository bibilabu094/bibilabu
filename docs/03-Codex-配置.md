# Codex 小白配置

本页用于把 Codex 接到 **OpenAI Responses 兼容** 的第三方 Provider。示例地址 `https://gate.bibilabu.cc/v1` 是作者当前使用的平台；使用其他平台时，必须替换成自己的 API 地址、Key 和模型名。

Codex 使用 `/v1/responses`，因此 `base_url` 必须保留末尾 `/v1`。本页采用当前 Codex 的 `env_key` 方式读取 Key：Key 放在环境变量中，不创建 `auth.json`，也不把 Key 写进 `config.toml`。

## Windows 10/11

### 1. 安装并验证 Codex

打开“终端（管理员）”或“Windows PowerShell（管理员）”，执行：

```powershell
npm install -g @openai/codex
codex --version
```

看到版本号说明安装成功。

### 2. 创建 `config.toml`

在普通 PowerShell 中依次执行：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex"
New-Item -ItemType File -Force "$env:USERPROFILE\.codex\config.toml"
notepad "$env:USERPROFILE\.codex\config.toml"
```

粘贴下面配置。若使用其他平台，只替换 `base_url` 的域名，保留 `/v1`；`BIBILABU_API_KEY` 是环境变量名称，必须与下一步完全一致。

```toml
model_provider = "bibilabu"
model_reasoning_effort = "medium"

[model_providers.bibilabu]
name = "bibilabu"
base_url = "https://gate.bibilabu.cc/v1"
env_key = "BIBILABU_API_KEY"
wire_api = "responses"
requires_openai_auth = false
```

按 `Ctrl + S` 保存。不要添加 `preferred_auth_method` 或 `discover_models`；第三方 Provider 的 `requires_openai_auth` 必须为 `false`。

### 3. 设置 Key 环境变量

仍在 PowerShell 中执行，并把 `YOUR_API_KEY` 替换成自己的 Key：

```powershell
[Environment]::SetEnvironmentVariable("BIBILABU_API_KEY", "YOUR_API_KEY", "User")
```

关闭当前 PowerShell，再打开一个新的普通 PowerShell。新终端才会读取刚保存的用户环境变量。

### 4. 启动并验证

进入代码项目目录并启动：

```powershell
cd "C:\Users\你的用户名\Documents\我的项目"
codex
```

发送一条普通消息。需要切换模型时再输入 `/model`，并只选择平台明确支持 Responses 的 GPT 系列模型。

## macOS 和 Linux

打开终端，执行：

```bash
npm install -g @openai/codex
codex --version
mkdir -p ~/.codex
nano ~/.codex/config.toml
```

粘贴与 Windows 相同的 TOML，按 `Ctrl + O` 保存、回车确认、`Ctrl + X` 退出。随后在**启动 Codex 的同一个终端**设置 Key：

```bash
export BIBILABU_API_KEY="YOUR_API_KEY"
cd ~/你的项目目录
codex
```

上述 `export` 仅对当前终端有效；关闭后需要再次设置，或按自己使用的 shell 安全地持久化环境变量。

## 这四项不要改错

| 配置项 | 必须填写的值 | 说明 |
| --- | --- | --- |
| `base_url` | 地址末尾必须是 `/v1` | Codex 从此地址请求 Responses 接口 |
| `env_key` | `BIBILABU_API_KEY` | 告诉 Codex 从哪个环境变量读取 Key |
| `wire_api` | `responses` | 当前唯一支持的协议值，省略时也默认是它 |
| `requires_openai_auth` | `false` | 使用第三方 Provider 的环境变量 Key，而非 OpenAI 身份认证 |
| `BIBILABU_API_KEY` | 你的实际 Key | 必须在启动 Codex 的进程环境中存在 |

## 成功标准

- `codex --version` 能显示版本。
- `config.toml` 没有 `auth.json`、`preferred_auth_method` 或 `discover_models`，且 `requires_openai_auth = false`。
- 启动 Codex 的终端已经读取到 `BIBILABU_API_KEY`。
- 选择受支持模型后，普通请求获得正常回复。

## 常见错误

| 现象 | 先检查什么 |
| --- | --- |
| `401` 或认证失败 | 新开的终端是否读取到 `BIBILABU_API_KEY`；环境变量名是否与 `env_key` 完全一致 |
| `404` 或路径错误 | `base_url` 是否遗漏或重复了 `/v1` |
| `unknown variant openai` | `wire_api` 被写成了旧值；改为 `responses` 或删除该行使用默认值 |
| 找不到模型 | 先确认平台列出的模型是否支持 `/v1/responses`，不要把 Claude 模型用于 Codex Responses |

其他 HTTP 状态码见 [API 报错排查](troubleshooting-api-errors.md)。
