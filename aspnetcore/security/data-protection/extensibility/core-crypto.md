---
title: "核心加密可扩展性"
author: rick-anderson
description: "说明 IAuthenticatedEncryptor、 IAuthenticatedEncryptorDescriptor、 IAuthenticatedEncryptorDescriptorDeserializer 和顶级工厂。"
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: ead4012236244d88cff0b0520d000d89f93f3355
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="core-cryptography-extensibility"></a>核心加密可扩展性

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> 实现的任意以下接口的类型应该是线程安全的多个调用方。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor**接口是加密子系统的基本构建基块。 通常没有一个 IAuthenticatedEncryptor 每个键，并且 IAuthenticatedEncryptor 实例包装所有加密的密钥材料和算法执行加密操作所需的信息。

顾名思义，类型是负责提供经过身份验证的加密和解密服务。 以下两个 Api，它公开。

* 解密 (ArraySegment<byte>已加密文本，ArraySegment<byte> additionalAuthenticatedData): byte]

* 加密 (ArraySegment<byte>纯文本，ArraySegment<byte> additionalAuthenticatedData): byte]

加密方法返回的 blob，包括 enciphered 纯文本和身份验证标记。 身份验证标记必须包含附加经过身份验证的数据 (AAD) 时，尽管 AAD 本身不需要从最终的负载。 解密方法验证身份验证标记，并返回被解码的负载。 应为 CryptographicException homogenized （除 ArgumentNullException 和类似） 的所有失败。

> [!NOTE]
> 实际上，IAuthenticatedEncryptor 实例本身不需要包含密钥材料。 例如，实现可以将委托到 HSM 中的所有操作。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>如何创建 IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory**接口表示一种类型，知道如何创建[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)实例。 其 API 是，如下所示。

* CreateEncryptorInstance （IKey 密钥）： IAuthenticatedEncryptor

对于任何给定的 IKey 实例，其 CreateEncryptorInstance 方法创建的任何经过身份验证的加密程序应被视为等效，如下面的代码示例。

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor**接口表示一种类型，知道如何创建[IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor)实例。 其 API 是，如下所示。

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

如 IAuthenticatedEncryptor，假定的 IAuthenticatedEncryptorDescriptor 实例包装一个特定键。 这意味着，对于任何给定的 IAuthenticatedEncryptorDescriptor 实例，其 CreateEncryptorInstance 方法创建的任何经过身份验证的加密程序应被视为等效，如下面的代码示例。

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor （ASP.NET 核心 2.x 仅）

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor**接口表示知道如何将自身导出到 XML 的类型。 其 API 是，如下所示。

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML 序列化

IAuthenticatedEncryptor 和 IAuthenticatedEncryptorDescriptor 的主要区别是描述符知道如何创建加密程序和其提供有效的参数。 请考虑其实现依赖于 SymmetricAlgorithm 和 KeyedHashAlgorithm IAuthenticatedEncryptor。 加密器的作业是使用这些类型，但它不一定知道这些类型的来源，因此它不能真正写出如何重新创建本身应用程序重启时的正确说明。 描述符充当基于此较高级别。 由于描述符知道如何创建加密程序实例 （例如，它知道如何创建所需的算法），以便可以重新创建加密器实例，应用程序重置后，它可以序列化该知识库中的 XML 格式。

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

通过其 ExportToXml 例程，描述符可序列化。 此例程返回 XmlSerializedDescriptorInfo 其中包含两个属性： XElement 表示形式的说明符和表示的类型[IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer)可以是用于使重新起用给定相应 XElement 此描述符。

序列化的描述符可能包含敏感信息，例如加密的密钥材料。 数据保护系统具有已持久化到存储空间之前加密信息的内置支持。 若要充分利用此功能，描述符应标记的元素，其中包含敏感信息的属性名称"requiresEncryption"(xmlns"http://schemas.asp.net/2015/03/dataProtection")，值"true"。

>[!TIP]
> 没有用于将此属性设置的帮助器 API。 调用 XElement.MarkAsRequiresEncryption() 位于命名空间 Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel 扩展方法。

也可以是其中的序列化的描述符不包含敏感信息的情况。 再次考虑存储在 HSM 中的加密密钥的大小写。 在序列化本身由于 HSM 就不会暴露纯文本形式材料时可描述符无法写出密钥材料。 相反，描述符可以编写出密钥包装的密钥 （如果 HSM 允许以这种方式导出） 或密钥的 HSM 自己唯一标识符版本。

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer**接口表示知道如何反序列化从 XElement IAuthenticatedEncryptorDescriptor 实例的类型。 它公开一种方法：

* ImportFromXml （XElement 元素）： IAuthenticatedEncryptorDescriptor

ImportFromXml 方法采用返回 XElement [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml)和创建原始 IAuthenticatedEncryptorDescriptor 等效项。

实现 IAuthenticatedEncryptorDescriptorDeserializer 该类型应具有以下两个公共构造函数之一：

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider 传递到构造函数可能为 null。

## <a name="the-top-level-factory"></a>顶级工厂

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration**类表示知道如何创建该类型[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)实例。 它公开一个单一的 API。

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

将 AlgorithmConfiguration 视为顶级工厂。 配置用作模板。 它所包装算法信息 （例如，此配置生成使用 AES 128 GCM 主密钥描述符），但它尚不与特定键关联。

当调用 CreateNewDescriptor、 仅用于此调用中，创建新的密钥材料，并且生成新 IAuthenticatedEncryptorDescriptor 其包装此密钥材料以及所需使用材料算法信息。 无法在软件中创建 （和保存在内存中） 的密钥材料，它无法创建并保存在 HSM 中，依次类推。 关键点是 CreateNewDescriptor 任何两个调用应永远不会创建等效 IAuthenticatedEncryptorDescriptor 实例。

AlgorithmConfiguration 类型用作密钥创建例程的入口点如[自动密钥滚动](../implementation/key-management.md#key-expiration-and-rolling)。 若要更改的所有将来的键的实现，请在 KeyManagementOptions 中设置 AuthenticatedEncryptorConfiguration 属性。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration**接口表示一种类型它知道如何创建[IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor)实例。 它公开一个单一的 API。

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

将 IAuthenticatedEncryptorConfiguration 视为顶级工厂。 配置用作模板。 它所包装算法信息 （例如，此配置生成使用 AES 128 GCM 主密钥描述符），但它尚不与特定键关联。

当调用 CreateNewDescriptor、 仅用于此调用中，创建新的密钥材料，并且生成新 IAuthenticatedEncryptorDescriptor 其包装此密钥材料以及所需使用材料算法信息。 无法在软件中创建 （和保存在内存中） 的密钥材料，它无法创建并保存在 HSM 中，依次类推。 关键点是 CreateNewDescriptor 任何两个调用应永远不会创建等效 IAuthenticatedEncryptorDescriptor 实例。

IAuthenticatedEncryptorConfiguration 类型用作密钥创建例程的入口点如[自动密钥滚动](../implementation/key-management.md#key-expiration-and-rolling)。 若要更改的所有将来的键的实现，请在服务容器中注册的单独 IAuthenticatedEncryptorConfiguration。

---
