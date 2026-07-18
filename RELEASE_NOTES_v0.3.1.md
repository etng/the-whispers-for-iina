# The Whispers for IINA 0.3.1

这次更新重点解决“看似进入队列、实际后台尚未初始化”的空等问题，并把常用控制直接放回 Overlay。

## 新增与改进

- Overlay 在用户 runtime 尚未完成设置时立即提示 `whispersctl setup`，不会写入无人处理的假队列请求。
- 请求提交后 8 秒没有新任务状态或 worker 健康心跳，会显示“后台没有响应”并提供重新连接入口。
- 字幕卡片右上角新增直接暂停/恢复；暂停只影响当前播放窗口，不会破坏已经在后台安全执行的任务。
- 设置面板新增 35%–100% 背景透明度，可在真实字幕卡片上即时预览并持久化。
- 本地字幕可脱敏生成贡献草稿，并在用户明确确认后自动创建 `etng/whisper-subs` Pull Request。
- 支持按媒体根目录批量整理字幕；相同内容指纹会合并到公开仓库的原条目，不制造重复数据。

## 贡献数据边界

自动贡献只包含 `manifest.json`、各阶段 Interactive JSON、可用 ASS、artifact 摘要与根 `index.json`。音视频、绝对路径、模型、日志和 Codex session 不会进入草稿或 Pull Request。没有 `--submit` 时只在本机保留可检查草稿，不产生远端写操作。

## 推荐安装或升级

```shell
brew update
brew upgrade etng/tap/the-whispers-for-iina
whispersctl setup
```

首次安装：

```shell
brew install etng/tap/the-whispers-for-iina
whispersctl setup
```

## 已执行验证

- 87 项 Python 单元与集成测试通过。
- 30 项 IINA 插件测试通过。
- 初始化缺失与 worker 8 秒无响应均有插件级防回归测试。
- Overlay 初始化提示、透明度即时预览和暂停/恢复完成真实浏览器可视检查。
- 真实只读视频成功生成脱敏贡献草稿，媒体目录无写入，未执行公开提交。
- 发行包完成隔离安装、五阶段真实 ASR、Interactive JSON/ASS 生成、checksum 与安全卸载验证。

## 当前边界与 TODO

- 当前运行包尚未使用 Developer ID Application 签名，也未完成 Apple notarization。
- 当前仅发布并验证 Apple Silicon；Intel Mac 包待补。
- 当前组件选择使用 `whispersctl`；图形化 Manager.app 待补。

大型模型不包含在发行包内。安装向导会先复用系统已有工具、Hugging Face cache 与用户指定的模型目录，只在确实缺少时显示下载计划。
