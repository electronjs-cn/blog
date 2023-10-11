---
title: Electron 27.0.0
date: 2023-10-10T00:00:00.000Z
authors:
  - 
    name: vertedinde
    url: 'https://github.com/vertedinde'
    image_url: 'https://github.com/vertedinde.png?size=96'
slug: electron-27-0
---

Electron 27.0.0 已发布！ 此次升级中包含 Chromium `118.0.5993.32`，V8 `11.8`，和 Node.js `18.17.1` 。

---

Electron 团队很高兴发布了 Electron 27.0.0 ！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

- Chromium `118.0.5993.32`
  - [Chromium 有新版本 118](https://developer.chrome.com/blog/new-in-chrome-118/)
  - [DevTools 118 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-118/)
- Node.js `18.17.1`
  - [Node 18.17.1 博文](https://nodejs.org/en/blog/release/v18.17.1/)
  - [Node 18.17.0 博文](https://nodejs.org/en/blog/release/v18.17.0/)
- V8 `11.8`

### 重大更改

### 已移除：macOS 10.13 / 10.14 / 支援

macOS 10.13 (High Serria) 和 macOS 10.14 (Mojave) 不再支援[Chromium](https://chromium-review.googlesource.com/c/chromium/src/+/4629466).

Older versions of Electron will continue to run on these operating systems, but macOS 10.15 (Catalina) or later will be required to run Electron v27.0.0 and higher.

### 弃用：`ipcRenderer.sendTo()`

`ipcRenderer.sendTo()` 已被弃用。 可以通过在渲染器之间设置一个 [`MessageChannel`](https://www.electronjs.org/docs/latest/tutorial/message-ports#setting-up-a-messagechannel-between-two-renderers) 来替换它。

`IpcRendererEvent` 的 `senderId` 和 `senderIsMainFrame` 属性也已被弃用。

### 已删除： `systemPreferences` 中的 color scheme 相关事件

以下 `systemPreferences` 事件已被删除：

- `inverted-color-scheme-changed`
- `high-contrast-color-scheme-changed`

请改用 `nativeTheme` 模块上的新 `updated` 事件。

```js
// 已删除
systemPreferences.on('inverted-color-scheme-changed', () => {
  /* ... */
});
systemPreferences.on('high-contrast-color-scheme-changed', () => {
  /* ... */
});

// 替换为
nativeTheme.on('updated', () => {
  /* ... */
});
```

### 已删除: `webContents.getPrinters`

`webContents.getPrinters` 方法已被删除。 使用`webContents.getPrintersAsync`代替。

```js
const w = new BrowserWindow({ show: false });

// 已删除
console.log(w.webContents.getPrinters());
// 替换为
w.webContents.getPrintersAsync().then((printers) => {
  console.log(printers);
});
```

### 已删除：`systemPreferences.{get,set}AppLevelAppearance` 和 `systemPreferences.appLevelAppearance`

方法`systemPreferences.getAppLevelAppearance` 和 `systemPreferences.setAppLevelAppearance` 已被删除，也包括属性 `systemPreferences.appLevelAppearance`。 请改用模块 `nativeTheme`。

```js
// 已删除
systemPreferences.getAppLevelAppearance();
// 替换为
nativeTheme.shouldUseDarkColors;

// 已删除
systemPreferences.appLevelAppearance;
// 替换为
nativeTheme.shouldUseDarkColors;

// 已删除
systemPreferences.setAppLevelAppearance('dark');
// 替换为
nativeTheme.themeSource = 'dark';
```

### 已删除：`systemPreferences.getColor` 的 `alternate-selected-control-text` 值

`systemPreferences.getColor` 的 `alternate-selected-control-text` 值已被删除。 替换为 `selected-content-background`。

```js
// 已删除
systemPreferences.getColor('alternate-selected-control-text');
// 替换为
systemPreferences.getColor('selected-content-background');
```

### 新特性

- 添加了应用可访问性透明度设置 api [#39631](https://github.com/electron/electron/pull/39631)
- 添加了对 `chrome.scripting` 扩展 API 的支持 [#39675](https://github.com/electron/electron/pull/39675)
- 默认启用 `WaylandWindowDecorations` [#39644](https://github.com/electron/electron/pull/39644)

## 终止对 24.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 24.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E27（23 年 10 月） | E22 (23 年 12 月) | E29(24 年 2 月) |
| -------------- | --------------- | ------------- |
| 27.x.y         | 28.x.y          | 29.x.y        |
| 26.x.y         | 27.x.y          | 28.x.y        |
| 25.x.y         | 26.x.y          | 27.x.y        |

## 终止对 22.x.y 的支持

今年早些时候，Electron 团队将 Electron 22 的计划生命终止日期从 2023 年 5 月 30 日延长至 2023 年 10 月 10 日，以配合 Chrome 对 Windows 7/8/8.1 的扩展支持（详情请查看 [Farewell, Windows 7/8/8.1](https://www.electronjs.org/blog/windows-7-to-8-1-deprecation-notice)）。

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)和此支持扩展，Electron 22.x.y 已经达到了支持的终点。 这将恢复到支持最新的三个稳定的主要版本，并将结束对Windows 7/8.1的官方支持。

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
