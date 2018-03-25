---
title: "限制受保护的负载的生存期"
author: rick-anderson
description: "本文档说明如何限制使用 ASP.NET Core 数据保护 Api 的受保护负载的生存期。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: d631851b5b933d75c37a308f492840e3442e6f1a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a>限制受保护的负载的生存期

有一些应用程序开发人员希望创建将在一段时间后过期的受保护的负载的情形。 例如，受保护的负载可能表示只应为有效一小时的密码重置令牌。 当然也可以为开发人员创建他们自己包含的嵌入的到期日期的负载格式和高级开发人员可能想要执行此操作，但对于开发人员的大多数管理这些过期时间可能变得需要很长时间。

若要简化此过程对于我们的开发人员受众，包[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)用于创建在一段时间后自动过期的负载包含实用工具 Api。 这些 Api 的挂起注销`ITimeLimitedDataProtector`类型。

## <a name="api-usage"></a>API 使用情况

`ITimeLimitedDataProtector`接口是用于保护和取消保护的限时 / 自即将到期的负载的核心接口。 若要创建的实例`ITimeLimitedDataProtector`，你需要先正则表达式的实例[IDataProtector](overview.md)构造具有特定的用途。 一次`IDataProtector`实例可用时，调用`IDataProtector.ToTimeLimitedDataProtector`扩展方法，能获得重新保护程序内置到期功能。

`ITimeLimitedDataProtector` 公开以下 API 图面和扩展方法：

* CreateProtector （字符串目的）： ITimeLimitedDataProtector-此 API 已类似于现有`IDataProtectionProvider.CreateProtector`在于它可以用于创建[目的链](purpose-strings.md)从根的限时保护程序。

* Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]

* 保护 （字节 [] 纯文本、 TimeSpan 生存期）： byte]

* 保护 （字节 [] 纯文本）： byte]

* 保护 （字符串纯文本，DateTimeOffset 到期）： 字符串

* 保护 (TimeSpan 生存期中的字符串纯文本）： 字符串

* 保护 （字符串纯文本）： 字符串

除了核心`Protect`方法需要仅是纯文本形式，存在一些新重载允许指定的负载的到期日期。 到期日期可以指定为绝对日期 (通过`DateTimeOffset`) 或相对的时间 (从当前系统时间，通过`TimeSpan`)。 如果调用不带过期的重载，则负载假定为永远不会过期。

* 取消保护 (字节 [] protectedData，出 DateTimeOffset 到期): byte]

* 取消保护 (字节 [] protectedData): byte]

* 取消保护 (出 DateTimeOffset 过期字符串 protectedData): 字符串

* 取消保护 (字符串 protectedData): 字符串

`Unprotect`方法返回原始的未受保护的数据。 如果尚未超过负载，绝对过期则返回为可选输出参数以及原始的未受保护数据。 如果负载已过期，则取消保护方法的所有重载将都引发 CryptographicException。

>[!WARNING]
> 它具有不建议使用这些 Api 来保护需要长期或无限期暂留的负载。 "，我可以承受为要在一个月后永久性不可恢复的受保护负载？" 可用作一个很好的经验法则;如果问题的回答是没有然后开发人员应考虑备用 Api。

使用下面的示例[非 DI 代码路径](../configuration/non-di-scenarios.md)用于实例化数据保护系统的。 若要运行此示例，请确保先添加对 Microsoft.AspNetCore.DataProtection.Extensions 包的引用。

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
