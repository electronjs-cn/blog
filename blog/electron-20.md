> Ref: https://github.com/electron/electronjs.org-new/blob/main/blog/electron-20-0.md

Electron 20.0.0 已发布！它包括升级到 Chromium `104`, V8 `10.4`, 和 Node.js `16.15.0`。请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 20.0.0！您可以通过 `npm install electron@later` 进行安装，或者从我们的 [发布网站](https://www.electronjs.org/releases/stable) 下载它。继续阅读有关此版本的详细信息，并请分享您的任何反馈！

## 重要变化

### 新特性

* 在 Windows 上添加了沉浸式暗黑模式。[#34549](https://github.com/electron/electron/pull/34549)
* 添加支持类似面板的行为。窗口可以浮动在全屏应用上。[#34665](https://github.com/electron/electron/pull/34665)
* 更新了 Windows 控件覆盖按钮，使其在 Windows 11 上的外观和感觉更加原生。[#34888](https://github.com/electron/electron/pull/34888)
* 现在开始，渲染器在默认情况下会被沙盒化，除非指定了 `nodeIntegration: true` 或 `sandbox: false`。[#35125](https://github.com/electron/electron/pull/35125)
* 添加了使用 nan 构建原生模块时的保护措施。[#35160](https://github.com/electron/electron/pull/35160)

### Stack Changes

* Chromium `104`
    * [Chrome 104 新特性](https://developer.chrome.com/blog/new-in-chrome-104/)
    * [Chrome 103 新特性](https://developer.chrome.com/blog/new-in-chrome-103/)
    * [DevTools 中的新增功能](https://developer.chrome.com/blog/new-in-devtools-104/)
* Node.js `16.15.0`
    * [Node 16.15.0 博文](https://nodejs.org/en/blog/release/v16.15.0/)
* V8 `10.4`

## 破坏性 & API 更改

以下是 Electron 20 中引入的破坏性变化。有关这些和未来变化的更多信息可在 [计划的突破性变化](https://www.electronjs.org/docs/latest/breaking-changes) 页面找到。

### V8 Memory Cage 已启用

Electron 20 启用 [V8 沙盒指针](https://docs.google.com/document/d/1HSap8-J3HcrZvT7-5NsbYWcjfc0BVoops5TDHZNsnko/edit)，继 Chrome [决定在 Chrome 103](https://chromiumdash.appspot.com/commit/9a6a76bf13d3ca1c6788de193afc5513919dd0ed) 做同样的事情。这对原生模块有一定的影响。此功能具有性能和安全优势，但也对原生模块施加了一些新的限制，例如，使用指向外部（“堆外”）内存的ArrayBuffers。更多信息请查看 [博客文章](https://electronjs.org/blog/v8-memory-cage)。

### 默认值被更改：默认情况下，渲染器不为 `nodeIntegration: true` 将进行沙盒处理

之前, 指定预加载脚本的渲染器默认不启用沙盒。这意味着默认情况下，预加载脚本可以访问 Node.js。在 Electron 20中，此默认值将被更改。从 Electron 20 开始，渲染器 默认情况下会被沙盒化，除非指定了 `nodeIntegration: true` 或 `sandbox: false`。

如果预加载脚本不依赖于 Node，则无需执行任何操作。如果 preload 脚本依赖于 Node，请重构代码，或从渲染器中删除 Node 用法 ，或者显式指定相关渲染器 `sandbox: false`。

### 修复：nan原生模块自发崩溃的问题

在 Electron 20 中，我们更改了两个与原生模块相关的项目：
1. V8 headers 现在默认使用 `c++17`。这个标志已添加到 electron-rebuild 了。
1. 我们修复了一个问题，即一个缺失的 include 导致依赖于 nan 的原生模块自发崩溃。

为了获得最大的稳定性，我们建议在重新构建原生模块时使用 node-gyp >= 8.4.0 和 electron-rebuild >= 3.2.9，特别是依赖于nan的模块。有关更多信息，请参阅 electron [#35160](https://github.com/electron/electron/pull/35160) 和 node-gyp [#2497](https://github.com/nodejs/node-gyp/pull/2497)。

### 已删除：Linux 上的 `.skipTaskbar`

在 X11上, `skipTaskbar` 向 X11 窗口管理器发送一条 `_NET_WM_STATE_SKIP_TASKBAR` 消息。Wayland 没有与其一致的功能，并且已知的 变通办法具有不可接受的理由（如，在 GNOME 中 Window.is_skip_taskbar 需要不安全模式），因此 Electron 无法在 Linux 上支持此功能。

## 终止对 17.x.y 的支持

根据项目的[支持政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy)，Electron 17.x.y 已经达到了支持的终点。我们鼓励开发者和应用程序升级到更新的 Electron 版本。

| E18 (Mar'22) | E19 (May'22) | E20 (Aug'22) | E21 (Sep'22) | E22 (Dec'22) |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| 18.x.y       | 19.x.y       | 20.x.y       | 21.x.y       | 22.x.y       |
| 17.x.y       | 18.x.y       | 19.x.y       | 20.x.y       | 21.x.y       |
| 16.x.y       | 17.x.y       | 18.x.y       | 19.x.y       | 20.x.y       |
| 15.x.y       | --           | --           | --           | --           |

## 下一步做什么

在短期内，你可以期待我们的团队继续专注于跟上构成 Electron 的主要组件的发展，包括 Chromium、Node 和 V8。虽然我们谨慎地不对发布日期做出承诺，但我们的计划是大约每2个月发布一次带有新版本组件的主要版本的 Electron。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的突破性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
