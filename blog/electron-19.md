---
title: Electron 19.0.0
date: 2022-05-24T00:00:00.000Z
authors:
  - 
    name: VerteDinde
    url: 'https://github.com/VerteDinde'
    image_url: 'https://github.com/VerteDinde.png?size=96'
  - 
    name: ckerr
    url: 'https://github.com/ckerr'
    image_url: 'https://github.com/ckerr.png?size=96'
slug: electron-19-0
---

Electron 19.0.0 已发布！ 它包括升级到 Chromium `102`, V8 `10.2`, 和 Node.js `16.14.2`。 请阅读下文了解更多详情！

---

Electron 团队很高兴发布了 Electron 19.0.0！ 您可以通过 `npm install electron@later` 进行安装，或者从我们的 [发布网站](https://www.electronjs.org/releases/stable) 下载它。 继续阅读有关此版本的详细信息，并请分享您的任何反馈！

## 重要变化

### Electron 发布时间变更

该项目正在恢复其支持最新三个主要版本的早期政策。 [查看我们的版本文档](https://www.electronjs.org/docs/latest/tutorial/electron-versioning) 以了解更多关于 Electron 版本控制和支持的详细信息。 暂时有四个主要版本，以帮助用户适应从 Electron 15 开始的新版本发布节奏。 您可以在此处阅读 [完整详细信息](https://www.electronjs.org/blog/8-week-cadence)。

### 更改栈

* Chromium `102`
    * [Chrome 102 新特性](https://developer.chrome.com/blog/new-in-chrome-102/)
    * [Chrome 101 新特性](https://developer.chrome.com/blog/new-in-chrome-101/)
* Node.js `16.14.2`
    * [Node 16.14.2 博文](https://nodejs.org/en/blog/release/v16.14.2/)
* V8 `10.2`

## 破坏性 & API 更改

以下是 Electron 19 中引入的破坏性变化。 有关这些和未来变化的更多信息可在 [计划的破坏性变化](https://www.electronjs.org/docs/latest/breaking-changes) 页面找到。

### 在 Linux 上不受支持： `.skipTaskbar`

BrowserWindow constructor 选项 `skipTaskbar` 在 Linux 上不再支持。 在 [#33226 中更改](https://github.com/electron/electron/pull/33226)

### 已移除 WebPreferences.preloadURL

半文档化的 `preloadURL` 属性已从 Web 首选项中删除。 [#33228](https://github.com/electron/electron/pull/33228). 使用 `WebPreferences.preload` 代替。

## 终止对 15.x.y 和 16.x.y 的支持

Electron 14.x.y 和 15.x.y 都已终止支持。 [该博客文章](https://www.electronjs.org/blog/8-week-cadence/#-will-electron-extend-the-number-of-supported-versions) 使 Electron 恢复到其支持最新三个主要版本的 [ 早期政策](https://www.electronjs.org/docs/latest/tutorial/electron-timelines#version-support-policy) 。 鼓励开发人员和应用程序升级到较新版本的 Electron。

| E15 (9月21日) | E16 (11月21日) | E17 (2月22日) | E18 (3月22日) | E19 (5月22日) |
| ----------- | ------------ | ----------- | ----------- | ----------- |
| 15.x.y      | 16.x.y       | 17.x.y      | 18.x.y      | 19.x.y      |
| 14.x.y      | 15.x.y       | 16.x.y      | 17.x.y      | 18.x.y      |
| 13.x.y      | 14.x.y       | 15.x.y      | 16.x.y      | 17.x.y      |
| 12.x.y      | 13.x.y       | 14.x.y      | 15.x.y      | --          |
## 下一步做什么

在短期内，您可以期望该团队继续专注于跟上构成 Electron 的主要组件的开发，包括 Chromium，Node 和 V8。 虽然我们谨慎地不对发布日期做出承诺，但我们的计划是大约每2个月发布一次带有新版本组件的主要版本的 Electron。

您可以在此处找到 [Electron 的公开时间表](https://www.electronjs.org/docs/latest/tutorial/electron-timelines)。

有关这些和未来变化的更多信息可在 [计划的破坏性变化](https://github.com/electron/electron/blob/main/docs/breaking-changes.md) 页面找到。
