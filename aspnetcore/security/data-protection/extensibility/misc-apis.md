---
title: "各种 API"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护 ISecret 接口。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>各种 API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> 实现的任意以下接口的类型应该是线程安全的多个调用方。

## <a name="isecret"></a>ISecret

`ISecret`接口表示一个机密的值，如加密的密钥材料。 它包含以下 API 图面：

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer`方法填充原始机密值与提供的缓冲区。 此 API 将作为参数的缓冲区的原因而不返回`byte[]`直接是，这使调用方机会固定限制对托管的垃圾回收器的机密暴露该缓冲区对象。

`Secret`类型是的具体实现`ISecret`机密的值在进程中内存中的存储位置。 在 Windows 平台上的机密的值加密通过[CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)。
