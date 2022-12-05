> Ref: https://github.com/electron/website/blob/main/blog/electron-22-0.md
Electron 22.0.0 已发布！ 它包括了一个新的实用进程 API、对 Windows 7/8/8.1 支持的更新，以及对 Chromium `108`、V8 `10.8` 和 Node.js `16.17.1` 的升级。 
请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 22.0.0！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在Twitter上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

* Chromium `108`
    * [Chrome 108 新特性](https://developer.chrome.com/blog/new-in-chrome-108/)
    * [Chrome 107 新特性](https://developer.chrome.com/blog/new-in-chrome-107/)
    * [DevTools 108 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-108/)
    * [DevTools 107 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-107/)
* Node.js `16.17.1`
    * [Node 16.17.1 博文](https://nodejs.org/en/blog/release/v16.17.1/)
* V8 `10.8`

### 主要特性

### UtilityProcess API [#36089](https://github.com/electron/electron/pull/36089)

新的 `UtilityProcess` 主进程模块允许创建仅集成 Node.js 的轻量级 Chromium 子进程，同时还允许使用 `MessageChannel` 与沙盒渲染器进行通信。 

该API是基于Node.js的 `child_process.fork` 设计的，以允许更容易的过渡，一个主要的区别是，入口点 `modulePath` 必须来自打包的应用程序内，以允许只加载受信任的脚本。 

此外，该模块默认情况下会阻止与渲染器建立通信通道，以保证主进程是应用程序中唯一受信任进程。

您可以在此处阅读有关[我们文档中的新 UtilityProcess API](https://www.electronjs.org/docs/latest/api/utility-process) 的更多信息。

## Windows 7/8/8.1 支持更新

Electron 22 将是最后一个支持 Windows 7/8/8.1 的 Electron 主要版本。 

Electron 遵循计划中的 Chromium 弃用政策，该政策将 [在 Chromium 109 中弃用 Windows 7/8/8.1 支持（在此处阅读更多信息）](https://support.google.com/chrome/thread/185534985/sunsetting-support-for-windows-7-8-8-1-in-early -2023?hl=en)。

Electron 23 及以后的主要版本将不支持 Windows 7/8/8.1。

#### 其他显著的更改

* 添加了对 Linux 和 Windows 上的 Web Bluetooth pin 配对的支持。 [#35416](https://github.com/electron/electron/pull/35416)
* 添加了 `LoadBrowserProcessSpecificV8Snapshot` 作为新的 Fuse（引信)，让主/浏览器进程从位于 `browser_v8_context_snapshot.bin`的文件加载其 v8 快照。 任何其他进程将使用与之相同的路径。 [#35266](https://github.com/electron/electron/pull/35266)
* 添加了 `WebContents.opener` 以访问窗口打开器和 `webContents.fromFrame(frame)` 以获取与 WebFrameMain 实例对应的 WebContents。 [#35140](https://github.com/electron/electron/pull/35140)
* 通过新的会话处理程序 `ses.setDisplayMediaRequestHandler` 添加了对 `navigator.mediaDevices.getDisplayMedia` 的支持。 [#30702](https://github.com/electron/electron/pull/30702)

## 重要的API变更

以下是 Electron 22 中引入的破坏性变更。 您可以在 [破坏性变更](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面上阅读有关这些更改和未来更改的更多信息。

#### 已废弃：`webContents.incrementCapturerCount(stayHidden, stayAwake)`

`webContents.incrementCapturerCount(stayHidden, stayAwake)` 已被弃用。 当页面捕获完成时，它现在由 `webContents.capturePage` 自动处理。

```diff
const w = new BrowserWindow({ show: false })
- w.webContents.incrementCapturerCount()
- w.capturePage().then(image => {
-   console.log(image.toDataURL())
-   w.webContents.decrementCapturerCount()
- })
+ w.capturePage().then(image => {
+   console.log(image.toDataURL())
+ })
```

#### 已废弃：`webContents.decrementCapturerCount(stayHidden, stayAwake)`

`webContents.decrementCapturerCount(stayHidden, stayAwake)` 已弃用。 当页面捕获完成时，现在它由 `webContents.capturePage` 自动处理。

```diff
const w = new BrowserWindow({ show: false })
- w.webContents.incrementCapturerCount()
- w.capturePage().then(image => {
-   console.log(image.toDataURL())
-   w.webContents.decrementCapturerCount()
- })
+ w.capturePage().then(image => {
+   console.log(image.toDataURL())
+ })
```

#### 已废弃: WebContents `new-window` 事件

WebContents 中的 `new-window` 事件已经被废弃。 已弃用：[`webContents.setWindowOpenHandler()`](https://electronjs.org/docs/latest/api/web-contents#contentssetwindowopenhandlerhandler)。

```diff
- webContents.on('new-window', (event) => {
-   event.preventDefault()
- })
+ webContents.setWindowOpenHandler((details) => {
+   return { action: 'deny' }
+ })
```

#### 已废弃：Browserwindow `scroll-touch-*` 事件

在 BrowserWindow 上已弃用的 `scroll-touch-begin`、`scroll-touch-end` 和 `scroll-touch-edge` 事件已被删除。 
与此同时请使用新的 [`input-event` WebContents 上的事件](https://electronjs.org/docs/latest/api/web-contents#event-input-event)。

```diff
// Deprecated
- win.on('scroll-touch-begin', scrollTouchBegin)
- win.on('scroll-touch-edge', scrollTouchEdge)
- win.on('scroll-touch-end', scrollTouchEnd)
// Replace with
+ win.webContents.on('input-event', (_, event) => {
+   if (event.type === 'gestureScrollBegin') {
+     scrollTouchBegin()
+   } else if (event.type === 'gestureScrollUpdate') {
+     scrollTouchEdge()
+   } else if (event.type === 'gestureScrollEnd') {
+     scrollTouchEnd()
+   }
+ })
```

## 终止对 19.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 19.x.y 已经达到了支持的终点。 
我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E19 (2022年5月) | E20 (Aug'22) | E21 (2022年9月) | E22（22 年 11 月） | E23（23 年 1 月） |
| ------------- | ------------ | ------------- | -------------- | ------------- |
| 19.x.y        | 20.x.y       | 21.x.y        | 22.x.y         | 23.x.y        |
| 18.x.y        | 19.x.y       | 20.x.y        | 21.x.y         | 22.x.y        |
| 17.x.y        | 18.x.y       | 19.x.y        | 20.x.y         | 21.x.y        |

## 接下来

Electron 项目将于 2022 年 12 月暂停，并于 2023 年 1 月恢复。 更多信息可以在 [12 月关闭博客文章](https://www.electronjs.org/blog/a-quiet-place-22) 中找到。

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
