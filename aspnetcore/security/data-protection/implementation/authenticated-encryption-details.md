---
title: "经过身份验证的加密详细信息"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护的实现详细信息进行加密身份验证。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: b58f36a5f0353da69d6f1ef4db542aba8267027a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="authenticated-encryption-details"></a>经过身份验证的加密详细信息

<a name="data-protection-implementation-authenticated-encryption-details"></a>

对 IDataProtector.Protect 调用都是经过身份验证的加密操作。 保护方法提供保密性和真实性，并且它会绑定到用于此特定 IDataProtector 实例派生其根 IDataProtectionProvider 目的链。

IDataProtector.Protect 采用 byte [] 纯文本参数，并且生成的字节 [] 受保护的负载，其格式为下面所述。 （此外还有一个扩展方法重载以采用字符串的纯文本参数和返回的字符串受保护的负载。 如果使用此 API 的受保护的负载格式仍将下面结构，但它会[base64url 编码](https://tools.ietf.org/html/rfc4648#section-5)。)

## <a name="protected-payload-format"></a>受保护的负载格式

受保护的负载格式包含三个主要组件：

* 32 位神奇标头，它标识数据保护系统的版本。

* 128 位密钥 id 标识该密钥用于保护此特定的负载。

* 受保护的负载的剩余部分[特定于加密程序按此键封装](subkeyderivation.md#data-protection-implementation-subkey-derivation)。 在下面的示例密钥表示 AES 256 CBC + HMACSHA256 加密器和负载进一步细分，如下所示: * 一个 128 位密钥修饰符。 * 一个 128 位初始化向量。 * AES 256 CBC 输出 48 个字节。 * 一个 HMACSHA256 身份验证标记。

示例受保护的负载如下所示。

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

从上面的第一个 32 位或 4 个字节的负载格式是标识版本 (09 F0 C9 F0) 的幻标头

下一步的 128 位或 16 个字节是密钥标识符 (80 9 C 81 0 C 19 66 19 40 95 36 53 F8 AA FF EE 57)

其余部分包含负载，并特定于所使用的格式。

>[!WARNING]
> 给定的键对受保护的所有负载将都开始与同一个 20 字节 （神奇的值，密钥 id） 标头。 管理员可以使用这一事实以进行诊断来估计生成负载时间。 例如，上面的负载对应于键 {0c819c80-6619-4019-9536-53f8aaffee57}。 如果检查密钥的存储库后找到验证此特定密钥激活日期为 2015年-01-01 和其到期日期为 2015年-03-01，然后是合乎情理假定负载 （如果未被篡改） 在该窗口中，给予生成或采用一个较小在任意一侧奶油因子。
