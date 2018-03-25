---
title: "密钥存储格式"
author: tdykstra
description: "本文档介绍了 ASP.NET 核心数据保护密钥存储格式的实现详细信息。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 66783eb7264a4551eafdd9d5c7d99b014701a6de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-format"></a>密钥存储格式

<a name="data-protection-implementation-key-storage-format"></a>

对象存储在 XML 表示形式中的其余部分。 密钥存储的默认目录为 %localappdata%\asp.net\dataprotection-keys\。

## <a name="the-key-element"></a>\<密钥 > 元素

注册表项存在作为密钥存储库中的顶级对象。 按照约定密钥具有 filename**密钥-{guid}.xml**，其中 {guid} 是密钥的 id。 每个此类文件包含一个密钥。 文件的格式如下所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

\<密钥 > 元素包含以下属性和子元素：

* 密钥 id。此值是被看作是授权;文件名是只需以方便人们阅读过去。

* 版本\<密钥 > 元素，当前固定为 1。

* 密钥的创建、 激活和到期日期。

* A\<描述符 > 元素，它包含有关该注册表项中包含的经过身份验证的加密实现的信息。

在上面的示例中，密钥的 id 是 {80732141-ec8f-4b80-af9c-c4d2d1ff8901}，它已创建并将在 2015 年 3 月 19 日激活且它具有 90 天的生存期。 （有时激活日期可能略有如此示例所示的创建日期之前。 这是由于 n 它在 Api 工作和无碍在实践中的方式。）

## <a name="the-descriptor-element"></a>\<描述符 > 元素

外部\<描述符 > 元素包含属性 deserializerType，这是实现 IAuthenticatedEncryptorDescriptorDeserializer 的类型的程序集限定名称。 此类型是负责读取内部\<描述符 > 元素和分析中包含的信息。

特定的格式\<描述符 > 元素取决于经过身份验证的加密程序实现的键，封装和每个反序列化程序类型所预期的此略有不同的格式。 一般情况下，不过，此元素将包含算法的信息 (名称、 类型，Oid，或类似) 和机密密钥材料。 在上面的示例中，该描述符指定此密钥包装 AES 256 CBC 加密 + HMACSHA256 验证。

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > 元素

<encryptedSecret>元素它包含机密的密钥材料的加密的表单可能出现如果[启用了加密对静止的机密](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)。 属性 decryptorType 将实现 IXmlDecryptor 的类型的程序集限定名称。 此类型是负责读取内部<encryptedKey>元素和解密以恢复原始的纯文本。

与\<描述符 >，特定的格式<encryptedSecret>元素取决于正在使用的静态加密机制。 在上面的示例中，每个注释中使用 Windows DPAPI 对主密钥进行加密。

## <a name="the-revocation-element"></a>\<吊销 > 元素

吊销存在作为密钥存储库中的顶级对象。 按照约定吊销具有 filename**吊销-{时间戳}.xml** （适用于特定日期之前撤消所有键） 或**吊销-{guid}.xml** （对于都撤消特定键）。 每个文件包含单个\<吊销 > 元素。

对于各个密钥吊销，该文件的内容将如下。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

在这种情况下，只有指定的密钥已吊销。 如果密钥 id 为"*"，但是，如以下示例中，其创建日期是在指定的吊销日期之前的所有键都被都吊销。

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<原因 > 由系统永远不会读取元素。 它是只需存储吊销的用户可读的原因方便位置。
