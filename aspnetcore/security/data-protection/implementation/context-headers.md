---
title: "上下文标头"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护上下文标头的实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: c047c54efdcdb6192e4d38d2822c1077ee0a73e1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="context-headers"></a>上下文标头

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>背景和理论上

在数据保护系统中，"密钥"意味着可以提供的对象进行身份验证加密服务。 每个键由唯一的 id (GUID) 和它所携带算法信息和 entropic 材料。 其用途是，每个密钥执行唯一平均信息量，但系统不能强制实施的而我们还需要考虑的开发人员可能会通过修改密钥链中的现有密钥的算法信息手动更改密钥链。 若要实现提供这种情况下我们安全要求数据保护系统具有的概念[的加密灵活性](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/)，这样，安全地跨多个加密算法使用单个 entropic 值。

大多数系统支持的加密灵活性做到这一点包括有关负载之内算法某些标识信息。 算法的 OID 通常是适合于此。 但是，我们遇到的一个问题是有多种方法来指定相同的算法:"AES"(CNG) 托管 Aes、 AesManaged、 AesCryptoServiceProvider、 AesCng 和 （如果有特定的参数） 的 RijndaelManaged 类都确实是相同首先，，我们将需要维护所有这些到正确的 OID 的映射。 如果开发人员想要提供自定义算法 （或甚至另一个实现的 AES ！），他们将需要告诉我们其 OID。 此额外的注册步骤使系统配置特别棘手。

回顾一下，我们决定我们已从错误方向接近问题。 OID 提示您的算法是什么，但我们不实际关心这。 如果我们需要在两个不同的算法中安全地使用单个 entropic 值，它不是我们要知道算法的实际的所必要的。 我们实际上关心是它们的行为方式。 任何有效的对称块加密算法也是强伪随机排列 (PRP): 修复 （键，链接模式，IV，纯文本） 的输入和已加密文本输出将与急剧的概率为不同于任何其他对称的块密码提供的相同的输入的算法。 同样，任何相当的加密哈希函数也是强伪函数 (PRF)，并且给定的一组固定的输入其输出将极其能不同于任何其他加密哈希函数。

