---
title: "正在取消保护已吊销其密钥的有效负载"
author: rick-anderson
description: "本文档说明如何取消保护数据，使用密钥，因为已吊销，在 ASP.NET Core 应用程序保护。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 37332dda794f898fb866424b38394f5d4441e166
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>正在取消保护已吊销其密钥的有效负载

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

ASP.NET 核心数据保护 Api 主要不用于机密负载的无限期持久性。 其他技术喜欢[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)和[Azure Rights Management](https://docs.microsoft.com/rights-management/)更适合于以下场景： 无限期存储，并且它们的相应强密钥管理功能。 也就是说，无需进行任何开发人员禁止使用 ASP.NET Core 数据保护 Api 进行长期保护的机密数据。 密钥绝不会移除从密钥链中，因此`IDataProtector.Unprotect`，只要键为可用，有效始终可以恢复现有的负载。

但是，则会产生问题在开发人员尝试取消保护数据作为保护与吊销键，`IDataProtector.Unprotect`在这种情况下将引发异常。 这些类型的负载可以轻松地重新创建这些系统，以及在坏的情况下站点访问者可能需要重新登录，这可能是特别适用于短期或临时负载 （如身份验证令牌） 的。 但持久化负载，具有`Unprotect`抛出可能导致不可接受数据丢失。

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

若要支持允许负载为即使在遇到吊销密钥时未受保护的方案，数据保护系统包含`IPersistedDataProtector`类型。 若要获取其实例`IPersistedDataProtector`，只需获取其实例`IDataProtector`在正常情况和重试强制转换`IDataProtector`到`IPersistedDataProtector`。

> [!NOTE]
> 并非所有`IDataProtector`实例可以强制转换为`IPersistedDataProtector`。 开发人员应使用 C# 作为运算符或类似以避免运行时异常导致通过无效强制转换，并且应在准备好正确地处理失败案例。

`IPersistedDataProtector` 公开以下 API 图面：

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

此 API 采用 （作为字节数组） 的受保护的负载，并返回未受保护的负载。 没有任何基于字符串的重载。 Out 参数的两个如下所示。

* `requiresMigration`： 将设置为 true 时用于保护此负载密钥不再是活动默认密钥，例如，用于保护此负载的关键是旧，密钥滚动操作包含以来执行的位置。 调用方可能想要考虑重新保护具体取决于其业务需求的负载。

* `wasRevoked`： 将设置为 true 时用来保护此负载的密钥已被吊销。

>[!WARNING]
> 传递时应格外谨慎`ignoreRevocationErrors: true`到`DangerousUnprotect`方法。 如果调用此方法后的`wasRevoked`值是为 true，则用来保护此负载的密钥已被吊销，并且负载的真实性应被视为可疑。 在这种情况下，才继续操作操作系统上不受保护的负载，如果你具有一些单独的保障，它是可信的例如所来自的安全数据库，而不是由不受信任的 web 客户端发送。

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
