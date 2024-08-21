Electron 32.0.0 已发布！包括升级 Chromium `128.0.6613.36`，和 V8 `12.8` 以及 Node.js `20.16.2`。

---

Electron 团队很高兴发布了 Electron 32.0.0！你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 重点内容

- 在我们的文档中添加新的 API 版本历史，一个由 @piotrpdev 创建的功能，作为 Google Summer 代码的一部分。[#42982](https://github.com/electron/electron/pull/42982)
- 从 Web 文件 API 中移除非标准 `File.path` 扩展。[#42053](https://github.com/electron/electron/pull/42053)
- 尝试打开受阻止路径中的文件或目录时，将 Web [File System API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API) 中的故障路径与上游对齐。[#42993](https://github.com/electron/electron/pull/42993)
- 将以下现有的导航相关 API 添加到 `webContents.navigationHistory` : `canGoBack`, `goBack`, `canGoForward`, `goForward`, `canGoToOffset`, `goToOffset`, `clear`。旧的导航 API 现已被废弃。[#41752](https://github.com/electron/electron/pull/41752)

### 架构（Stack）更新

- Chromium `128.0.6613.36`
  - [128 新功能](https://developer.chrome.com/blog/new-in-chrome-128/)
  - [127 新功能](https://developer.chrome.com/blog/new-in-chrome-127/)
- Node `20.16.0`
  - [Node 20.16.0 博客](https://nodejs.org/en/blog/release/v20.16.0/)
- V8 `12.8`

Electron 32 将 Chromium 从 `126.0.6478.36` 升级到 `128.0.6613.36`， Node 从 `20.14.0` 升级到 `20.16.1` 以及 V8 从 `12.6` 升级到 `12.8`。

### 新特性

- 添加了对通过 `app` 模块的 [`'login'`](https://www.electronjs.org/docs/latest/api/app) 事件，响应来自实用程序进程发起的认证请求的支持。[#43317](https://github.com/electron/electron/pull/43317)
- 在 `CPUUsage` 结构中添加了 `cumulativeCPUUsage` 属性，该属性返回自进程启动以来使用的 CPU 时间的总秒数。[#41819](https://github.com/electron/electron/pull/41819)
- 将以下现有的导航相关 API 添加到 `webContents.navigationHistory` : `canGoBack`， `goBack`， `canGoForward`， `goForward`， `canGoToOffset`， `goToOffset`， `clear`。[#41752](https://github.com/electron/electron/pull/41752)
- 扩展 `WebContentsView` 以接受预先存在的 `webContents` 对象。[#42086](https://github.com/electron/electron/pull/42086)
- 在 `nativeTheme` 中添加了一个新属性 `prefersReducedTransparency` ，该属性指示用户是否选择通过系统辅助功能设置来降低操作系统级别的透明度。[#43137](https://github.com/electron/electron/pull/43137)
- 尝试打开阻塞路径中的文件或目录时，将文件系统访问 API 中的故障路径与上游对齐。[#42993](https://github.com/electron/electron/pull/42993)
- 在 Linux 上启用 Windows 控制叠加层 API。[#42681](https://github.com/electron/electron/pull/42681)
- 在网络请求中启用 `zstd` 压缩。[#43300](https://github.com/electron/electron/pull/43300)

### 重大更改

#### 移除: `File.path`

Electron 的早期版本在 Web [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File) 对象中添加了非标准 `path` 属性作为在渲染器中执行所有操作更为常见时处理本机文件的便捷方法。然而它偏离了标准，并且也带来了较小的安全风险，因此从 Electron 32 开始它已被移除，取而代之的是 [`webUtils.getPathForFile`](https://github.com/electron/electron/tree/main/docs/api/web-utils.md#webutilsgetpathforfilefile) 方法。

```js
// Before (renderer)
const file = document.querySelector('input[type=file]');
alert(`Uploaded file path was: ${file.path}`);
```

```js
// After (renderer)
const file = document.querySelector('input[type=file]');
electron.showFilePath(file);

// After (preload)
const { contextBridge, webUtils } = require('electron');

contextBridge.exposeInMainWorld('electron', {
  showFilePath(file) {
    // It's best not to expose the full file path to the web content if
    // possible.
    const path = webUtils.getPathForFile(file);
    alert(`Uploaded file path was: ${path}`);
  },
});
```

#### 废弃：`WebContents` 中的 `clearHistory`, `canGoBack`, `goBack`, `canGoForward`, `goForward`, `goToIndex`, `canGoToOffset`, `goToOffset`

在 `WebContents` 实例上与导航相关的API现在已被废弃。这些 API 已被移动到 `WebContents` 的 `navigationHistory` 属性，以便为管理导航历史提供一个更有条理和直观的接口。

```js
// Deprecated
win.webContents.clearHistory();
win.webContents.canGoBack();
win.webContents.goBack();
win.webContents.canGoForward();
win.webContents.goForward();
win.webContents.goToIndex(index);
win.webContents.canGoToOffset();
win.webContents.goToOffset(index);

// Replace with
win.webContents.navigationHistory.clear();
win.webContents.navigationHistory.canGoBack();
win.webContents.navigationHistory.goBack();
win.webContents.navigationHistory.canGoForward();
win.webContents.navigationHistory.goForward();
win.webContents.navigationHistory.canGoToOffset();
win.webContents.navigationHistory.goToOffset(index);
```

## 终止对 29.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 29.x.y 已经达到了支持的终点。我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E26（24 年 8月）                           | E33 (24 年 10 月)     | E28 (25 年 1 月)      |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| 32.x.y | 33.x.y | 34.x.y |
| 31.x.y | 32.x.y | 33.x.y |
| 30.x.y | 31.x.y | 32.x.y |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
