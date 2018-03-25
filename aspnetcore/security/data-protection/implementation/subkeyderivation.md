---
title: "子项派生和经过身份验证的加密"
author: rick-anderson
description: "本文档说明 ASP.NET 核心数据保护的实现详细信息子项派生和身份验证加密。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 4b905bbc7bb064b6ba1741557bd694c8c67ccfa8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>子项派生和经过身份验证的加密

<a name="data-protection-implementation-subkey-derivation"></a>

密钥链中的大多数键将包含某种形式的平均信息量，并且将有算法信息指出"CBC 模式下加密 + HMAC 验证"或"GCM 加密 + 验证"。 在这些情况下，我们将嵌入的平均信息量称为此密钥的主密钥材料 （或公里） 和我们执行派生将使用为实际的加密操作的密钥的密钥派生函数。

> [!NOTE]
> 键是抽象的并自定义实现可能不会按如下所示的那样。 如果密钥提供其自己的实现`IAuthenticatedEncryptor`而不是使用我们的内置工厂之一，本节中所述的机制不再适用。

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>其他经过身份验证的数据和子项派生

`IAuthenticatedEncryptor`接口用作所有经过身份验证的加密操作的核心接口。 其`Encrypt`方法采用两个缓冲区： 纯文本和 additionalAuthenticatedData (AAD)。 纯文本内容流保持不变调用`IDataProtector.Protect`，但 AAD 由系统生成和包括三个组件：

1. 32 位神奇标头 09 F0 C9 F0 标识此版本的数据保护系统。

2. 128 位密钥 id。

3. 从创建目的链形成的可变长度字符串`IDataProtector`，正在执行此操作。

由于 AAD 是为所有三个组件的元组唯一的我们可以使用它从密钥主机派生新密钥，而不是使用密钥主机本身中所有的我们的加密操作。 每次调用`IAuthenticatedEncryptor.Encrypt`，发生以下密钥派生过程：

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

在这里，我们正在呼叫 NIST SP800 108 KDF 中计数器模式 (请参阅[NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)，sec。 5.1) 使用以下参数：

* 密钥派生密钥 (KDK) = K_M

* PRF = HMACSHA512

* 标签 = additionalAuthenticatedData

* 上下文 = contextHeader | |keyModifier

上下文标头的可变长度和本质就是我们要为其派生 K_E 和 K_H 的算法的指纹。 密钥修饰符是每次调用随机生成一个 128 位字符串`Encrypt`并提供以确保与绝大多数概率 KE 和 KH 是唯一对于此特定身份验证加密操作，即使所有其他输入给 KDF 为常数。

CBC 模式下加密 + HMAC 验证操作，|K_E |的密钥长度是对称的块密码，和 |K_H |是的 HMAC 例程的摘要大小。 对 GCM 加密 + 验证操作 |K_H |= 0。

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC 模式下加密 + HMAC 验证

一旦 K_E 生成通过上述机制中，我们生成的随机初始化向量，并运行该对称的块密码算法来加密纯文本。 然后通过使用密钥 K_H 以生成 MAC 初始化 HMAC 例程将的初始化向量和已加密文本运行 下面以图形方式表示此过程和返回值。

![CBC 模式过程并返回](subkeyderivation/_static/cbcprocess.png)

*输出: = keyModifier | |iv | |E_cbc （K_E，iv，数据） | |HMAC (K_H、 iv | |E_cbc （K_E，iv，数据）)*

> [!NOTE]
> `IDataProtector.Protect`实现将[前面预置的幻标头和密钥 id](authenticated-encryption-details.md)到之前将其返回到调用方的输出。 因为神奇的标头和密钥 id 是隐式的一部分[AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad)，因为密钥修饰符作为输入提供给 KDF，这意味着 MAC 进行身份验证的最终返回负载的每个单字节

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/计数器模式加密 + 验证

一旦 K_E 生成通过上述机制中，我们将生成随机的 96 位 nonce 和运行对称块加密算法，来加密纯文本，并生成的 128 位身份验证标记。

![GCM 模式过程并返回](subkeyderivation/_static/galoisprocess.png)

*输出: = keyModifier | |nonce | |E_gcm （K_E，nonce，数据） | |authTag*

> [!NOTE]
> 即使 GCM 本机支持 AAD 的概念，我们有一个要仍输入 AAD 仅对原始 KDF 中，选择要传递到 GCM 其 AAD 参数为空字符串。 这样做的原因是两个折叠。 首先，[以支持灵活性](context-headers.md#data-protection-implementation-context-headers)我们永远不希望将 K_M 直接用作加密密钥。 此外，GCM 有一定非常严格的唯一性要求其输入。 GCM 加密例程是曾调用对两个或更清晰的概率具有相同 （键，nonce） 的输入数据集对不能超过 2 ^32。 如果我们修复 K_E 我们无法执行多个 2 ^32 加密操作之前运行与我们的 2 ^-32 限制。 这可能看起来非常大量的操作，但在几天内，这些密钥的正常生存期内良好的高流量 web 服务器可以通过 40 亿个请求。 保持合规的 2 ^ 我们继续使用 128 位密钥修饰符和 96 位 nonce，因而可真正扩展任何给定 K_M 的可用操作计数-32 概率限制。 为简单起见的设计中，我们将共享 CBC 和 GCM 操作之间的 KDF 代码路径，并且由于 AAD 已被视为 KDF 中没有无需将其转发给 GCM 例程。
