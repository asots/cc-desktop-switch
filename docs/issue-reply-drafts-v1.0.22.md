# Issue reply drafts for v1.0.22

> Drafts only. Do not post without maintainer approval.

## #20 Cowork WebFetch / `coworkEgressAllowedHosts`

感谢你把根因写得这么清楚。我按 Anthropic 的 3P 配置文档核对了一遍，`coworkEgressAllowedHosts` 确实是 Cowork sandbox 的网络放行项；旧版本只写了推理 gateway 相关字段，导致 Cowork/WebFetch 在 organization-managed 模式下可能只能访问 inference endpoint。

这个问题已经在本地修复：一键应用时会同时写入 `coworkEgressAllowedHosts=["*"]`，Windows registry、macOS plist、macOS JSON 和 `configLibrary` 都会覆盖到；清除配置时也会一起清掉。发布后请重新一键应用并完整重启 Claude Desktop。

## #22 `claude-haiku-4-5-20251001`

感谢反馈。这个不是你配置错了，是新版 Claude Desktop / Code 内部可能会请求 dated Haiku ID：`claude-haiku-4-5-20251001`，而旧版本只默认处理菜单里的 `claude-haiku-4-5`。

本地已经修复：只要你映射了 Haiku 槽位，请求 `claude-haiku-4-5-20251001` 时会自动走同一个 Haiku 映射，不需要额外把菜单弄得更复杂。发布后请重新一键应用并重启 Claude Desktop。

## #10 macOS DeepSeek 1M status

感谢补充。这个问题更像是 macOS 写入路径里的元数据丢失：旧版本在 JSON / `configLibrary` 路径里可能把 `inferenceModels` 压成纯模型名列表，导致 `supports1m` 没有被读回，健康检查就会提示 1M 尚未写入。

本地已经修复：macOS JSON 和 `configLibrary` 写入会保留 `supports1m`。发布后麻烦升级、重新一键应用 DeepSeek 1M、完整重启 Claude Desktop；如果仍然提示，请导出诊断包，我继续跟。

## #9 OpenCode / DeepSeek Max

感谢反馈。这里不是所有叫 DeepSeek 的中转都支持官方 DeepSeek Max 参数。旧版本判断太粗，只要 provider 名称里有 `deepseek` 就可能按官方 DeepSeek 处理，导致 OpenCode/one-api/new-api 这类中转收到它不支持的 `thinking/output_config.effort=max`。

本地已经修复：只有官方 DeepSeek endpoint，或明确标记支持 DeepSeek Max 的私有 gateway，才会注入这些参数。普通中转不会再因为名字包含 deepseek 就误套 Max 参数。

## #12 Windows in-app update installer

这个问题在 v1.0.21 已经修复，但你说得对：因为当前没有比 v1.0.21 更高的线上版本，所以还没法从应用内更新链路验证。等下一个版本发布后，再从 v1.0.21 点应用内更新测试一次；如果安装器仍不弹，我继续排查。

## #18 DeepSeek 502 / tools warning

这个报错更像是本机 gateway 连接 DeepSeek 上游时被断开，或者当前请求带了 DeepSeek Anthropic 接口不支持的 tools。它不一定是软件配置写入失败。

建议先测试一个不带工具调用的普通对话；如果普通对话也失败，请导出诊断包，并附上 provider、Base URL、API format 和 gateway 日志。这样可以判断是网络、上游接口、工具调用，还是配置问题。

## #19 PowerPoint / Microsoft 365 add-in

这个和 CC Desktop Switch 当前的一键应用不是同一套东西。Claude for Microsoft 365 插件需要的是企业/组织侧的 gateway URL 和 API token，还涉及 CORS、Microsoft admin consent 等配置；它不能直接复用本机 `127.0.0.1` 的 Claude Desktop gateway。

所以目前本项目不直接支持 PowerPoint 插件。后续如果要做，需要单独设计一个公网/企业 gateway 模式，而不是简单写 Claude Desktop policy。

## #7 Relay non-JSON response

这个需要更多诊断信息。截图看起来像上游中转返回了非预期 JSON/SSE 内容，常见原因是 Base URL 路径不匹配、API format 选错、鉴权失败页、HTML 错误页，或者中转没有完全兼容当前协议。

新版已经增强了非 JSON 响应诊断。请补充脱敏诊断包、Base URL、API format、gateway 日志和错误截图，我才能判断是配置问题还是该中转暂不兼容。
