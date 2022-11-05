---
title: 介绍 Electron Forge 6
date: 2022-11-03T00:00:00.000Z
authors:
  - 
    name: georgexu99
    url: 'https://github.com/georgexu99'
    image_url: 'https://github.com/georgexu99.png?size=96'
  - 
    name: vertedinde
    url: 'https://github.com/vertedinde'
    image_url: 'https://github.com/vertedinde.png?size=96'
  - 
    name: erickzhao
    url: 'https://github.com/erickzhao'
    image_url: 'https://github.com/erickzhao.png?size=96'
slug: forge-v6-release
toc_max_heading_level: 3
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

我们很高兴地宣布 Electron Forge v6.0.0 现已推出！ 此版本标志着自 2018 年以来 Forge 的第一个主要版本，并将该项目从 `electron-userland` 转移到 GitHub 上的 `Electron` 组织。

继续阅读以了解新功能以及您的应用如何调用 Electron Forge！

## Electron Forge 是什么？

[Electron Forge](https://electronforge.io) 是一个用于打包和分发 Electron 应用程序的工具。 它将 Electron 的构建工具生态系统统一到一个可扩展的界面中，这样每个人都可以直接上手制作 Electron 应用。

它的亮点：
* 📦 应用打包和代码签名
* 🚚 Windows、macOS 和 Linux 上的可定制安装程序（DMG、deb、MSI、PKG、AppX 等）
* ☁️ 云提供商（GitHub、S3、Bitbucket 等）的自动化发布流程
* ⚡️ 易于使用的 Webpack 和 TypeScript 样板模板
* ⚙️ 原生 Node.js 模块支持
* 🔌 可扩展的 JavaScript 插件 API

:::info 延伸阅读

访问 [Why Electron Forge][] 文档，了解更多关于 Forge 的理念和架构信息。

:::

## v6 中有什么新功能？

### 完全重写

从 v1 到 v5，Electron Forge 是在现已停止的 [`electron-compile`](https://www.npmjs.com/package/electron-compile) 项目的基础上进行开发的。 Forge 6 是对项目的完全重写，具有新的模块化架构，可以扩展以满足大多数 Electron 应用程序的需求。

在过去的几年里，Forge `v6.0.0-beta` 已经实现了与 v5 相同的功能，并且代码流失速度显着放缓，使该工具为普遍采用做好了准备。

:::caution 不要安装错误的软件包

对于 v5 及更低的版本，Electron Forge 已在 npm 上发布了 `electron-forge` 包。 从 v6 重写开始，Forge 被构建为一个带有许多较小项目的 monorepo 项目。

:::

### 官方支持

从历史上看，Electron 的维护者对构建工具不感兴趣，而是把这个任务留给各个由社区维护的工具包。 然而，随着 Electron 项目的成熟，新的 Electron 开发人员越来越难以理解他们需要哪些工具来构建和分发他们的应用程序。

为了在分发过程中帮助指导 Electron 开发人员，**我们决定让 Forge 成为 Electron 的官方 batteries-included（内置方法）的构建流**。

在过去的一年里，我们一直在慢慢地将 Forge 集成到官方的 Electron 文档中，最近我们将 Forge 从其在 `electron-userland/electron-forge` 的旧地址转移到了 [electron/forge](https://github.com/electron/forge) 存储库。 现在，我们终于准备好向广大用户发布 Electron Forge 了！

## 入门指南

### 初始化一个新的 Forge 项目

可以使用 `create-electron-app` CLI 脚本来搭建一个新的 Electron Forge 项目。

<Tabs>
  <TabItem value="Yarn" label="Yarn" default>

```bash
yarn create electron-app my-app --template=webpack
cd my-app
yarn start
```

  </TabItem>
  <TabItem value="npm" label="npm">

```bash
npm init electron-app@latest my-app --template=webpack
cd my-app
npm start
```

  </TabItem>
</Tabs>

该脚本将在 `my-app` 文件夹中创建一个 Electron 项目，其中包含完整的 JavaScript Bundling 及预先配置好的构建工作流。

有关详细信息，请参阅 Forge 文档中的[入门指南][]。

:::info 优先支持 webpack

上面的代码片段使用了 Forge 的 [Webpack 模板][]，我们建议将其作为新 Electron 项目的初始化配置。 此模板是围绕 [`@electron-forge/plugin-webpack`](https://www.electronforge.io/config/plugins/webpack) 插件构建的，该插件以几种方式将 webpack 与 Electron Forge 集成，包括：

- 使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 增强本地开发流程，包括在渲染器中支持 HMR；
- 在应用程序打包之前处理 webpack 包的构建逻辑； 并且
- 在 webpack bundling 过程中添加对 Native Node 模块的支持。

如果您需要 TypeScript 支持，请考虑改用 [Webpack + TypeScript][] 模板。

:::

### 导入现有项目

Electron Forge CLI 还包含现有 Electron 项目的导入命令。

<Tabs>
  <TabItem value="Yarn" label="Yarn" default>

```bash
cd my-app
yarn add --dev @electron-forge/cli
yarn electron-forge import
```

  </TabItem>
  <TabItem value="npm" label="npm">

```bash
cd my-app
npm install --save-dev @electron-forge/cli
npm exec --package=@electron-forge/cli -c "electron-forge import"
```

  </TabItem>
</Tabs>

当您使用 `import` 命令时，Electron Forge 将添加一些核心依赖项并创建一个新的 `forge.config.js` 配置。 如果您有任何现有的构建工具（例如 Electron Packager、Electron Builder 或 Forge 5），它将尝试迁移尽可能多的设置。 您的某些现有配置可能需要手动迁移。

手动迁移详细信息可以在 Forge [导入文档][]中找到。 如果您需要帮助，请访问我们的 [Discord](https://discord.gg/f4cH9BzaDw) 服务器！

## 为什么要切换到 Forge？

如果您已经有了用于打包和发布 Electron 应用程序的工具，那么采用 Electron Forge 带来的好处仍然可以超过最初地转换成本。

我们认为使用 Forge 有两个主要好处：

1. **Forge 在 Electron 支持的新功能出现后，会立即得到这些新功能并用于应用程序的构建**。 在这种情况下，您不需要自己编写新的工具去实现，或者在升级之前等待其他更新包实现。 有关最近的示例，请参阅 [macOS 通用二进制文件](https://github.com/electron/universal)和 [ASAR 完整性检查](https://www.electronjs.org/docs/latest/tutorial/asar-integrity)。

1. **Forge 的多包架构使其易于理解和扩展**。 是由于 Forge 由许多具有明确职责的较小包组成，因此更容易遵循代码流。 此外，Forge 的可扩展 API 设计意味着你可以在所提供的配置选项之外为高级用例编写自己的额外构建逻辑。 有关编写自定义 Forge 插件、制作者和发布者的更多详细信息，请参阅文档的扩展 [Electron Forge][] 部分。

## 重大变更

Forge 6 已经在 beta 阶段花了很长时间，它的发布节奏也逐渐放缓。 然而，我们在 2022 年下半年加速了开发，并利用在 v6.0.0 稳定版本之前的最后几个版本推出了一些最终的重大更改。

如果您是 Electron Forge 6 测试版用户，请参阅 [v6.0 GitHub 更新日志](https://github.com/electron/forge/releases/tag/v6.0.0) 了解最近测试版中所做的重大变更 (`>=6.0.0-beta.65`)。

完整的更改和提交列表可以在仓库的 [CHANGELOG.md](https://github.com/electron/forge/blob/main/CHANGELOG.md) 中找到。

## 提交反馈

告诉我们您的想法！ Electron Forge 团队一直在尝试提升用户在构建项目时候的开发体验。

您可以通过提交功能请求、[提交 issues](https://github.com/electron/forge/issues) 或直接联系我们来进行反馈及改进 Electron Forge！ 您也可以加入我们的 [官方 Electron Discord 英文社区](https://discord.com/invite/electronjs)，那里有一个专门用于 Electron Forge 讨论的频道。

如果您想在 https://electronforge.io 上对 Forge 文档提供任何反馈，我们有一个 GitBook 实例同步到 [electron-forge/electron-forge-docs](https://github.com/electron-forge/electron-forge-docs) 存储库。

<!-- links -->

[入门指南]: https://www.electronforge.io/
[导入文档]: https://www.electronforge.io/import-existing-project

[Webpack 模板]: https://www.electronforge.io/templates/webpack-template
[Webpack + TypeScript]: https://www.electronforge.io/templates/webpack-typescript-template
[Electron Forge]: https://www.electronforge.io/advanced/extending-electron-forge
[Why Electron Forge]: https://www.electronforge.io/core-concepts/why-electron-forge
