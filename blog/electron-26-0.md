---
title: Electron 26.0.0
date: 2023-08-15T00:00:00.000Z
authors:
  - 
    name: vertedinde
    url: 'https://github.com/vertedinde'
    image_url: 'https://github.com/vertedinde.png?size=96'
slug: electron-26-0
---

Electron 26.0.0 已发布！ 此次升级中包含 Chromium `116.0.5845.62`，V8 `11.2`，和 Node.js `18.16.1` 。 请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 26.0.0 ！ 您可以通过 `npm install electron@latest` 进行安装，或者从我们的 [发布网站](https://releases.electronjs.org/releases/stable) 下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 Twitter 上与我们分享，或加入我们的社区 [Discord](https://discord.com/invite/electronjs)！ Bug 和功能请求可以在 Electron 的 [问题跟踪器](https://github.com/electron/electron/issues) 中报告。

## 重要变化

### 架构（Stack）更新

- Chromium `116.0.5845.62`
  - [Chromium 有新版本 116](https://developer.chrome.com/blog/new-in-chrome-116/)
  - [DevTools 116 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-116/)
- Node.js `18.16.1`
  - [Node 18.16.1 博文](https://nodejs.org/en/blog/release/v18.16.1/)
- V8 `11.2`

### 重大更改

### `webContents.getPrinters` 被弃用

`webContents.getPrinters` 方法已经被弃用。 使用`webContents.getPrintersAsync`代替。

```js
const w = new BrowserWindow({ show: false });

// 被弃用
console.log(w.webContents.getPrinters());
// 使用它来代替
w.webContents.getPrintersAsync().then((printers) => {
  console.log(printers);
});
```

### `systemPreferences.{get,set}AppLevelAppearance` 和 `systemPreferences.appLevelAppearance` 被弃用

方法`systemPreferences.getAppLevelAppearance` 和 `systemPreferences.setAppLevelAppearance` 已被弃用，也包括属性 `systemPreferences.appLevelAppearance`。 请改用模块 `nativeTheme`。

```js
// 被弃用
systemPreferences.getAppLevelAppearance();
// 替换为
nativeTheme.shouldUseDarkColors;

// 被弃用
systemPreferences.appLevelAppearance;
// 替换为
nativeTheme.shouldUseDarkColors;

// 被弃用
systemPreferences.setAppLevelAppearance('dark');
// 替换为
nativeTheme.themeSource = 'dark';
```

### `systemPreferences.getColor` 的 `alternate-selected-control-text` 值被弃用

`systemPreferences.getColor` 的 `alternate-selected-control-text` 值已被弃用。 替换为 `selected-content-background`。

```js
// 被弃用
systemPreferences.getColor('alternate-selected-control-text');
// 替换为
systemPreferences.getColor('selected-content-background');
```

### 新特性

- 添加了 `safeStorage.setUsePlainTextEncryption` 和 `safeStorage.getSelectedStorageBackend` api。 [#39107](https://github.com/electron/electron/pull/39107)
- 添加了 `safeStorage.setUsePlainTextEncryption` 和 `safeStorage.getSelectedStorageBackend` api。 [#39155](https://github.com/electron/electron/pull/39155)
- 添加了 `senderIsMainFrame` to messages sent via `ipcRenderer.sendTo()`. [#39206](https://github.com/electron/electron/pull/39206)
- 添加了由键盘初始化菜单弹出的支持。 [#38954](https://github.com/electron/electron/pull/38954)

## 终止对 23.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 23.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E26（23 年 8月） | E27（23 年 10 月） | E28 (Jan'24) |
| ------------ | -------------- | ------------ |
| 26.x.y       | 27.x.y         | 28.x.y       |
| 25.x.y       | 26.x.y         | 27.x.y       |
| 24.x.y       | 25.x.y         | 26.x.y       |
| 22.x.y       |                |              |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
