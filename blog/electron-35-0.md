Electron 35.0.0 已发布！ 它包括对 Chromium 134.0.6998.44、V8 13.5 和 Node 22.14.0 的升级。

---

Electron 团队很高兴发布了 Electron 35.0.0！你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Bluesky](https://bsky.app/profile/electronjs.org) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 用于改进扩展支持的 Service Worker 预加载脚本

最初由 [@samuelmaddock](https://github.com/samuelmaddock) 在 [RFC #8](https://github.com/electron/rfcs/blob/main/text/0008-preload-realm.md) 中提出，Electron 35 添加了将预加载脚本附加到 [Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) 的功能。 由于 Chrome 的 Manifest V3 Extensions 通过 [扩展 service workers](https://developer.chrome.com/docs/extensions/develop/concepts/service-workers) 处理大量工作，该功能填补了 Electron 对现代 Chrome 扩展程序支持的空白。

在会话级别以编程方式注册预加载脚本时，现在可以使用 [`ses.registerPreloadScript(script)`](https://www.electronjs.org/docs/latest/api/session#sesregisterpreloadscriptscript) API 将其专门应用于 Service Worker 上下文。

```js title='Main Process'
// 将我们的预加载脚本添加到会话中。
session.defaultSession.registerPreloadScript({
  // 我们的脚本应该只在 service worker 预加载中运行。
  type: 'service-worker',
  // 脚本的绝对路径。
  script: path.join(__dirname, 'extension-sw-preload.js'),
});；
```

此外，现在可以通过 `ServiceWorkerMain.ipc` 类在 Service Workers 及其附加的预加载脚本之间进行 IPC 通信。 预加载脚本仍将使用 `ipcRenderer` 模块与其 Service Worker 进行通信。 请参阅原始 RFC 以了解更多详细信息。

此功能之前已经进行了许多其他更改，为其奠定了基础：

- [#45329](https://github.com/electron/electron/pull/45329) 重新设计了 Session 模块的预加载 API，以支持注册和取消注册单独的预加载脚本。
- [#45229](https://github.com/electron/electron/pull/45330) 添加了实验性的 `contextBridge.executeInMainWorld(executionScript)` 脚本，用于通过 context bridge 在 main world 中执行 JavaScript。
- [#45341](https://github.com/electron/electron/pull/45341) 添加了 `ServiceWorkerMain` 类，用于与主进程中的 Service Workers 交互。

### 架构（Stack）更新

- Chromium `134.0.6998.44`
  - [134 新功能](https://developer.chrome.com/blog/new-in-chrome-134/)
  - [133 新功能](https://developer.chrome.com/blog/new-in-chrome-133/)
- Node `22.14.0`
  - [Node 22.14.0 博客](https://nodejs.org/en/blog/release/v22.14.0/)
- V8 `13.5`

Electron 35 将 Chromium 从 `132.0.6834.83` 升级到 `134.0.6998.44`，Node 从 `20.18.1` 升级到 `22.14.0`，V8 从 `13.2` 升级到 `13.5`。

### 新特性

- 在 Info.plist 中添加了 `NSPrefersDisplaySafeAreaCompatibilityMode` = `false`，以移除应用选项中的 “Scale to fit below built-in camera” 选项。 [#45357](https://github.com/electron/electron/pull/45357) <sup>（也适用于 [v34.1.0](https://github.com/electron/electron/pull/45469)）</sup>
- 在主进程中添加了 `ServiceWorkerMain` 类，用于与服务工作线程进行交互。 [#45341](https://github.com/electron/electron/pull/45341)
  - 在 `ServiceWorkers` 上添加了 `running-status-changed` 事件，用于指示服务工作线程的运行状态何时发生更改。
  - 在 `ServiceWorkers` 上添加了 `startWorkerForScope` 方法，用于启动可能先前已停止的工作线程。
- 添加了实验性的 `contextBridge.executeInMainWorld` 方法，用于安全地跨 world 边界执行代码。 [#45330](https://github.com/electron/electron/pull/45330)
- 在 `'console-message'` 事件中添加了 `frame` 属性。 [#43617](https://github.com/electron/electron/pull/43617)
- 在 Windows 上添加了 `query-session-end` 事件并改进了 `session-end` 事件。 [#44598](https://github.com/electron/electron/pull/44598)
- 添加 `view.getVisible()`。 [#45409](https://github.com/electron/electron/pull/45409)
- 添加了 `webContents.navigationHistory.restore(index, entries)` API，允许恢复导航记录。 [#45583](https://github.com/electron/electron/pull/45583)
- 在 `BrowserWindow.setVibrancy` 中添加了可选的动画参数。 [#35987](https://github.com/electron/electron/pull/35987)
- 为 `document.executeCommand("paste")` 添加了权限支持。 [#45471](https://github.com/electron/electron/pull/45471) <sup>（同样在 [v34.1.0](https://github.com/electron/electron/pull/45472) 中）</sup>
- 在 Windows 上添加了对 `roundedCorners` BrowserWindow 构造函数选项的支持。 [#45740](https://github.com/electron/electron/pull/45740) <sup>（同样在 [v34.3.0](https://github.com/electron/electron/pull/45739) 中）</sup>
- 添加了对 service worker 线程预加载脚本的支持。 [#45408](https://github.com/electron/electron/pull/45408)
- 支持 Portal 的 `globalShortcuts`。 Electron 必须使用 `--enable-features=GlobalShortcutsPortal` 运行，该功能才能生效。 [#45297](https://github.com/electron/electron/pull/45297)

## 重大更改

### 移除了 `PrinterInfo` 上的 `isDefault` 和 `status` 属性。

这些属性已从 `PrinterInfo` 对象中移除，因为它们已从上游 Chromium 中移除。

### 已弃用：`session.serviceWorkers` 上的 `getFromVersionID`。

`session.serviceWorkers.fromVersionID(versionId)` API 已被弃用，建议使用 `session.serviceWorkers.getInfoFromVersionID(versionId)`。
此更改是为了在引入 `session.serviceWorkers.getWorkerFromVersionID(versionId)` API 后，更清楚地表明返回的是哪个对象。

```js
// 已弃用
session.serviceWorkers.fromVersionID(versionId);

// 替换为
session.serviceWorkers.getInfoFromVersionID(versionId);
```

### 已弃用：`Session` 上的 `setPreloads`、`getPreloads`。

`registerPreloadScript`、`unregisterPreloadScript` 和 `getPreloadScripts` 被引入作为对已弃用方法的替代。 这些新的 API 允许第三方库注册 preload scripts，而无需替换现有脚本。 此外，新的 `type` 选项允许超出 `frame` 的其他预加载目标。

```js
// 已弃用
session.setPreloads([path.join(__dirname, 'preload.js')]);

// 替换为：
session.registerPreloadScript({
  type: 'frame',
  id: 'app-preload',
  filePath: path.join(__dirname, 'preload.js'),
});

```

### 已弃用：`WebContents` 上的 `console-message` 事件中的 `level`、`message`、`line` 和 `sourceId` 参数。

`WebContents` 上的 `console-message` 事件已更新，以提供有关 `Event` 参数的详细信息。

```js
// 已弃用
webContents.on(
  'console-message',
  (event, level, message, line, sourceId) => {},
);

// 替换为：
webContents.on(
  'console-message',
  ({ level, message, lineNumber, sourceId, frame }) => {},
);

```

此外，`level` 现在是一个字符串，可能的值为 `info`、`warning`、`error` 和 `debug`。

### 行为变更：`WebRequestFilter` 的 `urls` 属性。

之前，一个空的 URLs 数组被解释为包含所有 URL。 为了明确包含所有 URL，开发人员现在应该使用 `<all_urls>` 模式，这是一个与所有可能的 URL 匹配的 [指定 URL 模式](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Match_patterns#all_urls)。 此更改阐明了意图，并确保行为更加可预测。

```js
// 已弃用
const deprecatedFilter = {
  urls: [],
};

// 替换为
const newFilter = {
  urls: ['<all_urls>'],
};

```

### 已弃用：`systemPreferences.isAeroGlassEnabled()`。

`systemPreferences.isAeroGlassEnabled()` 函数已被弃用，且没有替代方案。
自 Electron 23 起，它一直返回 `true`，因为 Electron 23 仅支持 Windows 10+，而 Windows 10+ 中 DWM 合成已无法禁用。

https://learn.microsoft.com/en-us/windows/win32/dwm/composition-ovw#disabling-dwm-composition-windows7-and-earlier

## 终止对 32.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 32.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E35 (2025 年 3 月)    | E36 (2025 年 4 月)    | E37 (2025 年 6 月)    |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| 35.x.y | 36.x.y | 37.x.y |
| 34.x.y | 35.x.y | 36.x.y |
| 33.x.y | 34.x.y | 35.x.y |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
