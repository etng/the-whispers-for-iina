# The Whispers for IINA

为 IINA 提供渐进式双语字幕：先尽快显示粗略原文，再逐步替换为更完整的字幕结果。

这个仓库只承载可公开分发的发行包、校验文件与安装说明。项目源码在私有开发仓库中维护。

## 安装

当前公开版本支持 Apple Silicon Mac（macOS 13 或更高版本）。推荐使用 Homebrew：

```shell
brew tap etng/tap
brew install the-whispers-for-iina
whispersctl setup
```

`whispersctl setup` 会显示组件计划并要求确认；它不会把大型模型打进安装包，也不会重复复制系统中已有、能够复用的 Hugging Face 或 whisper.cpp 模型。

如果尚未完成设置，IINA Overlay 会直接提示，不再假装任务已经进入后台队列；worker 无响应时也会给出重新连接入口。播放时可在字幕卡片右上角临时暂停字幕功能，或打开设置即时调整九档字号、三档宽度、三套皮肤和 35%–100% 背景透明度。

可用安装档位：

- `viewer`：只安装 IINA 插件，用于查看已有字幕。
- `local-asr`：只做本地英语/日语识别，不翻译。
- `standard`：默认档位，本地识别并翻译为中文；如果没有配置精校服务，会直接发布初译结果。
- `custom`：交互选择组件。

安装后可随时检查：

```shell
whispersctl doctor
whispersctl models scan
whispersctl worker status
```

已有字幕可以先脱敏整理为本地草稿；只有增加 `--submit` 并再次确认后，才会通过 GitHub Pull Request 公开字幕、ASS 和媒体匹配信息，不会上传音视频、本机路径、模型或日志：

```shell
whispersctl contribution current "/path/to/video.mkv"
```

卸载程序但保留字幕库和共享模型：

```shell
whispersctl uninstall
brew uninstall the-whispers-for-iina
```

## 手动下载

每个 [GitHub Release](https://github.com/etng/the-whispers-for-iina/releases) 都包含：

- Apple Silicon 运行包（`.tar.gz`）
- IINA 插件（`.iinaplgz`）
- Python wheel（供集成与审计使用）
- `SHA256SUMS`、发行清单和验证报告

## 当前边界

- 公开包尚未使用 Apple Developer ID 签名和 notarization；Homebrew 是当前推荐安装路径。
- Intel Mac 构建与验证尚未完成。
- 当前使用 `whispersctl` 完成组件选择和维护；图形化安装管理器留作后续版本。

这些边界会明确记录在每个发行版本中，不影响现有 Apple Silicon + Homebrew 安装链路。

## 许可证

发行仓库中的安装说明与发布元数据使用 MIT License。随包分发的第三方组件继续遵循各自许可证。
