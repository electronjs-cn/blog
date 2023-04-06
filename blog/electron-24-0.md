---
title: Electron 24.0.0
date: 2023-04-04T00:00:00.000Z
authors:
  - 
    name: georgexu99
    url: 'https://github.com/georgexu99'
    image_url: 'https://github.com/georgexu99.png?size=96'
slug: electron-24-0
---

Electron 24.0.0 已发布！ 此次升级中包含 Chromium `112.0.5615.49`，V8 `11.2`，和 Node.js `18.14.0` 。请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 24.0.0 ！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在Twitter上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

- Chromium `112.0.5615.49`
  - [Chromium 有新版本 112](https://developer.chrome.com/blog/new-in-chrome-112/)
  - [Chromium 有新版本 111](https://developer.chrome.com/blog/new-in-chrome-111/)
  - [DevTools 112 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-112/)
  - [DevTools 111 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-111/)
- Node.js `18.12.1`
  - [Node 18.12.1 博文](https://nodejs.org/en/blog/release/v18.12.1/)
- V8 `11.0`

### 重大更改

#### API Changed: `nativeImage.createThumbnailFromPath(path, size)`

`maxSize` 参数已更改为 `size` 以反映传入的大小将是创建的缩略图的大小。以前，如果图像小于 `maxSize`，Windows 不会放大图像，而 macOS 总会将大小设置为 `maxSize`。现在跨平台的行为是相同的。

```js
// 一张 128x128 的图片。
const imagePath = path.join('path', 'to', 'capybara.png');

// 放大较小的图像。
const upSize = { width: 256, height: 256 };
nativeImage.createThumbnailFromPath(imagePath, upSize).then((result) => {
  console.log(result.getSize()); // { width: 256, height: 256 }
});

// 缩小更大的图像。
const downSize = { width: 64, height: 64 };
nativeImage.createThumbnailFromPath(imagePath, downSize).then((result) => {
  console.log(result.getSize()); // { width: 64, height: 64 }
});
```

#### 已废弃：`BrowserWindow.setTrafficLightPosition(position)`

`BrowserWindow.setTrafficLightPosition(position)` 已弃用，应使用 `BrowserWindow.setWindowButtonPosition(position)` API，它接受 `null` 而不是 `{ x: 0, y: 0 }` 将位置重置为系统默认值。

```js
// 在 Electron 24 中弃用
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

- 添加了使用 `cookies.get()` 过滤 cookie 中 `HttpOnly` 的功能。 [#37365](https://github.com/electron/electron/pull/37365)
- 将 `logUsage` 添加到 `shell.openExternal()` 参数中，以允许将 `SEE_MASK_FLAG_LOG_USAGE` 标志传递给 Windows 上的 `ShellExecuteEx`。 `SEE_MASK_FLAG_LOG_USAGE` 标志表示用户发起启动，可以跟踪常用程序和其他行为。 [#37291](https://github.com/electron/electron/pull/37291)
- 为 `webRequest` 过滤器添加了 `types`，添加了过滤监听请求的能力。[#37427](https://github.com/electron/electron/pull /37427)
- 向 `webContents` 添加了一个新的 `devtools-open-url` 事件，以允许开发人员使用它们打开新窗口。 [#36774](https://github.com/electron/electron/pull/36774)
- 向 `webContents.print()` 添加了几个标准页面大小选项。 [#37265](https://github.com/electron/electron/pull/37265)
- 向会话处理程序 `ses.setDisplayMediaRequestHandler()` 的回调中添加了 `enableLocalEcho` 标志，以允许当 `audio` 为 `WebFrameMain` 时在本地输出流中回显远程音频输入。 [#37528](https://github.com/electron/electron/pull/37528)
- 允许向 `inAppPurchase.purchaseProduct()` 传递一个特定于应用程序的用户名。 [#35902](https://github.com/electron/electron/pull/35902)
- 公开了 `window.invalidateShadow()`，以清除 macOS 上残留的视觉伪影。 [#32452](https://github.com/electron/electron/pull/32452)
- 整个程序优化现在在 Electron headers 配置文件中默认启用，允许编译器利用程序中所有模块的信息进行优化，而不是以每个模块 (compiland) 为基础。 [#36937](https://github.com/electron/electron/pull/36937)
- `SystemPreferences::CanPromptTouchID` (macOS) 现在支持 Apple Watch。 [#36935](https://github.com/electron/electron/pull/36935)

## 终止对 21.x.y 的支持

根据项目的 [支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 21.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E24（23 年 4 月） | E25（23 年 5 月） | E26（23 年 8月） |
| ------------- | ------------- | ------------ |
| 24.x.y        | 25.x.y        | 26.x.y       |
| 23.x.y        | 24.x.y        | 25.x.y       |
| 22.x.y        | 23.x.y        | 24.x.y       |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
