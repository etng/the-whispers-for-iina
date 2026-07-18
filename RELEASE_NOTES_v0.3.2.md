# The Whispers for IINA 0.3.2

这次更新增加 Intel Mac 只看版，并修正升级后仍可能继续运行旧 runtime 的问题。

## Intel 只看版

- 新增独立 x86_64 自包含包，Homebrew 会按 CPU 自动选择。
- Intel 包不安装 ffmpeg、Whisper、翻译服务或模型，模型增量固定为 0 B。
- 打开媒体后只读取有界的头尾数据计算指纹，再从公开字幕仓库下载、校验并加载其他机器贡献的 ASS 与 Overlay 数据。
- 在线仓库尚无匹配时会明确说明“本机只看模式不会下载模型”，不会误入音轨探测或本地生成队列。
- Intel 包固定使用 `viewer` 档位；显式请求本地生成档位时会停止并给出说明。

## 升级可靠性

- `whispersctl setup` 现在始终激活当前安装包携带的 runtime，即使旧版 `current` 已经存在。
- 新版 runtime 写入不可变版本目录后再原子切换，旧版仍可用于回滚。
- 只看档位也会安装轻量在线匹配 worker，因此 Overlay 能判断后台健康状态并取得远端字幕。

## 推荐安装或升级

```shell
brew update
brew upgrade etng/tap/the-whispers-for-iina
whispersctl setup
```

Intel Mac 首次安装会自动使用只看档位；也可以显式执行：

```shell
brew install etng/tap/the-whispers-for-iina
whispersctl setup --profile viewer
```

## 已执行验证

- 91 项 Python 单元与集成测试通过。
- 31 项 IINA 插件测试通过。
- arm64 完整包在隔离目录完成五阶段真实 ASR、Interactive JSON/ASS 生成与安全卸载。
- x86_64 解释器经 `file` 确认为 Intel Mach-O，并在 Rosetta 下完成 CLI、零模型设置和安全卸载。
- x86_64 只看 worker 使用只读纪录片的已知指纹，从公开仓库命中 `final` 并生成非空 Interactive JSON 与 ASS；没有探测音轨或启动本地模型。
- 两个发行包、插件、wheel、Formula 与 manifest 的 SHA-256 全部通过。

## 当前边界与 TODO

- 当前运行包尚未使用 Developer ID Application 签名，也未完成 Apple notarization。
- Intel 只看包已完成 x86_64/Rosetta 验证，仍需在物理 Intel Mac 上复核。
- 当前组件选择使用 `whispersctl`；图形化 Manager.app 待补。

大型模型不包含在发行包内。Apple Silicon 安装向导会先复用系统已有工具、Hugging Face cache 与用户指定的模型目录；Intel 只看版不会进入模型下载流程。
