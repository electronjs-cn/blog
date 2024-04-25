Electron 30.0.0 已发布！ 它包括对 Chromium `124.0.6367.49`、V8 `12.4` 和 Node.js `20.11.1` 的升级。

---

Electron 团队很高兴发布了 Electron 30.0.0 ！ 你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 重点内容

- Windows 现在支持 ASAR 完整性检查 ([#40504](https://github.com/electron/electron/pull/40504))
  - 启用 ASAR 完整性的现有应用程序如果配置不正确，可能无法在 Windows 上工作。使用 Electron 打包工具的应用应该升级到 `@electron/packager@18.3.1` 或 `@electron/forge@7.4.0`。
  - 查看我们的 [ASAR Integrity 教程](https://www.electronjs.org/docs/latest/tutorial/asar-integrity) 以获取更多信息。
- 添加了 [`WebContentsView`](https://www.electronjs.org/docs/latest/api/web-contents-view) 和 [`BaseWindow`](https://www.electronjs.org/docs/latest/api/base-window) 主进程模块，废弃并替换 `BrowserView` ([#35658](https://github.com/electron/electron/pull/35658))
  - `BrowserView` 现在是 `WebContentsView` 的一个壳，并且旧的实现已被移除。
  - 查看 [我们的 Web Embeds 文档](https://www.electronjs.org/docs/latest/tutorial/web-embeds) 以便将新的 `WebContentsView` API 和其他类似 API 进行比较。
- 实现了对 [File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API) 的支持 ([#41827](https://github.com/electron/electron/commit/cf1087badd437906f280373decb923733a8523e6))

### 架构（Stack）更新

- Chromium `124.0.6367.49`
  - [Chrome 124](https://developer.chrome.com/blog/new-in-chrome-124/) 和 [DevTools 124](https://developer.chrome.com/blog/new-in-devtools-124/) 中的新功能
  - [Chrome 123](https://developer.chrome.com/blog/new-in-chrome-123/) 和 [DevTools 123](https://developer.chrome.com/blog/new-in-devtools-123/) 中的新功能
- Node `20.11.1`
  - [Node 20.11.1 发布说明](https://nodejs.org/en/blog/release/v20.11.1/)
- V8 `12.4`

Electron 30 将 Chromium 从 `122.0.6261.39` 升级到 `124.0.6367.49`, Node 从 `20.9.0` 升级到 `20.11.1` 以及 V8 从 `12.2` 升级到 `12.4`。

### 新特性

- 在 webviews 中添加了 `transparent` 网页偏好设置。([#40301](https://github.com/electron/electron/pull/40301))
- 在 webContents API 上添加了一个新的实例属性 `navigationHistory`，配合 `navigationHistory.getEntryAtIndex` 方法，使应用能够检索浏览历史中任何导航条目的 URL 和标题。([#41662](https://github.com/electron/electron/pull/41662))
- 新增了 `BrowserWindow.isOccluded()` 方法，允许应用检查窗口是否被遮挡。([#38982](https://github.com/electron/electron/pull/38982))
- 为工具进程中 `net` 模块发出的请求添加了代理配置支持。([#41417](https://github.com/electron/electron/pull/41417))
- 添加了对 `navigator.serial` 中的服务类 ID 请求的蓝牙端口的支持。([#41734](https://github.com/electron/electron/pull/41734))
- 添加了对 Node.js [`NODE_EXTRA_CA_CERTS`](https://nodejs.org/api/cli.html#node_extra_ca_certsfile) 命令行标志的支持。([#41822](https://github.com/electron/electron/pull/41822))

### 重大更改

#### 行为变更：跨源 iframe 现在使用 Permission Policy 来访问功能。

跨域 iframe 现在必须通过 `allow` 属性指定一个给定 `iframe` 可以访问的功能。

有关更多信息，请参见 [文档](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#allow)。

#### 移除：`--disable-color-correct-rendering` 命令行开关

此开关从未正式文档化，但无论如何这里都记录了它的移除。Chromium 本身现在对颜色空间有更好的支持，因此不再需要该标志。

#### 行为变更：`BrowserView.setAutoResize` 在 macOS 上的行为

在 Electron 30 中，BrowserView 现在是围绕新的 [WebContentsView](https://www.electronjs.org/docs/latest/api/web-contents-view) API 的包装器。

以前，`BrowserView` API 的 `setAutoResize` 功能在 macOS 上由 [autoresizing](https://developer.apple.com/documentation/appkit/nsview/1483281-autoresizingmask?language=objc) 支持，并且在 Windows 和 Linux 上由自定义算法支持。
对于简单的用例，比如使 BrowserView 填充整个窗口，在这两种方法的行为上是相同的。
然而，在更高级的情况下，BrowserViews 在 macOS 上的自动调整大小与在其他平台上的情况不同，因为 Windows 和 Linux 的自定义调整大小算法与 macOS 的自动调整大小 API 的行为并不完全匹配。
自动调整大小的行为现在在所有平台上都标准化了。

如果您的应用使用 `BrowserView.setAutoResize` 做的不仅仅是使 BrowserView 填满整个窗口，那么您可能已经有了自定义逻辑来处理 macOS 上的这种行为差异。
如果是这样，在 Electron 30 中不再需要这种逻辑，因为自动调整大小的行为是一致的。

#### 移除：`WebContents` 上 `context-menu` 的 `params.inputFormType` 属性

`WebContents` 的 `context-menu` 事件中 params 对象的 `inputFormType` 属性已被移除。请改用新的 `formControlType` 属性。

#### 移除：`process.getIOCounters()`

Chromium 已删除对这些信息的访问。

## 终止对 27.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 27.x.y 已经达到了支持的终点。我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E30(24 年 4 月) | E31 (24 年 6 月) | E26（24 年 8 月） |
| --------------- | ---------------- | ----------------- |
| 30.x.y          | 31.x.y           | 32.x.y            |
| 29.x.y          | 30.x.y           | 31.x.y            |
| 28.x.y          | 29.x.y           | 30.x.y            |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
