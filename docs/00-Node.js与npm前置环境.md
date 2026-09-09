# Node.js 与 npm 前置环境

Claude Code 和 Codex 都通过 npm 安装，所以先安装一次 Node.js LTS 即可，两个 CLI 共用。若你只使用 ChatBox、Cherry Studio、Cline、Roo Code 或 CC Switch 桌面版，不需要做本页步骤。

## Windows 10/11

### 1. 安装 Node.js LTS

在浏览器打开 [Node.js 官网](https://nodejs.org/)，下载 **LTS 64-bit** Windows 安装包。双击安装包，保持默认选项一路点击下一步即可。

### 2. 重新打开管理员 PowerShell 并验证

关闭所有已打开的 PowerShell。按 `Win + X`，选择“终端（管理员）”或“Windows PowerShell（管理员）”。执行：

```powershell
node -v
npm -v
```

两行都显示版本号，例如 `vXX.X.X` 和 `XX.X.X`，说明安装成功。

### 3. 仅在 PowerShell 阻止 npm 脚本时执行

如果安装 CLI 时出现 `npm.ps1` 被禁止执行，仍在管理员 PowerShell 中运行下面命令，看到确认提示后输入 `Y` 并回车：

```powershell
Set-ExecutionPolicy RemoteSigned
```

### 4. npm 下载很慢时切换镜像

```powershell
npm config set registry https://registry.npmmirror.com
```

这项设置只需做一次。完成后继续 Claude Code 或 Codex 教程。

## macOS

若已安装 Homebrew，在终端执行：

```bash
brew install node
node -v
npm -v
```

如果没有 Homebrew，请先按 [Homebrew 官方安装说明](https://brew.sh/) 安装，再执行上面的命令。若 npm 下载很慢，可以执行：

```bash
npm config set registry https://registry.npmmirror.com
```

## Linux

请通过你的发行版软件源或 Node.js 官方 LTS 安装页安装 Node.js，安装后验证：

```bash
node -v
npm -v
```

两条命令均显示版本号后，继续 Claude Code 或 Codex 教程。

## 成功标准

- `node -v` 和 `npm -v` 都显示版本号。
- 关闭再重新打开终端后，两个命令仍可执行。
- 可以继续运行 `npm install -g ...` 安装 CLI。
