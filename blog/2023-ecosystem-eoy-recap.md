回顾 2023 年 Electron 开发者生态系统的改进和变化。

---

过去几个月里，我们在整个 Electron 生态系统中进行一些优化，以提升 Electron 应用程序的开发者体验！ 这是 Electron HQ 最新增加的项目的快速概述。 

## Electron Forge 7 及其以后

Electron Forge 7 — 用于打包和分发 Electron 应用程序的一体化工具的最新主要版本 — 现已发布。

虽然 Forge 6 与 v5 是完全重写的，但 v7 的范围较小，但仍包含一些重大变更。 未来，我们将继续发布 Forge 的主要版本，以便进行必要的重大变更。

欲了解更多详情，请参阅 GitHub 上的完整描述 [Forge v7.0.0 更新日志](https://github.com/electron/forge/releases/tag/v7.0.0.0)。 

### 重大变更

- **切换 macOS notarization 工具到 `notarytool`：** 截至2023-11-01。 苹果废弃了 macOS notarization 的传统工具 `altool`，此次发布将其从 Electron Forge 中完全删除。
- **最小 Node.js 支持增加到 v16.4.0:** 在这个版本中，我们已将所需版本的 Node.js 设置为 16.4.0。
- **废弃了对 `electron-prebuild` 和 `electron-prebuild-compile` 的支持：** `electron-prebuild` [是 electron npm 模块的原始名称](https://www.electronjs.org/blog/npm-install-electron-electron)的支持，但在 v1.3.1 中 被 `electron` 代替了。`electron-prebuild-compile` 是一个带有增强开发体验功能的二进制文件的替代方案，但最终作为一个项目被放弃了。

### 重点内容

- **[Google Cloud Storage 发布器](https://github.com/electron/forge/pull/2100):** 作为我们推动更好地支持静态自动更新的一部分，Electron Forge 现在支持直接发布到 Google Cloud Storage！
- **[ESM forge.config.js 支持](https://github.com/electron/forge/pull/3358):** Electron Forge 现在支持 ESM `forge.config.js` 文件。（附言：期待在 Electron 28 中支持 ESM entrypoint。）
- **[Makers 现在可以并行运行](https://github.com/electron/forge/pull/3363)：** 在 Electron Forge 6 中 Makers 由于 ✨ 遗产 ✨ 原因而顺序运行。从那时起，我们已经测试了使用并行 Make 步骤的效果，并且没有出现任何负面影响。 当在同一平台上构建多个目标时，你应该会看到加速效果！

:::info 非常感谢！

🙇 对于 **[mahnunchik](https://github.com/mahnunchik)** 为 GCS Publisher 和 Forge 配置中支持 ESM 的贡献，表示衷心感谢！

:::

## 静态存储自动更新的改进

Squirel.Windows 和 Squirrel.Mac 是支持 Electron 的内置 `autoUpdater` 模块的特定平台更新技术。两个项目都支持通过两种方法自动更新：

- 一个与 Squirrel 兼容的更新服务器
- 在静态存储提供商上托管的清单 URL (例如 AWS、谷歌云平台、微软 Azure 等)

传统上，更新服务器方法一直被认为是 Electron 应用的推荐方法（并提供了额外的更新逻辑自定义），但它也有一个主要的缺点 — 如果应用是闭源的，它需要维护自己的服务器实例。

另一方面，虽然静态存储方法一直是可行的，但在 Electron 内部没有文档记录，并且在 Electron 工具包中支持不足。

通过 @MarshallOfSound 的杰出工作，无服务器自动应用程序更新已经得到了显著简化：

- Electron Forge 的 Zip 和 Squirrel.Windows 制作工具现在可以配置为输出与 `autoUpdater` 兼容的更新清单。
- 现在，`update-electron-app` 的一个新的主要版本（v2.0.0）可以读取这些生成的清单，作为 [update.electronjs.org](https://update.electronjs.org) 服务器的替代方案。

一旦你的 Makers 和 Publishers 配置好了并上传更新清单到云文件存储，您只需要几行配置代码就可以启用自动更新。

```jsx
const { updateElectronApp, UpdateSourceType } = require('update-electron-app');

updateElectronApp({
  updateSource: {
    type: UpdateSourceType.StaticStorage,
    baseUrl: `https://my-manifest.url/${process.platform}/${process.arch}`,
  },
});
```

:::info 延伸阅读

📦 想要了解更多？详细指南见 [Forge 的自动更新文档](https://www.electronforge.io/advanced/auto-update)。

:::

## `@electron/` 扩展宇宙

Electron 刚开始时，社区发布了许多软件包，以增强开发、包装和分发 Electron 应用的体验。随着时间的推移，其中许多软件包都被纳入 Electron 的 GitHub 组织，核心团队承担了维护负担。

在2022年，我们开始将所有这些 first-party 工具整合到 npm 的 `@electron/` 命名空间下。此变更意味着以前被称为 `electron-foo` 的软件包现在在 npm上被称为 `@electron/foo`，而以前命名为 `electron/electron-foo` 的仓库现在在 GitHub 上被称为 `electron/foo`。这些变化有助于明确划分 first-party 项目和 userland 项目。这包括许多常用的软件包，例如：

- `@electron/asar`
- `@electron/fuses`
- `@electron/get`
- `@electron/notarize`
- `@electron/osx-sign`
- `@electron/packager`
- `@electron/rebuild`
- `@electron/remote`
- `@electron/symbolicate-mac`
- `@electron/universal`

从现在开始，我们发布的所有 first-party 软件包都将在 `@electron/` 命名空间中。这条规则有三个例外情况：

- Electron 核心将继续在 `electron` 软件包下发布。
- Electron Forge 将继续在 `@electron-forge/` namespace 下发布其所有的 monorepo 软件包。

:::info 寻找 Star

⭐ 在这个过程中，我们意外地将 electron/packager 仓库设为私有，这不幸地导致我们的 GitHub 星星计数被删除（擦除之前超过 9000 颗）。如果你是 Packager 的活跃用户，我们将非常感谢你的 ⭐ **Star** ⭐ ！

:::

## 介绍 `@electron/windows-sign`

从 2023 年 6 月 1日开始，行业标准要求 Windows 代码签名证书的密钥必须存储在符合 FIPS 标准的硬件上。

在实践中，这意味着对于在 CI 环境中构建和签名的应用来说，代码签名变得更加困难，因为许多 Electron 工具使用证书文件和密码作为配置参数，并尝试使用硬编码逻辑从中进行签名。

这种情况对于 Electron 开发人员来说是一个常见的痛点，这就是为什么我们一直在努力寻找一个更好的解决方案，将 Windows 代码签名独立到一个单独的步骤中，类似于 `@electron/osx-sign` 在 macOS 上的做法。

在未来，我们计划将此软件包完全整合到 Electron Forge 工具链中，但目前它独立存在。该软件包目前可通过 `npm install --save-dev @electron/windows-sign` 进行安装，并可通过编程或命令行界面（CLI）进行使用。

请尝试在[项目的 issue 跟踪器](https://github.com/electron/windows-sign/issues)中给我们反馈！

## 下一步

我们将在下个月进入我们每年 12 月安静的时期。在这样做的同时，我们将考虑如何在 2024 年使 Electron 开发体验更好。

你是否希望看到我们接下来的工作？请告诉我们！
