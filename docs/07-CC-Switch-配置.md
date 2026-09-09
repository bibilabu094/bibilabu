# CC Switch 配置

CC Switch 用于管理多套 Claude Code API 配置，并可一键切换。本页使用平台 Docs 中的 Anthropic 类型供应商配置方式。

示例地址 `https://gate.bibilabu.cc` 是作者当前使用的平台演示地址；使用其他平台时，请替换为自己的 API 地址、API Key 和模型名称。

## 方式一：推荐使用桌面应用

1. 打开 [CC Switch Releases](https://github.com/farion1231/cc-switch/releases)。这是第三方项目，请确认发布页、安装包来源和系统类型后再下载。
2. Windows 下载 `.msi`，macOS 下载 `.dmg`，Linux 下载 `.deb` 或 `AppImage`，然后按安装程序提示安装。
3. 打开 CC Switch，点击左下角 `+` 添加供应商。
4. 供应商类型选择 **Anthropic**，填写：

```text
名称: 比比拉布 API
API Key: YOUR_API_KEY
Base URL: https://gate.bibilabu.cc
类型: Anthropic
```

5. 在供应商的“模型”设置中填写你当前 Key 有权使用的模型名称。可以配置：

```text
ANTHROPIC_MODEL=YOUR_MODEL_ID
ANTHROPIC_DEFAULT_SONNET_MODEL=YOUR_MODEL_ID
```

6. 保存后点击“启用”。随后启动 Claude Code，在 `/model` 中确认可用模型并发送测试消息。

## 方式二：命令行版

命令行版需要 Node.js 16 或更高版本。先按 [Node.js 与 npm 前置环境](00-Node.js与npm前置环境.md) 完成环境准备，再执行：

```bash
npm install -g @adithya-13/cc-switch
```

命令行版的具体交互以该项目当前说明为准。无论使用桌面版还是命令行版，API 地址都应使用 Anthropic 根地址，末尾不要加 `/v1`。

## 常见错误

| 现象 | 检查项 |
| --- | --- |
| 认证失败 | Key 是否为你自己的有效 Key，且未复制额外空格 |
| 404 或地址错误 | Base URL 末尾不能加 `/v1` |
| 切换后 Claude Code 仍用旧配置 | 在 CC Switch 中确认已点击“启用”，然后完全退出并重启 Claude Code |
| 模型不可用 | 使用当前 Key 在 `/v1/models` 或 Claude Code `/model` 中看到的模型名称 |
