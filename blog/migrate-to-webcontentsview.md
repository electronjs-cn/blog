`BrowserView` 自 [Electron 30](http://www.electronjs.org/blog/electron-30-0) 起已被弃用，现由 `WebContentView` 替代。幸运的是，迁移过程相对简单。

---

Electron 正在从 [`BrowserView`](https://www.electronjs.org/docs/latest/api/browser-view) 迁移到 [`WebContentsView`](https://www.electronjs.org/docs/latest/api/web-contents-view)，以便与 Chromium 的 UI 框架 [Views API](https://www.chromium.org/chromium-os/developer-library/guides/views/intro/) 对齐。`WebContentsView` 提供了一个可重用的 view，可以直接与 Chromium 的渲染管道相连，简化了未来的升级，并为开发者在他们的 Electron 应用中集成非网页 UI 元素打开了可能性。通过采用 `WebContentsView`，应用程序不仅为即将到来的更新做好了准备，还能在长期中受益于减少代码复杂性和潜在错误的数量。

熟悉 BrowserWindows 和 BrowserViews 的开发者应注意，`BrowserWindow` 和 `WebContentsView` 分别是从 [`BaseWindow`](https://www.electronjs.org/docs/latest/api/base-window) 和 [`View`](https://www.electronjs.org/docs/latest/api/view) 基类继承的子类。要全面了解可用的实例变量和方法，请务必查阅这些基类的文档。

## 迁移步骤

### 1. 升级 Electron 到 30.0.0 或更高

新版本 Electron 可能含有破坏性更改，影响到您的应用程序。在继续进行此迁移的其他部分之前，最好先在您的应用程序上测试并完成 Electron 升级。可以在[这里](https://www.electronjs.org/docs/latest/breaking-changes)找到每个 Electron 主版本的破坏性更改列表，以及在 Electron 博客中每个主版本的发布说明。

### 2. 熟悉您的应用程序在哪些地方使用了 BrowserViews

一种方法是搜索你的代码库中 `new BrowserView(`。这将让你了解你的应用程序是如何使用 BrowserViews 的，以及有多少需要迁移的调用点。

> 在大多数情况下，您的应用程序实例化新的 BrowserViews 时，每个实例都可以与其他实例独立迁移。

### 3. 迁移每个 `BrowserView`

1. 迁移实例化。这应该相当简单，因为 [WebContentsView](https://www.electronjs.org/docs/latest/api/web-contents-view#new-webcontentsviewoptions) 和 [BrowserView](https://www.electronjs.org/docs/latest/api/browser-view#new-browserviewoptions-experimental-deprecated) 的构造函数基本上具有相同的形式。两者都通过 `webPreferences` 参数接受 [WebPreferences](https://www.electronjs.org/docs/latest/api/structures/web-preferences)。

   ```diff
   - this.tabBar = new BrowserView({
   + this.tabBar = new WebContentsView({
   ```

2. 迁移 `BrowserView` 到添加到父窗口的地方

   ```diff
   - this.browserWindow.addBrowserView(this.tabBar)
   + this.browserWindow.contentView.addChildView(this.tabBar);
   ```

3. 迁移父窗口上的 `BrowserView` 实例方法调用

   | 旧方法                     | 新方法                                                                | 注意：                                 |
   | ----------------------- | ------------------------------------------------------------------ | ----------------------------------- |
   | `win.setBrowserView`    | `win.contentView.removeChildView` + `win.contentView.addChildView` |                                     |
   | `win.getBrowserView`    | `win.contentView.children`                                         |                                     |
   | `win.removeBrowserView` | `win.contentView.removeChildView`                                  |                                     |
   | `win.setTopBrowserView` | `win.contentView.addChildView`                                     | 在现有视图上调用 `addChildView` 会将其重新排序到顶部。|
   | `win.getBrowserViews`   | `win.contentView.children`                                         |                                     |

4. 将 `setAutoResize` 实例方法迁移到一个 resize 监听器上

   ```diff
   - this.browserView.setAutoResize({
   -   vertical: true,
   - })

   + this.browserWindow.on('resize', () => {
   +   if (!this.browserWindow || !this.webContentsView) {
   +     return;
   +   }
   +   const bounds = this.browserWindow.getBounds();
   +   this.webContentsView.setBounds({
   +     x: 0,
   +     y: 0,
   +     width: bounds.width,
   +     height: bounds.height,
   +    });
   +  });
   ```


   > 所有现有的 `browserView.webContents` 使用以及实例方法 `browserView.setBounds`、`browserView.getBounds` 和 `browserView.setBackgroundColor` 都无需迁移，并且应该与 `WebContentsView` 实例无缝兼容！


### 4 测试并提交您的更改

遇到问题了吗？请检查 Electron Issues 上的 [WebContentsView](https://github.com/electron/electron/labels/component%2FWebContentsView) 标签，以查看您遇到的问题是否已被报告。如果您在那里没有看到您的问题，请随时添加一个新的错误报告。包含测试用例 gist 将帮助我们更好地判断您的问题！

恭喜，您已经迁移到 WebContentsView！ 🎉