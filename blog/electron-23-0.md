---
title: Electron 23.0.0
date: 2023-2-07T00:00:00.000Z
authors:
  - 
    name: VerteDinde
    url: 'https://github.com/VerteDinde'
    image_url: 'https://github.com/vertedinde.png?size=96'
  - 
    name: georgexu99
    url: 'https://github.com/georgexu99'
    image_url: 'https://github.com/georgexu99.png?size=96'
  - 
    name: erickzhao
    url: 'https://github.com/erickzhao'
    image_url: 'https://github.com/erickzhao.png?size=96'
slug: electron-23-0
---

Electron 23.0.0 已发布！ 此次升级中包含 Chromium `110`, V8 `11.0`, 和 Node.js `18.12.1`。  此外，取消了对Windows 7/8.1的支持。 请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 23.0.0！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在Twitter上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

* Chromium `110`
    * [Chromium有新版本110](https://developer.chrome.com/blog/new-in-chrome-110/)
    * [Chromium有新版本109](https://developer.chrome.com/blog/new-in-chrome-109/)
    * [DevTools 110 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-110/)
    * [DevTools 109 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-109/)
* Node.js `18.12.1`
    * [Node 18.12.1 博文](https://nodejs.org/en/blog/release/v18.12.1/)
* V8 `11.0`

### 新特性

* 将 `label` 属性添加到 [`Display`](https://www.electronjs.org/docs/latest/api/structures/display) 对象上。 [#36933](https://github.com/electron/electron/pull/36933)
* 添加了 `app.getPreferredSystemLanguages()` API 以返回用户的系统语言。 [#36035](https://github.com/electron/electron/pull/36035)
* 添加了对 [WebUSB](https://developer.mozilla.org/en-US/docs/Web/API/WebUSB_API) API 的支持。 [#36289](https://github.com/electron/electron/pull/36289)
* 添加了对 [`SerialPort.forget()`](https://developer.mozilla.org/en-US/docs/Web/API/SerialPort/forget) 的支持，以及当给定来源被撤销时在 [Session](https://www.electronjs.org/docs/latest/api/session) 对象上发出的新事件 `serial-port-revoked` 。 [#35310](https://github.com/electron/electron/pull/35310)
* 添加了新的 `win.setHiddenInMissionControl` API 以允许开发者选择退出 macOS 上的任务控制。 [#36092](https://github.com/electron/electron/pull/36092)

## 放弃对 Windows 7/8.1 支持

Electron 23 不再支持Windows 7/8.1。 Electron 遵循计划中的 Chromium 弃用政策，该政策将 [在 Chromium 109 中弃用 Windows 7/8/8.1 支持（在此处阅读更多信息）](https://support.google.com/chrome/thread/185534985/sunsetting-support-for-windows-7-8-8-1-in-early-2023?hl=en)。

## 重要的API变更

以下是 Electron 23 中引入的破坏性变更。 您可以在 [Planned Breaking Changes](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面上阅读有关这些更改和未来更改的更多信息。

### 已删除：BrowserWindow `scroll-touch-*` 事件

在 BrowserWindow 上已弃用的 `scroll-touch-begin`、`scroll-touch-end` 和 `scroll-touch-edge` 事件已被移除。 与此同时，可在 WebContents 上使用最新可用的 `input-event` 事件。

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
+})
```

## 终止对 20.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 20.x.y 已经达到了支持的终点。 Developers and applications are encouraged to upgrade to a newer version of Electron.

| E22（22 年 11 月） | E23（23 年 1 月） | E24（23 年 4 月） | E25（23 年 5 月） | E26（23 年 8月） |
| -------------- | ------------- | ------------- | ------------- | ------------ |
| 22.x.y         | 23.x.y        | 24.x.y        | 25.x.y        | 26.x.y       |
| 21.x.y         | 22.x.y        | 23.x.y        | 24.x.y        | 25.x.y       |
| 20.x.y         | 21.x.y        | 22.x.y        | 23.x.y        | 24.x.y       |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
