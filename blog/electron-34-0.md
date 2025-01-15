Electron 34.0.0 已发布！它包括对 Chromium 132.0.6834.83、V8 13.2 和 Node 20.18.1 的升级。

---

Electron 团队很高兴发布了 Electron 34.0.0！你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Bluesky](https://bsky.app/profile/electronjs.org) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 重点内容

- 添加了 `WebFrameMain.collectJavaScriptCallStack()` 用于访问无响应渲染器的 JavaScript 调用堆栈。[#44938](https://github.com/electron/electron/pull/44938)
- 添加了 API 以管理共享字典，以提高使用 Brotli 或 ZStandard 的压缩效率。新的 API 是 `session.getSharedDictionaryUsageInfo()`、 `session.getSharedDictionaryInfo(options)`、 `session.clear. SharedDictionaryCache()`、 和 `session.clear. SharedDictionaryCacheForIsolation(options)`. [#44950](https://github.com/electron/electron/pull/44950)

### 架构（Stack）更新

- Chromium `132.0.6834.83`
  - [131 新功能](https://developer.chrome.com/blog/new-in-chrome-131/)
  - [132 新功能](https://developer.chrome.com/blog/new-in-chrome-132/)
- Node `20.18.1`
  - [Node 20.18.1 博客](https://nodejs.org/en/blog/release/v20.18.1/)
- V8 `13.2`

Electron 34 将 Chromium 从 `130.0.6723.44` 升级到 `132.0.6834.83`，Node 从 `20.18.0` 升级到 `20.18.1` 以及 V8 从 `13.0` 升级到 `13.2`。

### 新特性

- 添加了 API 以管理共享字典，以提高使用 Brotli 或 ZStandard 的压缩效率。新的 API 是 `session.getSharedDictionaryUsageInfo()`、 `session.getSharedDictionaryInfo(options)`、 `session.clear. SharedDictionaryCache()`、 和 `session.clear. SharedDictionaryCacheForIsolation(options)`. [#44950](https://github.com/electron/electron/pull/44950)
- 添加了 `WebFrameMain.collectJavaScriptCallStack()` 用于访问无响应渲染器的 JavaScript 调用堆栈。[#44938](https://github.com/electron/electron/pull/44938)
- 为处于卸载状态的帧添加了 `WebFrameMain.detached`。
  - 添加了 `WebFrameMain.isDestroyed()` 方法，用于判断 frame 是否已被销毁。
  - 修复了 `webFrameMain.fromId(processId, frameId)` 在框架卸载时返回的 `WebFrameMain` 实例与给定参数不匹配的问题。[#43473](https://github.com/electron/electron/pull/43473)
- 在 utility process 中增加了 error 事件，以支持有关 V8 致命错误的诊断报告。[#43774](https://github.com/electron/electron/pull/43774)
- 新功能：GPU 加速的共享纹理离屏渲染。[#42953](https://github.com/electron/electron/pull/42953)

### 重大更改

### 行为改变：在 Windows 全屏时，菜单栏将被隐藏

这使行为与 Linux 保持一致。之前的行为：在 Windows 上全屏时菜单栏仍然可见。新行为：在 Windows 全屏时隐藏菜单栏。

**更正**：之前这被列为 Electron 33 中的重大更改，但首次发布是在 Electron 34 中。

## 终止对 31.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 31.x.y 已经达到了支持的终点。我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E28 (25 年 1 月) | E35 (25 年 4 月) | E36 (25 年 6 月) |
| ---------------- | ---------------- | ---------------- |
| 34.x.y           | 35.x.y           | 36.x.y           |
| 33.x.y           | 34.x.y           | 35.x.y           |
| 32.x.y           | 33.x.y           | 34.x.y           |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
