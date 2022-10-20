> Ref: https://www.electronjs.org/blog/maintainer-summit-2022-recap


上个月，Electron 的维护者小组在加拿大温哥华举行会议，讨论了 2023 年及以后的项目方向。 在这四天的会议中，核心维护者和受邀合作者讨论了新的倡议、维护痛点和总体项目健康状态。

<figure>
  <img src="/assets/img/2022-maintainer-summit.jpg"/>
  <figcaption align="center">
    合影！取自 <a href="https://github.com/groundwater">@groundwater</a>
  </figcaption>
</figure>

今后，团队仍将全力以赴，定期快速发布 Chromium 升级和 bug 修复，以确保 Electron 对每个人都更安全、更高效。 我们也有一些令人兴奋的项目正在进行，我们很乐意与社区分享！

## 变革性的新 API

Electron 项目中需要达成共识的主要 API 提案需要通过申请 (RFC) 流程后，由我们的 API 工作组成员审核。

今年，我们推动了两项主要提案，这些提案有可能为 Electron 应用程序开启一个新的功能维度。 这些建议是实验性很强的，在这里可以先睹为快！

### 新的原生附加组件 (C API)

该提案概述了一个新的 Electron C API 层，它将允许应用程序开发人员编写他们自己的原生 Node 插件用于访问 Electron 内部资源，类似于 Node 的 Node-API。 有关拟议新 API 的更多信息可以在[这里](https://github.com/electron/governance/blob/main/wg-api/spec-documents/electron-c-apis.md)找到。

#### 示例：使用 Chromium 资源增强应用程序

许多 Electron 应用程序都有维护自己的分支，以便直接访问 Chromium 内部资源，而普通的（未修改的）Electron 是无法访问这些资源的。 通过在 C API 层中公开这些资源，这些代码可以作为原生模块与 Electron 一起存在，从而可能减少应用程序开发人员的维护负担。

### 开放 Chromium 的 UI 层 (Views API)

在底层，Chrome 用户界面 (UI) 的非网站部分，例如工具栏、选项卡或按钮，是使用名为 Views 的框架构建的。 Views API 提案将这个框架的一部分作为 Electron 中的 JavaScript 类引入，最终目标是允许开发人员为其 Electron 应用程序创建非 Web UI 元素。 这将避免应用程序不得不将网页内容集中在一起。

使这套新的 API 成为可能的基础工作目前正在进行之中。 以下是您在不久的将来可以期待的一些事。

#### 示例：将窗口模型重置为 `WebContentsView`

我们计划的第一次更改是在 Electron 的 API 中显示 Chrome 的 WebContentsView。这将 是我们现有的 BrowsView API 的继承者（尽管该名称是 Electron 指定的代码 与 Chromium 视图无关）。 随着 WebContentsView 暴露，我们将有一个可重用的 View 对象 可以显示网页内容，为使 BrowserWindow 类成为纯粹的 JavaScript 打开了大门。 更多的消除了代码复杂性。

尽管此更改并未为应用程序开发人员提供很多新功能，但它是一个大型重构，消除了许多底层代码，简化了 Chromium 升级并减少主要版本之间出现新错误的风险。

如果您是一个使用 BrowserViews 的 Electron 开发者，请勿担心，我们没有忘记您！ 我们计划将现有的 BrowserView 类变成一个 WebContentsView 的垫片（shim），以便在您过渡到较新的 API 时提供一个缓冲。

见: electron/electron#35658

#### 示例：使用 `ScrollView` 滚动网页

我们在 [Stack](https://stackbrowser.com/) 的朋友一直在推动一项倡议，将 Chromium ScrollView 组件暴露给 Electron 的 API。 通过这个新的 API，任何子视图组件都可以水平滚动或 垂直滚动。

尽管这个新的 API 实现了一个较小的功能，但该团队的最终目标是建立 一套实用的视图组件，可以作为一个工具包来构建更复杂的非 HTML 接口。

### 加入我们

你是 Electron 应用程序的开发者，对这些 API 提案感兴趣吗？ 虽然我们还没有准备好接收更多的 RFC，但请继续关注未来的更多细节!

## Electron Forge v6 稳定发布

自从该框架诞生以来，Electron 的构建工具生态系统在很大程度上是由社区驱动的。 社区驱动的，由许多小型的单用途包组成(例如 electron-installer, electron-packager, electron-notarize, electron-osx-sign)。 尽管这些工具能够正常使用，但对于用户来说，要完成一个构建工作不是一件很容易的事。

为了让 Electron 开发者有一个更友好的开发体验，我们新建了 Electron Forge，一个多合一的解决方案，将所有这些现有的工具整合到一个界面中。 一体化的解决方案，将所有这些现有的工具整合到一个单一的界面中。 虽然 Forge 自 2017 年以来一直在开发，但该项目在过去几年中一直处于停顿状态。 然而，考虑到社区对 Electron 构建工具状态的反馈，我们一直在努力发布下一代稳定的 Forge 主要版本。

Electron Forge 6 有一流的 TypeScript 和 Webpack 支持。 还有一个可扩展的 API，它允许开发者创建自己的插件和安装程序。

### 敬请关注：即将发布的公告

如果您对使用 Forge 构建项目或使用 Forge 的可扩展第三方 API 构建模板或插件感兴趣，请继续关注我们将在本月发布的 Forge v6 稳定版本的官方公告！

## 下一步是什么？

除此以外，该团队一直在思考一堆探索性项目，以使 Electron 应用开发者和终端用户获得更好的体验。 更新工具、API 审查流程和增强的文档是我们正在试验的事情。 我们希望在不久的将来有更多的消息可以与大家分享!
