---
title: "密钥不可变性和更改设置"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护密钥可变性 Api 的实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a>密钥不可变性和更改设置

一旦对象持久保留到后备存储，其表示形式是永久固定的。 新的数据可以添加到后备存储，但可以永远不会更改现有数据。 此行为的主要目的是为了防止数据损坏。

此行为的一个后果是，一旦给后备存储编写了一个键，不可变。 其创建、 激活和到期日期可以永远不会更改，但它可以通过使用吊销`IKeyManager`。 此外，其基础的算法信息、 主密钥材料和加密 rest 属性也是不可变。

如果开发人员更改会影响密钥保持任何设置，这些更改不会生效只有在下次时都会生成一个密钥，是指在通过显式调用`IKeyManager.CreateNewKey`或通过数据保护系统的自己[自动密钥生成](key-management.md#data-protection-implementation-key-management)行为。 影响密钥持久性的设置如下所示：

* [默认密钥生存时间](key-management.md#data-protection-implementation-key-management)

* [在 rest 机制密钥加密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [在项包含的算法信息](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

如果你需要这些设置，以便早于下一步的自动密钥滚动时间启动，请考虑使显式调用`IKeyManager.CreateNewKey`强制创建新的密钥。 请记得提供显式激活日期 （{现在 + 2 天} 是一个很好的经验法则以允许更改传播的时间） 和调用中的到期日期。

>[!TIP]
> 点击存储库中的所有应用程序应指定要用的相同设置`IDataProtectionBuilder`扩展方法。 否则，保存的密钥的属性将取决于调用的密钥生成例程的特定应用程序。
