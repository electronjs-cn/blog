Electron 31.0.0 已发布！ 包括升级 Chromium `126.0.6478.36`，和 V8 `12.6` 以及 Node. js `20.14.2`。

---

Electron 团队很高兴发布了 Electron 31.0.0 ！ 你可以通过 `npm install electron@latest` 或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 重点内容

- 扩展 `WebContentsView` 以接受预先存在的 `webContents` 对象 [#42319](https://github.com/electron/electron/pull/42319)
- 添加了对 `NODE_EXTRA_CA_CERTS` 的支持。 [#41689](https://github.com/electron/electron/pull/41689)
- 更新了 `window.flashFrame(bool)` 以在 macOS 上持续闪烁。 [#41391](https://github.com/electron/electron/pull/41391)
- 移除了 `WebSQL` 支持。[#41868](https://github.com/electron/electron/pull/41868)
- `nativeImage.toDataURL` 将保留 PNG 颜色空间 [#41610](https://github.com/electron/electron/pull/41610)
- 扩展 `webContents.setWindowOpenHandler` 以支持手动创建 BrowserWindow。 [#41432](https://github.com/electron/electron/pull/41432)

### 架构（Stack）更新

- Chromium `126.0.6478.36`
  - [126 新功能](https://developer.chrome.com/blog/new-in-chrome-126/)
  - [125 新功能](https://developer.chrome.com/blog/new-in-chrome-125/)
- Node `20.14.0`
  - [Node 20.14.0 博客](https://nodejs.org/en/blog/release/v20.14.0/)
- V8 `12.6`

Electron 31 将 Chromium 从 `114.0.6367.49` 升级到 `122.. 0.661.39`, Node 从 `20.11.2` 升级到 `20.14.0`，V8 从 `12.4` 升级到 `12.6`。

### 新特性

- 添加 `clearData` 方法支 `Session`. [#40983](https://github.com/electron/electron/pull/40983)
  - 添加参数到 `Session.clearData` API。 [#41355](https://github.com/electron/electron/pull/41355)
- 添加了对 `navigator.serial` 中的服务类ID请求的蓝牙端口的支持。 [#41638](https://github.com/electron/electron/pull/41638)
- 支持 Node's [`NODE_EXTRA_CA_CERTS`](https://nodejs.org/api/cli.html#node_extra_ca_certsfile) 环境变量. [#41689](https://github.com/electron/electron/pull/41689)
- 扩展 `webContents.setWindowOpenHandler` 以支持手动创建 BrowserWindow。 [#41432](https://github.com/electron/electron/pull/41432)
- 实现了对 web 标准 [File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API) 的支持。 [#41419](https://github.com/electron/electron/pull/41419)
- 扩展 WebContentsView 以接受预先存在的 webContents 对象 [#42319](https://github.com/electron/electron/pull/42319)
- 在 webContents API 上添加了一个新的实例属性 `navigationHistory`，配合 `navigationHistory.getEntryAtIndex` 方法，使应用能够检索浏览历史中任何导航条目的 URL 和标题。 [#41577](https://github.com/electron/electron/pull/41577) *(也在 [29](https://github.com/electron/electron/pull/41661), [30](https://github.com/electron/electron/pull/41662))*

### 重大更改

#### 移除: `WebSQL` 的支持

Chromium 已经在移除了对 WebSQL 的支持，仅在 Android 上保留。 更多信息，请阅读
[Chromium's 移除意图的讨论](https://groups.google.com/a/chromium.org/g/blink-dev/c/fWYb6evVA-w/m/pziWcvboAgAJ)

#### 行为变更：`nativeImage.toDataURL` 将保留 PNG 色彩空间。

PNG 解码器已支持保留颜色空间数据。 从此函数返回的编码数据现在与预期结果匹配。

更多信息，请阅读 [crbug.com/332584706](https://issues.chromium.org/issues/332584706)。

#### 行为变更：win.flashFrame(bool) 将在 macOS 上持续闪烁 Dock 图标。

这使得该行为与 Windows 和 Linux 保持一致。之前的行为：第一次调用 `flashFrame(true)` 只会使 Dock 图标弹跳一次 (使用 [NSInformationalRequest](https://developer.apple.com/documentation/appkit/nsrequestuserattentiontype/nsinformationalrequest) level)，调用 `flashFrame(false)` 不会有任何效果。 现在的行为：持续闪烁，直到调用 `flashFrame(false)`。 使用了 [NSCriticalRequest](https://developer.apple.com/documentation/appkit/nsrequestuserattentiontype/nscriticalrequest) 级别替换。 如果要明确使用 `NSInformationalRequest` 使 Dock 图标弹跳一次，仍然可以使用
[`dock.bounce('informational')`](https://www.electronjs.org/docs/latest/api/dock#dockbouncetype-macos).

## 终止对 28.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 28.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E31 (24 年 6 月)      | E26（24 年 8月）                           | E33 (24 年 10 月)     |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| 31.x.y | 32.x.y | 33.x.y |
| 30.x.y | 31.x.y | 32.x.y |
| 28.x.y | 29.x.y | 31.x.y |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
