---
uid: whitepapers/ms03-32-issue
title: "应用安全更新，IE 后的修复不可用服务器应用程序错误 |Microsoft 文档"
author: rick-anderson
description: "本白皮书介绍了为影响 Wi 上运行的 ASP.NET 1.0 应用程序的 Internet 资源管理器与 MS03 32 安全更新解决了问题的修补程序..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 8658e387aeb4ea0340080666906b2b89db49a31a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>应用安全更新的 IE 后为不可用服务器应用程序错误修复
====================
> 本白皮书介绍会影响 Windows XP 专业版上运行的 ASP.NET 1.0 应用程序的 Internet explorer 与 MS03 32 安全更新解决了问题的修补程序。
> 
> 适用于 ASP.NET 1.0 和 Windows XP 专业版。


Microsoft 与 Internet Explorer 的安全修补程序 MS03 32 安全更新和 Windows XP 上运行的 ASP.NET 1.0 标识的问题。 手动或通过从 Windows 更新站点中获取最新的关键更新，则可以安装此修补程序。

此问题的症状就是在一台 Windows XP 计算机上安装此修补程序后, 产生的本地 IIS 5.1 web 服务器上运行的 ASP.NET 应用程序的所有请求中一个表明"服务器应用程序不可用"错误消息。 到远程 web 服务器的请求不会受到影响。

此问题只会影响在 Windows XP 上运行 ASP.NET 1.0 安装。 它不会影响运行 Windows 2000 或 Windows Server 2003 的计算机。 它也不会影响与 ASP.NET 1.1 安装运行 Windows XP 的计算机。

请注意，此问题**不**使用 ASP.NET 安全 bug。 它**不**打开或允许对某个 ASP.NET 应用程序或服务器的任何恶意攻击。 相反，它是纯函数 bug 引起的修补程序本身。

我们正在努力针对此问题的永久解决方案。 在此期间，你可以执行以下的批处理文件作为一个问题的解决方法。 批处理文件执行以下任务：

1. 停止 IIS 和 ASP.NET 状态服务
2. 删除并重新创建具有已知的临时密码的 ASPNET 帐户
3. 使用 Windows`runas`命令以启动创建 ASPNET 用户配置文件的可执行文件
4. 重新注册 ASP.NET。 这将创建新的随机密码的帐户并将应用为其默认 ASP.NET 访问控制设置
5. 重新启动 IIS 服务

批处理文件包含硬编码的临时密码"**1pass@word**"你将提示您输入的 runas 命令运行批处理文件时。 Runas 命令完成后，ASPNET 帐户密码的强随机值使用重新创建。 请注意，如果硬编码密码不符合你的环境中的密码复杂性要求，可能会失败的批处理文件。 如果是这种情况，你可以将其更改为适合于你的环境的另一个值。

*> [!IMPORTANT]*如果你已添加自定义访问控制设置或数据库帐户为 ASPNET 帐户的权限，它们将需要此批处理文件完成后重新创建。 这是因为当该帐户将重新创建，它将获取新的安全标识符 (SID)。

*> [!IMPORTANT]*如果运行的 ASP.NET 工作进程具有自定义在 ASPNET 帐户以外的帐户，则你不应运行此批处理文件。 相反，你应该以交互方式登录或与该帐户将创建该帐户的用户配置文件中使用 runas 命令。

批处理文件包含以下自解压缩的存档中。 若要使用它：

1. 你必须为帐户运行，使用管理员权限
2. [下载并打开自解压可执行文件](ms03-32-issue/_static/fixup1.exe)
3. 将内容提取到 c:\
4. 从开始菜单中，选择运行...和输入`cmd.exe`
5. 在打开命令窗口中，键入`c:\fixup.cmd`。
6. 出现提示时，输入 **1pass@word** 作为密码。
7. 如果你有以前自定义访问控制设置或数据库帐户为 ASPNET 帐户的权限，你将需要重新立即应用这些设置。

许多歉意，这导致歉意。 随着可用，我们将发布的其他信息。

以下表详细说明了平台和版本受到此问题。

| .NET Framework | 平台 | 受影响 |
| --- | --- | --- |
| 版本 1.0 | Windows 2000 Professional | No |
| 版本 1.0 | Windows 2000 Server | No |
| 版本 1.0 | Windows XP Professional | 是 |
| 版本 1.0 | Windows Server 2003 | No |
| 版本 1.0 | 使用 Cassini 的 Windows XP 家庭版 | No |
| 版本 1.1 | Windows 2000 Professional | No |
| 版本 1.1 | Windows 2000 Server | No |
| 版本 1.1 | Windows XP Professional | No |
| 版本 1.1 | Windows Server 2003 | No |
| 版本 1.1 | 使用 Cassini 的 Windows XP 家庭版 | No |

谢谢，   
 ASP.NET 团队
