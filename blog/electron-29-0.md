Electron 29.0.0 已发布！ 它包括升级 Chromium `122.0.6261.39`，和 V8 `12.2` 以及 Node.js `20.9.2`。

***

Electron 团队很高兴发布了 Electron 29.0.0 ！ 你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 重点内容

- 添加了一个新的顶级 `webUtils` 模块，这是一个渲染进程模块，提供与 Web API 对象交互的实用程序层。 模块中第一个可用的 API 是 `webUtils.getPathForFile`。 Electron 以前的 `File.path` 扩充偏离了网页标准；这个新的 API 更符合当前的 web 标准行为。

### 架构（Stack）更新

- Chromium `122.0.6261.39`
  - 新的 [Chrome 122](https://developer.chrome.com/blog/new-in-chrome-122/) 和 [DevTools 122](https://developer.chrome.com/blog/new-in-devtools-122/)
  - 新的 [Chrome 121](https://developer.chrome.com/blog/new-in-chrome-121/) 和 [DevTools 121](https://developer.chrome.com/blog/new-in-devtools-121/)
- Node `20.9.0`
  - [Node 20.9.0 博文](https://nodejs.org/en/blog/release/v20.9.0/)
  - [Node 20.0.0 博文](https://nodejs.org/en/blog/release/v20.0.0/)
- V8 `12.2`

Electron 29 将 Chromium 从 `120.0.6099.56` 升级到 `122..0.661.39`, Node 从 `18.18.2` 升级到 `20.9.0`，V8 从 `12.0` 升级到 `12.2`。

### 新特性

- 添加了新的 `webUtils` 模块，一个与 Web API 对象交互的实用程序层，替换 `File.path` 扩充。 [#38776](https://github.com/electron/electron/pull/38776)
- 添加 [net](https://www.electronijs.org/docs/latest/api/net) 模块到 [utility process](https://www.electronijs.org/docs/latest/glossary#utility-process)。 [#40890](https://github.com/electron/electron/pull/40890)
- 添加了一个新的 [Electron Fuse](https://www.electronjs.org/docs/latest/tutorial/fuses)， `grantFileProtocolExtravileges` 让 `file://` 协议使其具有更安全和更严格的行为，与 Chromium 相匹配。 [#40372](https://github.com/electron/electron/pull/40372)
- 在 `protocol.registerSchemesAsseged` 中添加了一个选项，以允许自定义协议开启 V8 code cache。 [#40544](https://github.com/electron/electron/pull/40544)
- 迁移 `app.{set|get}LoginItemSettings(settings)` 以便在 MacOS 13.0+ 上使用 Apple 推荐的新底层框架。 [#37244](https://github.com/electron/electron/pull/37244)

### 重大更改

#### 行为改变：`ipcRenderer` 不能再通过 `contextBridge` 发送

现在通过 `contextBridge` 传送整个 `ipcRenderer` 模块作为对象，会在 bridge 的接收方收到一个
空对象。 此更改是为了消除/减轻一种安全隐患。 你不应该直接暴露 bridge 上的 ipcRenderer 或它的方法。
相反，应该提供一个像下面这样的安全包装器：

```js
contextBridge.exposeInMainWorld('app', {
  onEvent: (cb) => ipcRenderer.on('foo', (e, ...args) => cb(args)),
});
```

#### 移除：`app` 上的 `render-process-crashed` 事件

The `renderer-process-crashed` event on `app` has been removed.
改用新的 `render-process-gone` 事件。

```js
// 移除
app.on('renderer-process-crashed', (event, webContents, killed) => {
  /* ... */
});

// 替换为
app.on('render-process-gone', (event, webContents, details) => {
  /* ... */
});
```

#### 移除：`WebContents` 和 `<webview> `上的 `crashed` 事件

`WebContents` 和 `<webview>` 上的 `crashed` 事件已被移除。
改用新的 `render-process-gone` 事件。

```js
// 移除
win.webContents.on('crashed', (event, killed) => {
  /* ... */
});
webview.addEventListener('crashed', (event) => {
  /* ... */
});

// 替换为
win.webContents.on('render-process-gone', (event, details) => {
  /* ... */
});
webview.addEventListener('render-process-gone', (event) => {
  /* ... */
});
```

#### Removed: `gpu-process-crashed` event on `app`

`app` 上的 `gpu-process-crashed` 事件已被移除。
使用新的 `child-process-gone` 事件代替。

```js
// 移除
app.on('gpu-process-crashed', (event, killed) => {
  /* ... */
});

// 替换为
app.on('child-process-gone', (event, details) => {
  /* ... */
});
```

## 终止对 26.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 26.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E29(24 年 2 月) | E30(24 年 4 月) | E31 (24 年 6 月) |
| -------------------------------- | -------------------------------- | --------------------------------- |
| 29.x.y                           | 30.x.y                           | 31.x.y                            |
| 28.x.y                           | 29.x.y                           | 30.x.y                            |
| 27.x.y                           | 28.x.y                           | 29.x.y                            |

## 接下来

你是否知道 Electron 最近添加了社区征求意见 (RFC) 流程？ 如果你想在框架中添加一项功能，RFC 可以成为你与维护者就功能设计展开对话的有用工具。 你还可以查看 PR 中正在讨论的即将发生的更改。若要了解更多信息，请直接查看我们的 [introduction electron/rfcs](https://www.electronijs.org/blog/rfcs) 博客文章，或查看 [electron/rfcs](https://www.github.com/electron/rfcs) 版本库的 README。

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