我们使用强 PRPs 和 PRFs 的概念构建上下文标头。 此上下文标头实质上是充当稳定指纹通过对于任何给定的操作，所用的算法，并提供所需的数据保护系统的加密灵活性。 此标头是可重现和更高版本用作的一部分[子项派生过程](subkeyderivation.md#data-protection-implementation-subkey-derivation)。 有两个不同的方法来生成具体的操作模式的基础的算法取决于上下文标头。

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC 模式下加密 + HMAC 身份验证

<a name="data-protection-implementation-context-headers-cbc-components"></a>

上下文标头由以下组件组成：

* [16 位]值 00 00，是一个标记，这意味着"CBC 加密 + HMAC 身份验证"。

* [32 位]对称块加密算法密钥长度 （以字节为单位，big endian）。

* [32 位]块大小 （以字节为单位，big endian） 的对称块加密算法。

* [32 位]HMAC 算法的密钥长度 （以字节为单位，big endian）。 （当前密钥的大小始终与匹配的摘要大小。）

* [32 位]HMAC 算法 （以字节为单位，big endian） 摘要大小。

* EncCBC (K_E、 IV，"")，它是提供空字符串输入对称的块密码算法的输出，并且其中 IV 是全为零的向量。 下面描述了 K_E 的构造。

* MAC (K_H，"")，这是提供空字符串输入 HMAC 算法的输出。 下面描述了 K_H 的构造。

理想情况下，我们无法 K_E 和 K_H 传递全为零的向量。 但是，我们想要避免这种情况，其中基础算法检查存在的共享密钥的安全性执行任何操作 （值得注意的是 DES 和 3DES） 之前，它可以阻止使用如全为零向量的简单或可重复模式。

相反，我们使用 NIST SP800 108 KDF 中计数器模式 (请参阅[NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec。 5.1) 具有零长度键、 标签和上下文和作为基础 PRF HMACSHA512。 我们派生 |K_E |+ |K_H |字节的输出，然后将分解结果为 K_E 和 K_H 本身。 数学上，这表示，如下所示。

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>示例： AES 192 CBC + HMACSHA256

作为示例，请考虑对称块加密算法是 AES 192 CBC，其中的验证算法是 HMACSHA256 这种情况。 系统将生成使用以下步骤的上下文标头。

首先，让 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 192 位和 |K_H |= 每个指定的算法的 256 位。 这将导致 K_E = 5BB6...21DD 和 K_H = A04A...在下面的示例 00A9:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

接下来，计算 Enc_CBC (K_E、 IV，"") 对于给定 IV 的 AES-192-CBC = 0 * 和 K_E 按上面所述。

result := F474B1872B3B53E4721DE19C0841DB6F

接下来，计算 MAC (K_H，"") 为 HMACSHA256 给定 K_H 按上面所述。

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

这将产生下面的完整上下文标头：

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

此上下文标头是经过身份验证的加密算法对 （AES 192 CBC 加密 + HMACSHA256 验证） 的指纹。 组件，如所述[上面](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)是：

* 标记 (00 00)

* 块密码密钥长度 (00 00 00 18)

* 块密码块大小 (00 00 00 10)

* HMAC 密钥长度 (00 00 00 20)

* HMAC 摘要的大小 (00 00 00 20)

* 块密码 PRP 输出 (F4 74-DB 6F) 和

* HMAC PRF 输出 (D4 79-结束)。

> [!NOTE]
> CBC 模式下加密 + HMAC 身份验证上下文标头生成而不考虑算法实现是否提供通过 Windows CNG 或托管 SymmetricAlgorithm 和 KeyedHashAlgorithm 类型相同的方式。 这允许在不同操作系统上运行的应用程序可靠地生成相同的上下文标头，即使之间的 Os 不同算法的实现。 （在实践中，KeyedHashAlgorithm 不必是正确的 HMAC。 它可以是任何加密哈希算法类型。）

### <a name="example-3des-192-cbc--hmacsha1"></a>示例： 3DES 192 CBC + HMACSHA1

首先，让 (K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 192 位和 |K_H |= 每个指定的算法的 160 位。 这将导致 K_E = A219...E2BB 和 K_H = DC4A...在下面的示例 B464:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

接下来，计算 Enc_CBC (K_E、 IV，"") 对于给定 IV 的 3DES-192-CBC = 0 * 和 K_E 按上面所述。

结果: = ABB100F81E53E10E

接下来，计算 MAC (K_H，"") 为 HMACSHA1 给定 K_H 按上面所述。

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

此示例的完整上下文标头，这是经过身份验证的指纹将产生如下所示的加密算法对 （3DES 192 CBC 加密 + HMACSHA1 验证）：

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

组件，如下所示细分：

* 标记 (00 00)

* 块密码密钥长度 (00 00 00 18)

* 块密码块大小 (00 00 00 08)

* HMAC 密钥长度 (00 00 00 14)

* HMAC 摘要的大小 (00 00 00 14)

* 块密码 PRP 输出 (AB B1-E1 0E) 和

* HMAC PRF 输出 (76 EB-结束)。

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/计数器模式加密 + 身份验证

上下文标头由以下组件组成：

* [16 位]值 00 01，是一个标记，这意味着"GCM 加密 + 身份验证"。

* [32 位]对称块加密算法密钥长度 （以字节为单位，big endian）。

* [32 位]在经过身份验证的加密操作期间使用的 nonce 大小 （以字节为单位，big endian）。 (对于我们的系统，这固定的 nonce 大小 = 96 位。)

* [32 位]块大小 （以字节为单位，big endian） 的对称块加密算法。 (为 GCM，这固定的块大小 = 128 位。)

* [32 位]身份验证标记大小 （以字节为单位，big endian） 已经过身份验证的加密函数生成。 (对于我们的系统，这固定为标记大小 = 128 位。)

* [128 位]Enc_GCM 的标记 (K_E，nonce，"")，它是给定的空字符串输入对称的块密码算法的输出，并且其中 nonce 是 96 位全为零向量。

K_E 派生使用相同的机制，如下所示 CBC 加密 + HMAC 身份验证方案。 但是，由于在此处 play 中没有任何 K_H，我们实质上具有 |K_H |= 0，且该算法会折叠到下面的表单。

K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-256-gcm"></a>示例： AES 256 GCM

首先，让 K_E = SP800_108_CTR (prf = HMACSHA512，键 =""，标签 =""，上下文 ="")，其中 |K_E |= 256 位。

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

接下来，计算 Enc_GCM 的身份验证标记 (K_E，nonce，"") 适用于给定 nonce 的 AES-256-GCM = 096 和 K_E 按上面所述。

result := E7DCCE66DF855A323A6BB7BD7A59BE45

这将产生下面的完整上下文标头：

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

组件，如下所示细分：

   * 标记 (00 01)

   * 块密码密钥长度 (00 00 00 20)

   * nonce 的大小 (00 00 00 0 C)

   * 块密码块大小 (00 00 00 10)

   * 身份验证标记大小 (00 00 00 10) 和

   * 从运行块密码的身份验证标记 (E7 DC-结束)。
