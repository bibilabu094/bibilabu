# Codex 小白配置

本页按照平台 Docs 的 OpenAI Responses 兼容方式配置 Codex。Codex 走 `/v1/responses`，所以 `base_url` 必须以 `/v1` 结尾；密钥只放在 `auth.json`，不要写进 `config.toml`。

示例使用 `https://gate.bibilabu.cc/v1`。其中域名是作者当前使用的平台演示地址；使用其他平台时，请替换成你自己的 API 地址和 Key。

## 先完成 Node.js 前置环境

若你还没有安装 Node.js 和 npm，先看 [Node.js 与 npm 前置环境](00-Node.js与npm前置环境.md)。完成后再按当前页面执行。

## Windows 10/11

### 1. 安装并验证 Codex

打开“终端（管理员）”或“Windows PowerShell（管理员）”，执行：

```powershell
npm install -g @openai/codex
codex --version
```

看到版本号说明安装成功。

### 2. 创建配置目录和 `config.toml`

在文件资源管理器地址栏输入下面路径并回车：

```text
%USERPROFILE%\.codex
```

若文件夹不存在，就在 `%USERPROFILE%` 下新建 `.codex` 文件夹。进入后新建文件 `config.toml`，并粘贴下面内容。若用其他平台，只替换 `base_url` 中的域名，**保留末尾 `/v1`**。

```toml
model_provider = "bibilabu"
# 不要固定 model；启动后用 /model 从实时列表选择。
model_reasoning_effort = "medium"
preferred_auth_method = "apikey"

[model_providers.bibilabu]
name = "bibilabu"
base_url = "https://gate.bibilabu.cc/v1"
wire_api = "responses"
requires_openai_auth = true
discover_models = true
```

按 `Ctrl + S` 保存。`preferred_auth_method = "apikey"` 必须位于文件顶部，不能放进 `[model_providers.bibilabu]` 区块。

### 3. 创建密钥文件 `auth.json`

仍在 `%USERPROFILE%\.codex` 文件夹中，新建文件：

```text
auth.json
```

打开后只粘贴下面 JSON，把 `YOUR_API_KEY` 替换为自己的 Key，再按 `Ctrl + S` 保存：

```json
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
```

注意文件名必须是 `auth.json`，不是 `auth.json.txt`。在文件资源管理器的“查看”中打开“文件扩展名”，可以确认文件名。

### 4. 启动并选模型

新开普通 PowerShell，进入代码项目目录，例如：

```powershell
cd "C:\Users\你的用户名\Documents\我的项目"
codex
```

进入 Codex 后输入：

```text
/model
```

选择当前 Key 可用的 GPT 系列模型。也可以执行 `codex debug models` 查看 Codex 识别到的模型。

## macOS 和 Linux

打开终端，依次执行：

```bash
npm install -g @openai/codex
codex --version
mkdir -p ~/.codex
nano ~/.codex/config.toml
```

在 `~/.codex/config.toml` 中粘贴并保存：

```toml
model_provider = "bibilabu"
# 不要固定 model；启动后用 /model 从实时列表选择。
model_reasoning_effort = "medium"
preferred_auth_method = "apikey"

[model_providers.bibilabu]
name = "bibilabu"
base_url = "https://gate.bibilabu.cc/v1"
wire_api = "responses"
requires_openai_auth = true
discover_models = true
```

在 nano 中按 `Ctrl + O` 保存、按回车确认、按 `Ctrl + X` 退出。接着执行：

```bash
nano ~/.codex/auth.json
```

粘贴并保存：

```json
{
  "OPENAI_API_KEY": "YOUR_API_KEY"
}
```

最后进入项目目录并运行 `codex`，在会话内输入 `/model` 选择 GPT 系列模型。

## 这五项不要改错

| 配置项 | 必须填写的值 | 写错会怎样 |
| --- | --- | --- |
| `base_url` | 地址末尾必须是 `/v1` | 无法正确访问 Responses 路径 |
| `wire_api` | `responses` | 新版 Codex 不接受 `openai` 或 `chat` |
| `preferred_auth_method` | `apikey`，且位于文件顶部 | Codex 不会按 `auth.json` 的 API Key 方式认证 |
| `requires_openai_auth` | `true` | 无法按 OpenAI API Key 方式接入 |
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
