今天早些时候，Electron 团队被告知，最近有几个公开的 CVEs 针对几个著名的 Electron 应用程序进行了提交。 这些 CVE 与 Electron 的两个 [fuses（保险丝）](https://www.electronjs.org/docs/latest/tutorial/fuses) - `runAsNode` 和 `enableNodeCliInspectArguments` - 相关，并错误地声称，如果这些组件没有被积极禁用，远程攻击者能够通过这些组件执行任意代码。

我们认为这些 CVE 并非出于善意提交。 首先，这个声明是不正确的 - 配置并不允许远程代码执行。 其次，尽管拥有漏洞赏金计划，但在这些 CVE 中被点名的公司并没有收到通知。 最后，虽然我们确实相信禁用相关组件可以增强应用程序的安全性，但我们认为这些 CVE 的提交并没有准确反映其严重性。 “Critical”级别是为最高危险的问题保留的，显然这里的情况并非如此。

任何人都可以申请 CVE。 虽然这对软件行业的整体健康有益，但是“挖掘 CVEs” 对增强单个安全研究员声誉并没有帮助。

话虽如此，我们理解仅仅是一个带有可怕的 `critical` 严重性的 CVE 的存在可能会导致用户感到困惑，因此作为一个项目，我们希望提供指导和协助来处理这个问题。

### 这会给我带来什么影响？

在审查了 CVEs 之后，Electron 团队认为这些 CVEs 并不是严重级别的。

攻击者需要已经能够在机器上执行任意命令，无论是通过对硬件的物理访问还是通过实现完全的远程代码执行。 这值得重申：所描述的漏洞_要求攻击者已经能够访问被攻击的系统_。

例如，Chrome [在其威胁模型中不考虑本地物理攻击](https://chromium.googlesource.com/chromium/src/+/master/docs/security/faq.md#Why-arent-physically_local-attacks-in-Chromes-threat-model)：

> 我们认为这些攻击超出了 Chrome 的威胁模型范围，因为对于已经能够以你的身份登录你的设备或者能够以你的操作系统用户账户的权限运行软件的恶意用户，Chrome（或任何应用程序）无法进行防御。 这样的攻击者可以修改可执行文件和DLL文件，更改像 `PATH` 这样的环境变量，更改配置文件，读取你的用户账户所拥有的任何数据，将其通过电子邮件发送给自己等等。 这样的攻击者对你的设备拥有完全的控制权，Chrome 所能做的任何事情都无法提供严格的防御保证。 这个问题不仅仅存在于Chrome - 所有应用程序都必须信任本地物理用户。

利用 CVE 中描述的漏洞，攻击者可以将受影响的应用程序用作具有继承 TCC 权限的通用 Node.js 进程。 因此，如果应用程序例如已被授权访问地址簿，攻击者可以将应用程序作为 Node.js 运行，并执行将继承该地址簿访问权限的任意代码。 这通常被称为“[寄生式攻击](https://www.crowdstrike.com/cybersecurity-101/living-off-the-land-attacks-lotl/)”。 攻击者通常使用 PowerShell、Bash 或类似工具来运行任意代码。

### 我是否受到了影响？

默认情况下，所有发布版本的 Electron 都启用了 `runAsNode `和 `enableNodeCliInspectArguments` 功能。 如果您没有按照 [Electron Fuses 文档](https://www.electronjs.org/docs/latest/tutorial/fuses)中描述的方法关闭它们，那么您的应用程序同样容易受到“寄生式攻击”的影响。 再次强调，攻击者需要已经能够在受害者的机器上执行代码和程序。

### 缓解措施

减轻这个问题的最简单方法是在您的 Electron 应用程序中禁用 `runAsNode` fuse。 `runAsNode` fuse 用于切换是否考虑` `ELECTRON_RUN_AS_NODE \` 环境变量。 请参阅 [Electron Fuses 文档](https://www.electronjs.org/docs/latest/tutorial/fuses)以获取有关如何切换这些 fuses 的信息。

请注意，如果禁用了这个 fuse，那么主进程中的 `process.fork` 将无法按预期运行，因为它依赖于这个环境变量来运行。 相反，我们建议您使用[实用进程](https://www.electronjs.org/docs/latest/api/utility-process)，它适用于许多需要独立的 Node.js 进程的用例（例如 Sqlite 服务器进程或类似情况）。

您可以在我们的[安全清单](https://www.electronjs.org/docs/latest/tutorial/security)中找到更多关于我们推荐的 Electron 应用程序的安全最佳实践的信息。
