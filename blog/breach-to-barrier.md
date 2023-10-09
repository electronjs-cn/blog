---
title: 'Breach to Barrier: Strengthening Apps with the Sandbox'
date: 2023-09-29T00:00:00.000Z
authors:
  - name: felixrieseberg
    url: 'https://github.com/felixrieseberg'
    image_url: 'https://github.com/felixrieseberg.png?size=96'
slug: breach-to-barrier
---

# 从漏洞到屏障：用沙箱加强应用程序

自 [CVE-2023-4863: Heap buffer overflow in WebP](https://chromereleases.googleblog.com/2023/09/stable-channel-update-for-desktop_11.html) 公开以来已经过去了一个多星期，这导致了大量渲染 `webp` 图像的软件新版本的发布：macOS、iOS、Chrome、Firefox 以及各种 Linux 发行版都收到了更新。这是在 Citizen Lab 的调查之后，他们发现一个位于“华盛顿特区的民间社会组织”使用的 iPhone 正在受到 iMessage 中的零点击利用的攻击。

Electron 也迅速行动，当天发布了新版本：如果您的应用渲染任何用户提供的内容，您应该更新您的 Electron 版本 - v27.0.0-beta.2、v26.2.1、v25.8.1、v24.8.3 和 v22.3.24 都包含了一个修复版本的`libwebp`，这是一个负责渲染`webp`图像的库。

现在，鲜为人知，像“渲染图像”这样看似无害的互动实际上是一种潜在的危险活动，我们想借此机会提醒大家，Electron 带有一个进程沙箱，它将限制下一次大型攻击的影响范围（无论它是什么）。

沙箱自 Electron v1 起就可用，并在 v20 中默认启用，但我们知道许多应用（尤其是那些已经存在了很长时间的应用）在代码中可能有一个`sandbox: false`，或者一个`nodeIntegration: true`，当没有明确的`sandbox`设置时，这同样会禁用沙箱。这是可以理解的：如果您与我们相处了很长时间，您可能喜欢在运行您的 HTML/CSS 的同一段代码中加入`require("child_process")`或`require("fs")`。

在我们讨论如何迁移到沙箱之前，让我们首先讨论为什么您想要它。

沙箱为所有渲染进程设置了一个坚固的笼子，确保无论里面发生什么，代码都在一个受限的环境中执行。作为一个概念，它比 Chromium 要早得多，并且被所有主要的操作系统提供为一个特性。Electron 的和 Chromium 的沙箱都是基于这些系统特性构建的。即使您从未显示用户生成的内容，您也应该考虑到您的渲染器可能会被破坏的可能性：从供应链攻击这样复杂的情境到小错误这样简单的情境都可能导致您的渲染器做出您并未完全打算让它做的事情。

沙箱使得这种情况变得不那么可怕：沙箱内的进程可以自由地使用 CPU 周期和内存——仅此而已。进程不能写入磁盘或显示它们自己的窗口。在我们的`libwep`错误的情况下，沙箱确保攻击者不能安装或运行恶意软件。事实上，在对员工 iPhone 的原始 Pegasus 攻击中，攻击特意针对一个非沙箱化的图像进程来获得对手机的访问权限，首先突破了通常沙箱化的 iMessage 的边界。当像这个例子中的 CVE 被公布时，您仍然需要将您的 Electron 应用升级到一个安全的版本——但与此同时，攻击者能造成的损害大大减少。

将一个普通的 Electron 应用从`sandbox: false`迁移到`sandbox: true`是一项重大任务。我知道，因为尽管我亲自写了[Electron 安全指南](https://www.electronjs.org/docs/latest/tutorial/security)的初稿，但我还没有成功地迁移我的一个应用来使用它。这个周末这种情况发生了变化，我建议您也做出改变。

![不要被行数的变化吓到，大部分都在`package-lock.json`中](https://www.electronjs.org/assets/images/breach-to-barrier-741ae594fea92cc24532491071794e18.png)

您需要处理两件事：

1. 如果您在`preload`脚本或实际的`WebContents`中使用 Node.js 代码，您需要将所有 Node.js 交互移动到主进程（或者，如果您喜欢，一个实用程序进程）。考虑到渲染器已经变得如此强大，很有可能您的大部分代码实际上并不真正需要重构。

   请参考我们关于[进程间通信](https://www.electronjs.org/docs/latest/tutorial/ipc)的文档。在我的情况下，我移动了很多代码并将其包装在`ipcRenderer.invoke()`和`ipcMain.handle()`中，但这个过程是直接的并且很快完成。在这里稍微注意一下您的 API - 如果您构建了一个名为`executeCodeAsRoot(code)`的 API，沙箱不会为您的用户提供太多保护。

2. 由于启用沙箱会在您的预加载脚本中禁用 Node.js 集成，您不能再使用`require("../my-script")`。换句话说，您的预加载脚本需要是一个单独的文件。

   有多种方法可以做到这一点：Webpack、esbuild、parcel 和 rollup 都可以完成这项工作。我使用了[Electron Forge 的出色的 Webpack 插件](https://www.electronforge.io/config/plugins/webpack)，同样受欢迎的`electron-builder`的用户可以使用[`electron-webpack`](https://webpack.electron.build/)。

总的来说，整个过程花了我大约四天的时间——这包括我决定利用这个机会以其他方式重构我的代码，所以我花了很多时间思考如何驾驭 Webpack 的巨大能力。