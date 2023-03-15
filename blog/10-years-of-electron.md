---
title: Electron 10 周年🎉
date: 2023-03-13T00:00:00.000Z
authors:
  name: erickzhao
  url: 'https://github.com/erickzhao'
  image_url: 'https://github.com/erickzhao.png?size=96'
slug: 10-years-of-electron
---

> Ref: https://github.com/electron/website/blob/main/blog/10-years-of-electron.md

# Electron 10 周年🎉

2013年3月13日[^1] 首次被提交到 `electron/electron` 仓库。

![由 @aroben 进行的 electron/electron 的初始提交。](https://www.electronjs.org/assets/images/first-commit-5f8ec3d1e3a503b28aa098198e12e1ef.png)

经过10年和来自 1192 位贡献者的 27147 次提交之后，Electron 已成为如今构建桌面应用程序最受欢迎的框架之一。 这个里程碑是一个完美的机会，可以庆祝和回顾我们迄今为止的旅程，并分享我们沿途所学到的东西。

如果没有每个人都献出时间和努力为该项目做出贡献，我们今天就不会在这里了。 尽管源代码提交总是最显著的贡献，但我们还必须承认那些报告错误、维护用户空间模块、提供文档和翻译，并参与网络空间中的 Electron 社区的人所做出的努力。 对于我们作为维护者来说，每一份贡献都是无价之宝。

**在我们继续博客文章的其余部分之前：谢谢你们。 ❤️**

## 我们怎么发展到现在的？

**Atom Shell** 是为 GitHub [Atom 编辑器](https://atom.io/) 打造的基础框架，该编辑器于 2014 年 4 月公开发布 beta 版。 它是从头建立的，作为当时可用的基于网络的桌面框架的替代品(node-webkit 和 Chrome 嵌入式框架)。 它有一个杀手级功能：嵌入 Node.js 和 Chromium，为网页 技术提供一个强大的桌面运行时间。

一年之内，Atom Shell 的功能和知名度开始大幅增长。 大型公司、初创公司和个人开发者都已开始使用它构建应用程序。(一些早期采用者包括 [Slack](https://slack.com), [GitKraken](https://gitkraken.com) 和 [WebTorrent](https://webtorrent.io)), 该项目被恰当地重命名为 **Electron**

从那时起，Electron 便一发不可收拾，从未停止过。 这里是每周下载量随时间变化的趋势，由 [npmtrends.com](https://npmtrends.com/electron) 提供:

![Electron 每周下载量随时间变化的图表](https://www.electronjs.org/assets/images/weekly-download-graph-4626defd22a1516f5d335e0a6f40beb2.png)

Electron v1 于 2016 年发布，承诺增加 API 稳定性和更好的文档和工具。 Electron v2 于 2018 年发布，并引入了语义版本，使 Electron 开发者更容易跟踪发布周期。

到 Electron v6 时，我们转变为常规的 12 周主要发布节奏，以与 Chromium 相匹配。 这个决定是该项目心态的改变，将“拥有最新的 Chromium 版本”从一种附加功能变成了一项优先考虑的任务。 这降低了版本升级之间的技术债务量，使我们更容易地保持 Electron 的更新和安全。

从那时起，我们就像一台润滑良好的机器一样运转，每次 Chromium 稳定版发布时都会同时发布一个新的 Electron 版本。 到 2021 年，Chromium 加快了发布速度，每 4 周发布一次，而我们也能够轻松地调整发布频率为每 8 周。

现在我们已经更新到了 Electron v23 (并且还在继续更新)，仍致力于构建最佳的跨平台桌面应用程序运行时。 尽管近年来 JavaScript 开发工具繁荣发展，但 Electron 仍然是桌面应用程序框架领域中一个稳定、经受过实战考验的重要支柱。 如今，Electron 应用程序已经无处不在：您可以使用 Visual Studio Code 进行编程，使用 Figma 进行设计，使用 Slack 进行通信，并使用 Notion 进行笔记记录 (还有许多其他用例)。 我们对这一成就感到非常自豪，并感谢使之成为可能的每一个人。

## 在这个过程中，我们学到了什么？

这十年的历程曲折而漫长。 以下是一些关键因素，帮助我们运营一个可持续的大型开源项目。

### 通过治理模型扩展分布式决策制定。

我们不得不克服的一个挑战是，在 Electron 初次爆发出人气后，如何处理项目的长期发展方向。 我们如何处理一个由几十名工程师组成、分布在不同公司、不同国家和不同时区的团队？

在早期，Electron 的维护者团队依靠非正式的协调方式，这对于较小的项目来说是快速和轻量级的，但不适用于更广泛的协作。 在 2019 年，我们转向了一种治理模型，不同的工作组拥有正式的责任领域。 这对于简化流程和将项目所有权的部分分配给特定的维护者非常重要。 现在每个工作组 (WG) 负责什么？

- 将 Electron 发布上线 (Releases WG)
- 升级 Chromium 和 Node.js (Upgrades WG)
- 监督公共 API 设计 (API WG)
- 保持 Electron 的安全性 (Security WG)
- 运营网站、文档和工具 (Ecosystem WG)
- 社区与企业外联 (Outreach WG)
- 社区管理 (Community & Safety WG)
- 维护我们的构建基础设施，维护者工具和云服务 (Infrastructure WG)

在我们转向治理模式的同时，我们还将 Electron 的所有权从 GitHub 转移到了 [OpenJS Foundation](https://www.electronjs.org/blog/electron-joins-openjsf) 虽然最初的核心团队今天仍在微软工作，但他们只是形成 Electron 治理的更大合作伙伴群体的一部分。

虽然这种模式并不完美，但它在全球流行病和持续的宏观经济逆风中为我们提供了良好的支持。 今后，我们计划修订治理章程，以指导我们走向 Electron 的第二个十年。

:::info
如果您想了解更多，请查看 [electron/governance](https://github.com/electron/governance) 代码仓库！
:::

### 社区

开源社区的组织是很困难的，特别是当你的外联团队是一群穿着“社区经理”外套的十几个工程师时。 话虽如此，成为一个大型的开源项目意味着我们拥有大量的用户，利用他们的能量来为 Electron 建立用户生态系统是维持项目健康的关键部分。

我们为发展社区做了哪些工作？

#### 建立社区

- 2020 年，我们推出了我们在 Discord server 的社区。 我们之前在 Atom 的论坛上有一个版块，但决定有一个更非正式的消息传递平台，让维护者和 Electron 开发人员之间有一个讨论的空间，并获得一般的调试帮助。
- 2021 年，我们在 [@BlackHole1](https://github.com/BlackHole1) 的帮助下建立了 [Electron China](https://github.com/electronjs-cn) 用户群。 这个群体在中国蓬勃发展的科技界中对 Electron 用户的增长起了重要作用，为他们在我们的英语语言的范畴之外提供了一个创意合作、讨论 Electron 的空间。 我们还要感谢 [cnpm](https://npmmirror.com/) 为支持 Electron 在他们的中文镜像中为 npm 所做的工作。

#### 参与高知名度的开源项目​

- 自 2019 年以来，我们每年都会庆祝 [Hacktoberest](https://hacktoberfest.com/)。 Hacktoberfest 是由 DigitalOcean 组织的每年一度的开源庆典，每年我们都会有数十名热情的贡献者加入其中，希望在开源软件上留下自己的印记。
- 在 2020 年，我们参加了首次举行的 Google Season of Docs 活动，与 [@bandantonio](https://github.com/bandantonio) 合作重新设计了 Electron 的新用户教程流程。
- 2022 年，我们第一次指导 Google Summer of Code 的学生。 [@aryanshridhar](https://github.com/aryanshridhar) 做了一些了不起的工作，重构了 [Electron Fiddle](https://github.com/electron/fiddle) 的核心版本加载逻辑，并将其捆绑包迁移到了 [webpack](https://webpack.js.org/)。

### 将所有东西自动化！

今天，Electron 的维护有大约 30 名积极的维护者。 我们中只有不到一半的人是全职贡献者，这意味着还有很多工作需要做。我们怎么才能让一切顺利进行呢？ 我们的座右铭是计算机便宜，人的时间是昂贵的。按照典型的工程师风格，我们开发了一套自动化支持工具来使我们的生活更轻松。

#### Not Goma​

Electron 的核心代码库是一个庞大的 C++ 代码库，而编译时间一直是限制我们能够快速发布错误修复和新功能的因素之一。在 2020 年，我们部署了 [Not Goma](https://www.electronjs.org/docs/latest/development/goma)，这是一个专门为 Electron 定制的后端，用于 Google 的 [Goma](https://chromium.googlesource.com/infra/goma/client/) 分布式编译器服务。Not Goma 处理来自授权用户机器的编译请求，并在后端的数百个核心上进行分布式编译。 它还会缓存编译结果，这样编译相同文件的其他人只需下载预编译的结果即可。

自从启用 Not Goma 以来，维护者的编译时间已经从几个小时缩短到几分钟的级别。拥有稳定的互联网连接成为贡献者编译 Electron 的最低要求！

:::info

如果你是一个开源贡献者，你也可以尝试 Not Goma 的只读缓存，它默认在 [Electron Build Tools](https://github.com/electron/build-tools) 中提供。

:::

#### Continuous Factor Authentication

[Continuous Factor Authentication (CFA)](http://continuousauth.dev/) 是 npm 双因素认证（2FA）系统的自动化层，我们将其与语义化版本控制工具(semantic-release)结合使用来管理我们各种 `@electron/` npm 包的安全和自动化发布。

虽然语义化版本控制工具(semantic-release)已经自动化了 npm 包的发布过程，但它需要关闭两步验证或传递绕过此限制的秘密令牌。

我们构建了CFA 来为 npm 2FA 提供基于时间的一次性密码 (TOTP)，以供任意 CI 任务使用，从而让我们利用语义化版本控制工具(semantic-release)的自动化功能，同时保持双重身份验证的额外安全性。

我们使用 CFA 与 Slack 集成前端，允许维护人员在任何拥有 Slack 的设备上验证软件包发布，只要他们手头有 TOTP 生成器即可。

:::info
如果您想在自己的项目中尝试 CFA，请查看 [GitHub存储库](https://github.com/continuousauth/web) 或 [文档](https://docs.continuousauth.dev/)！ 如果您使用 CircleCI 作为 CI 提供程序，我们还有 [一个方便的 orb](https://github.com/continuousauth/npm-orb)，可以快速搭建一个带有 CFA 的项目。
:::

#### Sheriff

[Sheriff](https://github.com/electron/sheriff) 是我们编写的一个开源工具，用于自动化管理 GitHub、Slack 和 Google Workspace 等平台上的权限。

Sheriff 的主要价值主张是权限管理应该是一个透明的进程。它使用一个单独的 YAML 配置文件，指定了所有上述服务的权限。使用 Sheriff，获得对仓库的协作者身份或创建新的邮件列表就像获得 PR 批准并合并一样容易。

Sheriff 还具有审计日志，会发布到 Slack，当 Electron 组织中的任何地方发生可疑活动时，会向管理员发出警告。

#### …和我们所有的 GitHub 机器人

GitHub 是一个具有丰富 API 可扩展性的平台，并且拥有一个称为 [Probot](https://probot.github.io/) 的官方机器人应用程序框架。 为了帮助我们专注于工作中更有创意的部分，我们开发了一套更小型的机器人，以帮助我们完成一些繁琐的工作。 以下是一些例子：

- [Sudowoodo](https://github.com/apps/sudowoodo-release-bot) 可以自动化 Electron 的发布流程，从启动构建到上传发布资产至 GitHub 和 npm，全部自动化完成。
- [Trop](https://github.com/electron/trop) 可以根据 GitHub PR 标签，在先前的发行分支上尝试挑选补丁，从而自动化 Electron 的回溯过程。
- [Roller](https://github.com/electron/roller) 可以自动化 Electron 的 Chromium 和 Node.js 依赖项的滚动升级过程。
- [Cation](https://github.com/electron/cation) 是我们针对 electron/electron PR 的状态检查机器人。

总的来说，我们的这些小机器人为我们的开发者生产力带来了巨大的提升！

#### 下一步是什么？

在我们进入项目的第二个十年时，您可能会问：Electron接下来会发生什么？

我们将与 Chromium 的发布节奏保持同步，每 8 周发布 Electron 的新主要版本，使该框架与 Web 平台和 Node.js 的最新技术保持同步，同时为企业级应用程序保持稳定性和安全性。

通常，我们会在计划即将实现时公布相关消息。 如果您想紧跟未来版本、功能和项目更新的最新动态，可以阅读 [我们的博客](https://electronjs.org/blog) 并关注我们的社交媒体账户 ([Twitter](https://twitter.com/electronjs) 和 [Mastodon](https://social.lfx.dev/@electronjs))！

[^1]: 这实际上是来自 [electron-archive/brightray 项目](https://github.com/electron-archive/brightray)的第一次提交，该项目在 2017 年被整合到了 Electron 中，并合并了其 Git 历史记录。 但谁在乎呢？ 今天是我们的生日，所以我们可以制定规则！