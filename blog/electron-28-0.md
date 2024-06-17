Electron 28.0.0 已发布！ 它包括升级  Chromium `120.0.6099.56` 和 V8 `12.0` 以及 Node.js `18.18.2`。

***

Electron 团队很高兴发布了 Electron 28.0.0 ！ 你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

## 重点内容

- 实现了对 ECMAScript 模块或 ESM 的支持（什么是 ECMAScript 模块？） [在这里了解更多信息](https://nodejs.org/api/esm.html#modules-ecmascript-modules). 这包括在 Electron 本身中支持 ESM，以及诸如 `UtilityProcess` API 入口点等方面。 [详情见我们的 ESM 文档](https://www.electronjs.org/docs/latest/tutorial/esm) 以获取更多详细信息。
- 除了在 Electron 本身中启用 ESM 支持外，Electron Forge 还支持使用 ESM 来打包、构建和开发 Electron 应用程序。 您可以在 [Forge v7.0.0](https://github.com/electron/forge/releases/tag/v7.0.0) 或更高版本中找到这种支持。

### 架构（Stack）更新

- Chromium `120.0.6099.56`
  - 新的 [Chrome 119](https://developer.chrome.com/blog/new-in-chrome-119/) 和 [DevTools 119](https://developer.chrome.com/blog/new-in-devtools-119/)
  - 新的 [Chrome 120](https://developer.chrome.com/blog/new-in-chrome-120/) 和 [DevTools 120](https://developer.chrome.com/blog/new-in-devtools-120/)
- Node `18.18.2`
  - [Node 18.18.0 博文](https://nodejs.org/en/blog/release/v18.18.0/)
  - [Node 18.18.1 博文](https://nodejs.org/en/blog/release/v18.18.1/)
  - [Node 18.18.2 博文](https://nodejs.org/en/blog/release/v18.18.2/)
- V8 `12.0`

### 新特性

- 启用 ESM 支持。 [#37535](https://github.com/electron/electron/pull/37535)
  - 更多细节，请见 [ESM documentation](https://www.electronjs.org/docs/latest/tutorial/esm)。
- 为 `UtilityProcess` API 添加了 ESM 入口点。 [#40047](https://github.com/electron/electron/pull/40047)
- 添加了几个属性到 display 对象中，包括 `detected`，`maximumCursorSize` 和 `nativeOrigin`。 [#40554](https://github.com/electron/electron/pull/40554)
- 新增对 Linux 上 `ELECTRON_OZONE_PLATFORM_HINT` 环境变量的支持。 [#39792](https://github.com/electron/electron/pull/39792)

### 重大更改

#### 行为改变：在宿主 `BrowserWindow` 中将 `WebContents.backgroundThrottling` 设置为 `false` 将影响所有的 `WebContents`。

将 `WebContents.backgroundThrottling` 设置为 `false` 将禁用由 `BrowserWindow` 显示的所有 `WebContents` 的帧节流。

#### 移除: `BrowserWindow.setTrafficLightPosition(position)`

已经移除了 `BrowserWindow.setTrafficLightPosition(position)` 方法，应该改用 `BrowserWindow.setWindowButtonPosition(position)`方法。新的方法接受 `null` 作为替换 `{ x: 0, y: 0 }` 的参数，以将位置重置为系统默认值。

```js
// 在 Electron 28 移除
win.setTrafficLightPosition({ x: 10, y: 10 });
win.setTrafficLightPosition({ x: 0, y: 0 });

// 代替为
win.setWindowButtonPosition({ x: 10, y: 10 });
win.setWindowButtonPosition(null);
```

#### 移除: `BrowserWindow.getTrafficLightPosition()`

已经删除了 `BrowserWindow.getTrafficLightPosition()`，应该使用 `BrowserWindow.getWindowButtonPosition()` API 代替，当没有自定义位置时，它返回 null 而不是 `{ x: 0, y: 0 }`。

```js
// 在 Electron 28 移除
const pos = win.getTrafficLightPosition();
if (pos.x === 0 && pos.y === 0) {
  // 没有自定义位置
}

// 代替为
const ret = win.getWindowButtonPosition();
if (ret === null) {
  // 没有自定义位置
}
```

#### 移除: `ipcRenderer.sendTo()`

`ipcRenderer.sendTo()` API 已经被删除。 应该替换为在 `Node` 和渲染器之间建立一个 [`MessageChannel`](tutorial/message-ports.md#setting-up-a-messagechannel-between-two-renderers) 来进行通讯。

`IpcRendererEvent` 的 `senderId` 和 `senderIsMainFrame` 属性也已经被移除。

#### 移除: `app.runningUnderRosettaTranslation`

`app.runningUnderRosettaTranslation` 属性已经被移除。
使用 `app.runningUnderARM64Translation` 代替.

```js
// 移除
console.log(app.runningUnderRosettaTranslation);
// 代替为
console.log(app.runningUnderARM64Translation);
```

## 终止对 25.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 25.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E28(23 年 12 月) | E29(24 年 2 月) | E30(24 年 4 月) |
| -------------- | ------------- | ------------- |
| 28.x.y         | 29.x.y        | 30.x.y        |
| 27.x.y         | 28.x.y        | 29.x.y        |
| 26.x.y         | 27.x.y        | 28.x.y        |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
