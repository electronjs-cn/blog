---
  title: Electron 25.0.0
  date: 2023-05-30T00:00:00.000Z
  authors:
    -
      name: georgexu99
      url: https://github.com/georgexu99
      image_url: https://github.com/georgexu99.png?size=96
    -
      name: vertedinde
      url: https://github.com/vertedinde
      image_url: https://github.com/vertedinde.png?size=96
  slug: electron-25-0
---

Electron 25.0.0 已发布！此次升级中包含 Chromium `114`，V8 `11.4`，和 Node.js `18.15.0`。请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 25.0.0 ！您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在Twitter上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 重点内容

- 使用 Chromium 的网络栈在 Electron 的 net 模块中实现了 `net.fetch`。和 Node 的 `fetch()` 不同的是后者使用了 Node.js 的 HTTP 栈。请参阅 [#36733](https://github.com/electron/electron/pull/36733) 和 [#36606](https://github.com/electron/electron/pull/36606).
- 添加了 `protocol.handle`，它取代并弃用了 `protocol.{register,intercept}{String,Buffer,Stream,Http,File}Protocol`。[#36674](https://github.com/electron/electron/pull/36674)
- 增加了对 Electron 22 的支持，以配合 Chromium 和 Microsoft 的 Windows 7/8/8.1 弃用计划。请参阅本博文末尾的更多详细信息。

### 架构（Stack）更新

- Chromium `114`
  - [Chromium 有新版本 114](https://developer.chrome.com/blog/new-in-chrome-114/)
  - [Chromium 有新版本 113](https://developer.chrome.com/blog/new-in-chrome-113/)
  - [DevTools 114 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-114/)
  - [DevTools 113 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-113/)
- Node.js `18.15.0`
  - [Node 18.15.0 博文](https://nodejs.org/en/blog/release/v18.15.0/)
- V8 `11.4`

### 重大更改

#### 已废弃：`protocol.{register,intercept}{Buffer,String,Stream,File,Http}Protocol`

`protocol.register*Protocol` 和 `protocol.intercept*Protocol` 方法已替换为 [`protocol.handle`](api/protocol.md#protocolhandlescheme-handler)。

新方法可以注册新协议或拦截现有协议协议和响应可以是任何类型。

```js
// 在 Electron 25 中弃用
protocol.registerBufferProtocol('some-protocol', () => {
  callback({ mimeType: 'text/html', data: Buffer.from('<h5>Response</h5>') });
});

// 代替为
protocol.handle('some-protocol', () => {
  return new Response(
    Buffer.from('<h5>Response</h5>'), // 也可以是 string or ReadableStream。
    { headers: { 'content-type': 'text/html' } }
  );
});
```

```js
// 在 Electron 25 中弃用
protocol.registerHttpProtocol('some-protocol', () => {
  callback({ url: 'https://electronjs.org' });
});

// 代替为
protocol.handle('some-protocol', () => {
  return net.fetch('https://electronjs.org');
});
```

```js
// 在 Electron 25 中弃用
protocol.registerFileProtocol('some-protocol', () => {
  callback({ filePath: '/path/to/my/file' });
});

// 代替为
protocol.handle('some-protocol', () => {
  return net.fetch('file:///path/to/my/file');
});
```

#### 已废弃：`BrowserWindow.setTrafficLightPosition(position)`

`BrowserWindow.setTrafficLightPosition(position)` 已弃用，应使用 `BrowserWindow.setWindowButtonPosition(position)` API，它接受 `null` 而不是 `{ x: 0, y: 0 }` 将位置重置为系统默认值。

```js
// 在 Electron 25 中弃用
win.setTrafficLightPosition({ x: 10, y: 10 });
win.setTrafficLightPosition({ x: 0, y: 0 });

// 替换为
win.setWindowButtonPosition({ x: 10, y: 10 });
win.setWindowButtonPosition(null);
```

#### 已废弃： `BrowserWindow.getTrafficLightPosition()`

`BrowserWindow.getTrafficLightPosition()` 已弃用，应使用 `BrowserWindow.getWindowButtonPosition()` API，当没有自定义位置时，它返回 `null` 而不是 `{ x: 0, y: 0 }` 。

```js
// 在 Electron 24 中弃用
const pos = win.getTrafficLightPosition()
if (pos.x === 0 && pos.y === 0) {
  // 没有自定义位置。
}

// 替换为
const ret = win.getWindowButtonPosition();
if (ret === null) {
  // 没有自定义位置。
}
```

### 新特性

- 已添加 `net.fetch()`。[#36733](https://github.com/electron/electron/pull/36733)
  - `net.fetch` 支持对 `file:` URL 和使用 `protocol.register*Protocol` 注册的自定义协议的请求。
- 添加了 BrowserWindow.set/getWindowButtonPosition API。[#37094](https://github.com/electron/electron/pull/37094)
- 添加了 `protocol.handle`，替换并弃用了 `protocol.{register,intercept}{String,Buffer,Stream,Http,File}Protocol`。[#34418](https://github.com/electron/electron/pull/34418)
- 向 `webContents` 和 `webview` 标签添加了一个 `will-frame-navigate` 事件，每当 frame 层中的任何一个 frame 调用 navigate 时，都会触发该函数。[#34418](https://github.com/electron/electron/pull/34418)
- 向 navigator 事件添加了启动信息。这个信息允许区分来自父框架 `window.open` 引起的 navigation 事件，而不是来自子框架发起的 navigation 事件。[#37085](https://github.com/electron/electron/pull/37085)
- 增加了 net.resolveHost，它使用 defaultSession 对象来解析 hosts。[#38152](https://github.com/electron/electron/pull/38152)
- 向 `app` 添加了新的 'did-resign-active' 事件。[#38018](https://github.com/electron/electron/pull/38018)
- 向 `webContents.print()` 添加了几个标准页面大小选项。[#37159](https://github.com/electron/electron/pull/37159)
- 向会话处理程序 `ses.setDisplayMediaRequestHandler()` 的回调中添加了 `enableLocalEcho` 标志，以允许当 `audio` 为 `WebFrameMain` 时在本地输出流中回显远程音频输入。[#37315](https://github.com/electron/electron/pull/37315)
- 向 `powerMonitor` 添加了 thermal 信息管理。[#38028](https://github.com/electron/electron/pull/38028)
- 允许将绝对路径传递给 session.fromPath() API。[#37604](https://github.com/electron/electron/pull/37604)
- 在 `webContents` 上公开 `audio-state-changed` 事件。[#37366](https://github.com/electron/electron/pull/37366)

## 终止对 22.x.y 的支持

正如再见 [再见, Windows 7/8/8.1](https://www.electronjs.org/blog/windows-7-to-8-1-deprecation-notice) 所述，Electron 22（Chromium 108）的计划寿命终止日期将从 2023 年 5 月 30 日延长至 2023 年 10 月 10 日。Electron 团队将会继续向 Electron 22 提供本计划中的任何安全修复，直到 2023 年 10 月 10 日。十月
支持日期遵循 Chromium 和 Microsoft 的延长支持日期。10 月 11 日，Electron 团队将支持回退到最新的三个稳定主要版本，将不再支持 Windows 7/8/8.1。

| E25（23 年 5 月） | E26（23 年 8 月） | E26（23 年 10 月） |
| ----------------- | ----------------- | ------------------ |
| 25.x.y            | 26.x.y            | 27.x.y             |
| 24.x.y            | 25.x.y            | 26.x.y             |
| 23.x.y            | 24.x.y            | 25.x.y             |
| 22.x.y            | 22.x.y            | --                 |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
