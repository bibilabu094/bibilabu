# Codex 小白配置

这篇教程配置的是 Codex。Codex 走 Responses 路径，Base URL 必须以 `/v1` 结尾；密钥保存在 `auth.json`，不要直接写进 `config.toml`。

示例使用 `https://gate.bibilabu.cc/v1`。其中域名是作者当前使用的平台演示地址；你使用其他平台时，请替换成自己的 API 地址和 Key。

## 配置前检查

在终端执行：

```text
codex --version
```

能看到版本号即可继续。若命令不存在，请先完成 Codex 安装，再回到本页配置。

## Windows 10/11

### 1. 创建 Codex 配置目录

按 `Win + R`，输入 `powershell` 并回车。执行：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex" | Out-Null
```

配置目录是：

```text
%USERPROFILE%\.codex
```

### 2. 编辑密钥文件 `auth.json`

执行：

```powershell
notepad "$env:USERPROFILE\.codex\auth.json"
```

在打开的文件中粘贴下面内容，把 `YOUR_API_KEY` 换成自己的 Key，然后按 `Ctrl + S` 保存：

```json
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
```

### 3. 编辑主配置文件 `config.toml`

回到 PowerShell，执行：

```powershell
notepad "$env:USERPROFILE\.codex\config.toml"
```

在打开的文件中粘贴下面内容。只有在你使用别的平台时，才把 `base_url` 的域名替换成自己的；**保留末尾的 `/v1`**。

```toml
model_provider = "bibilabu"

[model_providers.bibilabu]
name = "bibilabu"
base_url = "https://gate.bibilabu.cc/v1"
wire_api = "responses"
preferred_auth_method = "apikey"
discover_models = true

# 不要在这里写 API Key。
# 不要固定 model；启动后用 /model 从实时列表选择。
```

按 `Ctrl + S` 保存，然后关闭记事本。

### 4. 启动并选模型

在 PowerShell 执行：

```powershell
codex
```

进入 Codex 后输入：

```text
/model
```

选择你平台当前提供的 GPT 系列模型，随后发送一条普通请求测试。

## macOS 和 Linux

在终端执行：

```bash
mkdir -p ~/.codex
nano ~/.codex/auth.json
```

在 `~/.codex/auth.json` 中粘贴并保存：

```json
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
```

继续执行：

```bash
nano ~/.codex/config.toml
```

在 `~/.codex/config.toml` 中粘贴并保存：

```toml
model_provider = "bibilabu"

[model_providers.bibilabu]
name = "bibilabu"
base_url = "https://gate.bibilabu.cc/v1"
wire_api = "responses"
preferred_auth_method = "apikey"
discover_models = true
```

最后执行 `codex`，进入后输入 `/model` 选择 GPT 系列模型。

## 这四项不要改错

| 配置项 | 必须填写的值 | 写错会怎样 |
| --- | --- | --- |
| `base_url` | 地址末尾必须是 `/v1` | 无法正确访问 Responses 路径 |
| `wire_api` | `responses` | 新版 Codex 不接受 `openai` 或 `chat` |
| `preferred_auth_method` | `apikey` | Codex 不会从 `auth.json` 读取 Key |
| `discover_models` | `true` | 无法自动读取平台的可用模型列表 |

## 成功标准

- `codex --version` 能显示版本。
- `/model` 能显示模型列表。
- 选择 GPT 系列模型后，普通请求能获得正常回复。

## 限制和错误处理

- 当前已验证 Codex 的 `/v1/responses` 路径用于 GPT 系列模型。不要在 Codex 中选择 Claude 模型。
- 若提示 `unknown variant openai`，说明 `wire_api` 写成了旧值；改回 `responses`。
- 若 401 或认证失败，检查 `%USERPROFILE%\.codex\auth.json` 是否为有效 JSON，且 `OPENAI_API_KEY` 已替换成你自己的 Key。
- 若地址报错，检查 `base_url` 是否包含 `/v1`，以及域名是否为你自己的平台地址。

其他问题见 [常见问题](05-常见问题.md)。
