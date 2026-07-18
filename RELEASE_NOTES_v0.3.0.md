# The Whispers for IINA 0.3.0

首个可公开安装的 Apple Silicon 版本。

## 可以使用的能力

- 安装 IINA overlay 插件与后台 worker。
- 自动发现本机已有的 Hugging Face、whisper.cpp 和外部共享模型，避免重复占用磁盘。
- 提供 `viewer`、`local-asr`、`standard`、`custom` 四种组件档位。
- 通过版本化 runtime 和原子切换完成安装或升级，并可回滚上一版本。
- 默认卸载只移除程序，保留中央字幕库、缓存和外部模型。
- 对本地英语或日语音轨进行识别；`standard` 档位继续翻译为中文。

## 推荐安装

```shell
brew tap etng/tap
brew install the-whispers-for-iina
whispersctl setup
```

如果只想查看别人已经生成的字幕：

```shell
whispersctl setup --profile viewer
```

## 已执行验证

- 76 项 Python 单元测试通过。
- 6 项集成测试通过。
- 28 项 IINA 插件测试通过。
- 从发行包完成隔离安装、真实两分钟媒体识别、ASS 生成和安全卸载。
- 发行资源已从公开 Release 重新下载并通过 SHA-256 校验。
- Homebrew formula 通过格式与审计检查，并已从公开地址完成安装和自测。

详细机器可读结果见本 Release 中的 `validation-report.json`。

## 已知边界与 TODO

- 当前运行包尚未使用 Developer ID Application 签名，也未完成 Apple notarization。
- 当前仅发布并验证 Apple Silicon；Intel Mac 包待补。
- 当前组件选择使用 `whispersctl`；图形化 Manager.app 待补。

大型模型不包含在发行包内。安装向导会先复用系统已有模型，并在确实缺少时再按所选档位提示下载。
