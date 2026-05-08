# CC Desktop Switch v1.0.21

<p align="center">
  <a href="#english">English</a> |
  <a href="#simplified-chinese">简体中文</a>
</p>

<a id="english"></a>

## English

This is a small patch release for the current stable Python line. It publishes the fixes and improvements already merged into `main` and does not change the main provider workflow.

### Highlights

- **macOS fresh installs write Desktop configuration correctly**
  - When Claude Desktop has no `configLibrary` entry yet, CC Desktop Switch now creates one before writing the active gateway configuration.
  - This avoids a misleading successful apply on a fresh macOS environment where no Desktop config was actually written.

- **Intel Mac release assets are now included**
  - The release workflow now builds macOS x64 `.pkg` and `.dmg` assets in addition to Windows x64 and macOS arm64.
  - `latest.json` now includes `windows-x64`, `macos-arm64`, and `macos-x64` platforms.

- **Safe custom Claude model routes**
  - Provider settings can now include custom Claude route mappings.
  - Only `claude-*` route names are exposed to Claude Desktop, while the real upstream model IDs stay inside the local gateway mapping.
  - This keeps compatibility with newer Claude Desktop versions that reject raw third-party model IDs in `inferenceModels`.

- **Windows in-app update installer is visible**
  - Fixed a Windows update flow where the installer could be launched with hidden window flags after download.
  - The downloaded Windows installer now opens visibly so users can complete the install.

### Not Included

- #7, #9, and #10 remain open for follow-up.
- This release does not claim to fix every OpenAI/new-api relay variant, OpenCode Max compatibility, or the reported macOS DeepSeek 1M status issue.

### Upgrade Notes

No user configuration migration is required. After updating, re-apply the active provider to Claude Desktop if your Desktop configuration was written by an older version, then fully restart Claude Desktop.

<a id="simplified-chinese"></a>

## 简体中文

这是当前 Python 稳定线的小版本更新，只发布已经合并进 `main` 的修复和改进，不改变主要 provider 使用流程。

### 主要变化

- **macOS 全新环境可以正确写入桌面版配置**
  - 当 Claude Desktop 还没有 `configLibrary` entry 时，CC Desktop Switch 会先创建 entry，再写入当前本机 gateway 配置。
  - 这可以避免全新 macOS 环境里显示应用成功，但实际上没有写入桌面版配置的问题。

- **补齐 Intel Mac 发布资产**
  - Release workflow 现在会额外构建 macOS x64 `.pkg` 和 `.dmg`。
  - `latest.json` 会同时包含 `windows-x64`、`macos-arm64` 和 `macos-x64`。

- **支持安全的自定义 Claude 模型 route**
  - Provider 设置里可以添加自定义 Claude route 映射。
  - 只会把 `claude-*` route 暴露给 Claude Desktop，真实上游模型 ID 仍只保留在本机 gateway 内部。
  - 这样可以兼容新版 Claude Desktop 对 `inferenceModels` 的模型名校验。

- **Windows 应用内更新安装器会正常弹出**
  - 修复 Windows 下载更新后，安装器被隐藏窗口参数启动的问题。
  - 下载完成后，Windows 安装器会以可见窗口打开，用户可以继续完成安装。

### 未包含内容

- #7、#9、#10 继续保持打开，后续单独跟进。
- 本版本不声称已经修复所有 OpenAI/new-api 中转差异、OpenCode Max 兼容性或 macOS DeepSeek 1M 状态提示问题。

### 升级说明

不需要迁移用户配置。升级后，如果 Claude Desktop 配置来自旧版本，建议重新对当前 provider 执行“一键应用到 Claude 桌面版”，然后完整重启 Claude Desktop。
