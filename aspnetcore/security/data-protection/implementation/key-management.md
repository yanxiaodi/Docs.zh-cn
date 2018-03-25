---
title: "密钥管理"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护密钥管理 Api 的实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: a1fd7c55ec94d5def569bb407c064f4fd2fe9695
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="key-management"></a>密钥管理

<a name="data-protection-implementation-key-management"></a>

数据保护系统自动管理的主要密钥用于保护和取消保护负载的生存期。 每个密钥可以中四个阶段之一存在：

* 创建密钥存在于密钥环，但尚未激活。 密钥不应用于新保护操作直到足够的时间已过的密钥已有机会传播到使用此密钥环的所有计算机。

* 活动-密钥存在于密钥环，应使用对所有新的保护操作。

* 过期的密钥已运行其自然的生存期和应不再用于新的保护操作。

* 吊销-密钥遭到破坏，并且不必须用于新的保护操作。

创建、 活动和过期密钥可能都用来取消保护传入负载。 默认情况下的吊销的密钥不可能用于取消保护的负载，但应用程序开发人员可以[重写此行为](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect)如有必要。

>[!WARNING]
> 开发人员可能想要从密钥链中删除密钥，（例如，通过从文件系统中删除相应的文件）。 此时，保护的密钥的所有数据都都永久密匙，且都没有紧急替代具有吊销键可能没有。 删除键是真正破坏性的行为，并因此数据保护系统公开没有第一类 API 执行此操作。

## <a name="default-key-selection"></a>默认密钥选择

当数据保护系统从后备存储库读取密钥环时，它将尝试查找密钥环的"默认"密钥。 默认密钥用于新的保护操作。

常规的启发式方法是数据保护系统选择最新的激活日期与默认密钥的密钥。 （没有小奶油因素，以允许服务器到服务器时钟偏差。）如果密钥已过期或被吊销，并且如果应用程序禁用自动密钥生成，则将生成新的密钥与每个即时激活[密钥过期和滚动](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)下面的策略。

数据保护系统的原因会立即生成新密钥，而不是回退到不同的密钥是生成新密钥应视为已激活新密钥之前的所有键的隐式过期时间。 大致了解是，新密钥可能已配置了不同的算法或静态加密机制比旧的密钥，并且系统应首选的当前配置，而回退。

没有异常。 如果应用程序开发人员可以[禁用自动密钥生成](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)，然后数据保护系统必须选择内容的默认密钥。 在此回退方案中，系统将选择具有最新的激活日期、 非吊销键，而且会优先有时间传播到群集中其他计算机的密钥。 回调系统可以结束因此选择过期的默认密钥。 回调系统将永远不会选择将吊销的键用作默认密钥，并将如果密钥环为空或已被吊销的每个键通过然后系统将产生在初始化时出错。

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>密钥的过期和滚动

创建密钥时，它自动具有提供的激活日期为 {now + 2 天} 和 {now + 90 天} 的到期日期。 之前激活给予密钥的时间才能传遍整个系统的 2 天延迟。 也就是说，它允许指向的后备存储在其他应用程序密钥观察在其下一步的自动刷新期内，从而最大化，密钥环执行成为的活动已传播到所有应用程序，可能需要使用它的可能性。

如果默认密钥将在 2 天内过期，并且密钥环还没有密钥将处于活动状态的默认密钥到期后，数据保护系统将自动保留密钥环的新键。 此新的密钥都有 {默认密钥的过期日期} 的激活日期和到期日期的 {now + 90 天}。 这允许系统自动的服务不会发生中断定期轮转密钥。

可能有情况下将使用即时激活中创建密钥。 一个示例是当应用程序尚未运行的时间和过期密钥链中的所有键。 当发生这种情况时，系统将为密钥提供 {现在} 的不正常的 2 天激活延迟的情况下的激活日期。

默认密钥生存期是 90 天，但这是可配置，如以下示例所示。

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

管理员还可以更改默认系统范围内，但显式调用`SetDefaultKeyLifetime`将覆盖任何系统范围的策略。 默认密钥生存期不能短于 7 天。

## <a name="automatic-key-ring-refresh"></a>自动密钥环刷新

数据保护系统初始化时，它从基础存储库读取密钥环，并将其缓存在内存中。 此缓存允许未命中的后备存储的情况下继续保护和取消保护操作。 大约每隔 24 小时或当前的默认密钥过期，不管先满足后，系统会自动查看更改的后备存储。

>[!WARNING]
> 开发人员应极少数情况下 （如果有） 需要直接使用密钥管理 Api。 数据保护系统将执行自动密钥管理，如上面所述。

数据保护系统公开接口`IKeyManager`可用来检查和更改的密钥链。 DI 系统提供的实例`IDataProtectionProvider`还可以提供的实例`IKeyManager`供你使用。 或者，你可以请求`IKeyManager`直接从`IServiceProvider`以下示例所示。

修改键环 （显式创建一个新密钥或执行吊销） 的任何操作将使内存中缓存失效。 下次调用`Protect`或`Unprotect`将导致数据保护系统读取密钥环，并重新创建缓存。

下面的示例演示如何使用`IKeyManager`接口来检查和操作密钥环，包括撤销的现有密钥和手动生成新密钥。

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>密钥存储

数据保护系统具有，由此它将尝试自动推导出适当的密钥存储位置和 rest 机制加密启发式方法。 这也是由应用程序开发人员可配置的。 以下文档讨论这些机制的现成实现：

* [内置密钥存储提供程序](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [框 rest 服务提供商的密钥加密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
