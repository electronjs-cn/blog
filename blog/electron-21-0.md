> Ref: https://github.com/electron/electronjs.org-new/blob/main/blog/electron-21-0.md

Electron 21.0.0 已发布！ 此次升级中包含 Chromium `106`, V8 `10.6`, 和 Node.js `16.16.0`。 请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 21.0.0！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读以了解当前版本的更多详细信息。

如果您有任何反馈，请在Twitter上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

* Chromium `106`
    * [Chrome 106 新特性](https://developer.chrome.com/blog/new-in-chrome-106/)
    * [Chrome 105 新特性](https://developer.chrome.com/blog/new-in-chrome-105/)
    * [DevTools 106 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-106/)
    * [DevTools 105 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-105/)
* Node.js `16.16.0`
    * [Node 16.16.0 博文](https://nodejs.org/en/blog/release/v16.16.0/)
* V8 `10.6`

### 新特性

* 已添加 `webFrameMain.origin`。 [#35534](https://github.com/electron/electron/pull/35534)
* 添加新的 `WebContents.ipc` 和 `WebFrameMain.ipc` API。 [#35231](https://github.com/electron/electron/pull/35231)
* 添加支持类似面板的行为。 窗口可以浮动在全屏应用上。 [#34388](https://github.com/electron/electron/pull/34388)
* 添加对 macOS 应用的 APN 推送通知的支持。 [#33574](https://github.com/electron/electron/pull/33574)

## 不兼容的 API 变更

以下是 Electron 21 中引入的不兼容变更。

### V8 Memory Cage 已启用

Electron 21 启用 [V8 沙盒指针](https://docs.google.com/document/d/1HSap8-J3HcrZvT7-5NsbYWcjfc0BVoops5TDHZNsnko/edit)，继Chrome [决定在 Chrome 103](https://chromiumdash.appspot.com/commit/9a6a76bf13d3ca1c6788de193afc5513919dd0ed) 做同样的事情。 这对原生模块有一定的影响。 此功能具有性能和安全优势，但也对原生模块施加了一些新的限制，例如，使用指向外部（“堆外”）内存的ArrayBuffers。 更多信息请查看 [博客文章](https://electronjs.org/blog/v8-memory-cage)。 [#34724](https://github.com/electron/electron/pull/34724)

### 重构 webContents.printToPDF

重构了 `webContents.printToPDF` 以与 Chromium headless 实现对齐。 更多信息请见 [#33654](https://github.com/electron/electron/pull/33654)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://www.electronjs.org/docs/latest/breaking-changes) 页面找到。

## 终止对 18.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 18.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E18 (2022年3月) | E19 (2022年5月) | E20 (2022年8月) | E21 (2022年9月) | E22 (2022年12月) |
| ------------- | ------------- | ------------- | ------------- | -------------- |
| 18.x.y        | 19.x.y        | 20.x.y        | 21.x.y        | 22.x.y         |
| 17.x.y        | 18.x.y        | 19.x.y        | 20.x.y        | 21.x.y         |
| 16.x.y        | 17.x.y        | 18.x.y        | 19.x.y        | 20.x.y         |

## 接下来

在短期内，你可以期待我们的团队继续专注于跟上构成 Electron 的主要组件的发展，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
