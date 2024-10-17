Electron 33.0.0 已发布！ 它包括对 Chromium 130.0.6723.44、V8 13.0 和 Node 20.18.0 的升级。

---

Electron 团队很高兴发布了 Electron 33.0.0！ 你可以通过 `npm install electron@latest` 或者从我们的[发布网站](https://releases.electronjs.org/releases/stable)下载它。 继续阅读此版本的详细信息。

如果您有任何反馈，请在 [Twitter](https://twitter.com/electronjs) 或 [Mastodon](https://social.lfx.dev/@electronjs) 上与我们分享，或加入我们的 [Discord](https://discord.com/invite/electronjs) 社区！ Bug 和功能请求可以在 Electron 的[问题跟踪器](https://github.com/electron/electron/issues)中报告。

## 重要变化

### 重点内容

- 添加了一个处理程序 `app.setClientCertRequestPasswordHandler(handler)`，以帮助在需要 PIN 时解锁加密设备。 [#41205](https://github.com/electron/electron/pull/41205)
- 扩展 `navigationHistory` API，增加两个新功能以改善 history 管理。 [#42014](https://github.com/electron/electron/pull/42014)
- 改善了原生主题透明度检查。 [#42862](https://github.com/electron/electron/pull/42862)

### 架构（Stack）更新

- Chromium `130.0.6723.44`
  - [130 新功能](https://developer.chrome.com/blog/new-in-chrome-130/)
  - [129 新功能](https://developer.chrome.com/blog/new-in-chrome-129/)
- Node `20.18.0`
  - [Node 20.18.0 博客](https://nodejs.org/en/blog/release/v20.18.0/)
  - [Node 20.17.0 博客](https://nodejs.org/en/blog/release/v20.17.0/)
- V8 `13.0`

Electron 33 将 Chromium 从 `128.0.6613.36` 升级到 `130.0.6723.44`， Node 从 `20.16.0` 升级到 `20.18.0` 以及 V8 从 `12.8` 升级到 `13.0`。

### 新特性

- 添加了一个处理程序 `app.setClientCertRequestPasswordHandler(handler)`，以帮助在需要 PIN 时解锁加密设备。 [#41205](https://github.com/electron/electron/pull/41205)
- 在 utility process 中增加了 error 事件，以支持有关 V8 致命错误的诊断报告。 [#43997](https://github.com/electron/electron/pull/43997)
- 新增了 `View.setBorderRadius(radius)` 以自定义 view 的边框半径，并与 `WebContentsView` 兼容。 [#42320](https://github.com/electron/electron/pull/42320)
- 扩展 `navigationHistory` API，增加两个新功能以改善 history 管理。 [#42014](https://github.com/electron/electron/pull/42014)

### 重大更改

#### 移除：macOS 10.15 支持

macOS 10.15（Catalina）不再受到 [Chromium](https://chromium-review.googlesource.com/c/chromium/src/+/5734361) 的支持。

旧版本的 Electron 将继续在 Catalina 上运行，但要运行 Electron v33.0.0 及更高版本，将需要 macOS 11（Big Sur）或更高版本。

#### 行为变化：Native modules 现在需要 C++20

由于上游的改动，[V8](https://chromium-review.googlesource.com/c/v8/v8/+/5587859) 和 [Node.js](https://github.com/nodejs/node/pull/45427) 现在要求 C++20 作为最低版本。 使用原生 node 模块的开发人员应该使用 `--std=c++20` 而不是 `--std=c++17` 来构建他们的模块。 使用 gcc9 或更低版本的镜像可能需要更新到 gcc10 才能进行编译。 详见： [#43555](https://github.com/electron/electron/pull/43555)

#### 行为变化：Windows 上的自定义协议 URL 处理

由于在 Chromium 中进行的更改，以支持[非特殊方案 URL](http://bit.ly/url-non-special)，使用 Windows 文件路径的自定义协议 URL 将不再与已弃用的 `protocol.registerFileProtocol` 和 `BrowserWindow.loadURL`、`WebContents.loadURL` 以及 `<webview>.loadURL` 上的 `baseURLForDataURL` 属性工作。 `protocol.handle` 也无法处理这些类型的 URL，但这并不是一个变化，因为它一直都是这样的工作方式。

```js
// No longer works
protocol.registerFileProtocol('other', () => {
  callback({ filePath: '/path/to/my/file' });
});

const mainWindow = new BrowserWindow();
mainWindow.loadURL(
  'data:text/html,<script src="loaded-from-dataurl.js"></script>',
  { baseURLForDataURL: 'other://C:\\myapp' }
);
mainWindow.loadURL('other://C:\\myapp\\index.html');

// Replace with
const path = require('node:path');
const nodeUrl = require('node:url');
protocol.handle(other, (req) => {
  const srcPath = 'C:\\myapp\\';
  const reqURL = new URL(req.url);
  return net.fetch(
    nodeUrl.pathToFileURL(path.join(srcPath, reqURL.pathname)).toString()
  );
});

mainWindow.loadURL(
  'data:text/html,<script src="loaded-from-dataurl.js"></script>',
  { baseURLForDataURL: 'other://' }
);
mainWindow.loadURL('other://index.html');
```

#### 行为变化：`app` 上的 `login` 的 `webContents` 属性

`app` 中的 `login` 事件的 `webContents` 属性在事件因来自于使用 `respondToAuthRequestsFromMainProcess` 选项创建的 [utility process](https://www.electronjs.org/docs/latest/api/utility-process) 的请求而被触发时，将为 `null`。

#### 已弃用: `BrowserWindowConstructorOption.type` 中的 `textured` 选项

`BrowserWindowConstructorOptions` 中的 `type` 的 `textured` 选项已被弃用，且没有替代方案。 此选项依赖于 macOS 上的 [`NSWindowStyleMaskTexturedBackground`](https://developer.apple.com/documentation/appkit/nswindowstylemask/nswindowstylemasktexturedbackground) 样式掩码，该样式已被弃用且没有替代方案。

#### 已弃用：`systemPreferences.accessibilityDisplayShouldReduceTransparency`

`systemPreferences.accessibilityDisplayShouldReduceTransparency` 属性现已被弃用，取而代之的是新的 `nativeTheme.prefersReducedTransparency`，它提供相同的信息并支持跨平台使用。

```js
// Deprecated
const shouldReduceTransparency =
  systemPreferences.accessibilityDisplayShouldReduceTransparency;

// Replace with:
const prefersReducedTransparency = nativeTheme.prefersReducedTransparency;
```

## End of Support for 30.x.y

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 30.x.y 已经达到了支持的终点。 我们鼓励开发者将应用程序升级到更新的 Electron 版本。

| E33 (24 年 10 月)     | E28 (25 年 1 月)      | E35 (25 年 4 月)      |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| 33.x.y | 34.x.y | 35.x.y |
| 32.x.y | 33.x.y | 34.x.y |
| 31.x.y | 32.x.y | 33.x.y |

## 接下来

在短期内，您可以期待团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium、Node 和 V8。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在[计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md)页面找到。
