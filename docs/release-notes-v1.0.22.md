# CC Desktop Switch v1.0.22

<p align="center">
  <a href="#english">English</a> |
  <a href="#simplified-chinese">简体中文</a>
</p>

<a id="english"></a>

## English

This patch focuses on Claude Desktop / Cowork compatibility after the v1.0.21 release. It stays on the stable Python line and does not change the main provider workflow.

### Highlights

- **Cowork WebFetch compatibility**
  - CC Desktop Switch now writes `coworkEgressAllowedHosts` with `["*"]` when applying Claude Desktop policy.
  - Windows registry, macOS plist, macOS JSON, and macOS `configLibrary` writes all include the same setting.
  - Health checks now warn when this policy is missing and ask the user to re-apply the Desktop config.

- **Claude Haiku dated route compatibility**
  - Requests for `claude-haiku-4-5-20251001` are accepted as the internal dated alias of the mapped Haiku slot.
  - The model menu remains compact and still exposes only explicitly mapped Claude-safe routes.

- **macOS 1M metadata preservation**
  - macOS JSON and `configLibrary` writes now preserve `supports1m` model metadata instead of flattening `inferenceModels` to plain strings.
  - This helps DeepSeek 1M configurations pass the Desktop health check after re-applying and restarting Claude Desktop.

- **Safer DeepSeek Max handling**
  - DeepSeek Max request options are no longer injected just because a provider name contains `deepseek`.
  - Official DeepSeek remains supported; private gateways must explicitly declare DeepSeek Max compatibility before those options are merged.

### Notes

- #12 remains open until users can verify the in-app update flow from an older version to a newer one.
- #7, #18, and #19 remain support/diagnostics follow-ups. This release does not claim to support Microsoft 365 Office add-ins through the local Desktop gateway.

### Upgrade Notes

After updating, re-apply the active provider to Claude Desktop and fully restart Claude Desktop. macOS users should do this even if the previous apply looked successful, because v1.0.22 rewrites the `configLibrary` entry with preserved model metadata.

<a id="simplified-chinese"></a>

## 简体中文

这个小版本主要修复 v1.0.21 之后暴露出来的 Claude Desktop / Cowork 兼容问题。仍然基于当前 Python 稳定线，不改变主要 provider 使用流程。

### 主要变化

- **修复 Cowork WebFetch 访问网页问题**
  - 一键应用 Claude Desktop 配置时，现在会写入 `coworkEgressAllowedHosts=["*"]`。
  - Windows 注册表、macOS plist、macOS JSON、macOS `configLibrary` 都会同步写入。
  - 健康检查会识别缺少该配置的旧写入结果，并提示重新一键应用。

- **兼容 Claude Haiku dated route**
  - `claude-haiku-4-5-20251001` 会作为 Haiku 槽位的内部兼容别名处理。
  - 模型菜单仍保持简洁，只展示用户明确映射的 Claude-safe route。

- **修复 macOS 1M 元数据丢失**
  - macOS JSON 和 `configLibrary` 写入不再把 `inferenceModels` 压成纯字符串列表。
  - `supports1m` 会被保留下来，重新应用并重启 Claude Desktop 后，DeepSeek 1M 健康检查应能正常通过。

- **更谨慎地处理 DeepSeek Max**
  - 不再因为 provider 名称包含 `deepseek` 就自动注入 DeepSeek Max 参数。
  - 官方 DeepSeek 仍然支持；私有中转需要明确声明兼容后才会合并这些参数。

### 说明

- #12 继续保持打开，等用户从旧版本更新到新版本后再确认应用内更新安装器弹窗。
- #7、#18、#19 继续作为支持和诊断问题跟进。本版本不声称通过本机 Desktop gateway 支持 Microsoft 365 Office 插件。

### 升级说明

升级后，请对当前 provider 重新执行“一键应用到 Claude 桌面版”，然后完整重启 Claude Desktop。macOS 用户尤其需要重新应用，因为 v1.0.22 会重写 `configLibrary` entry 并保留模型能力元数据。
