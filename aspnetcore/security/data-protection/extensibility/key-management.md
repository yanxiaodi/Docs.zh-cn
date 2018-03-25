---
title: "密钥管理扩展性"
author: rick-anderson
description: "本文档概述了 ASP.NET 核心数据保护密钥管理扩展性。"
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: bcc4984efcee9a6ffd0f3b503a38089c78adf5e8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="key-management-extensibility"></a>密钥管理扩展性

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> 读取[密钥管理](../implementation/key-management.md#data-protection-implementation-key-management)之前阅读此部分中，因为它介绍了这些 Api 的基本概念的某些部分。

>[!WARNING]
> 实现的任意以下接口的类型应该是线程安全的多个调用方。

## <a name="key"></a>键

`IKey`接口是加密系统中的键的基本表示。 在抽象的意义上，不在"加密密钥材料"字面意义上此处使用的术语密钥。 密钥具有以下属性：

* 激活、 创建和到期日期

* 吊销状态

* 密钥标识符 (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

此外，`IKey`公开`CreateEncryptor`方法用于创建[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)实例绑定到此密钥。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

此外，`IKey`公开`CreateEncryptorInstance`方法用于创建[IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)实例绑定到此密钥。

---

> [!NOTE]
> 没有 API 检索从原始的加密材料`IKey`实例。

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager`接口表示负责常规的密钥存储、 检索和操作的对象。 它公开三个高级操作：

* 创建新密钥并将其保存在存储。

* 从存储中获取所有键。

* 撤消一个或多个密钥，而且将保留到存储的吊销信息。

>[!WARNING]
> 编写`IKeyManager`是一个非常高级的任务，和大多数开发人员不应尝试。 相反，大多数开发人员应充分利用所提供的功能[XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)类。

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager`类型是内置的具体实现`IKeyManager`。 它提供了几个有用的功能，包括密钥证书和加密对静止的密钥。 在此系统的密钥将呈现为 XML 元素 (具体而言， [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview))。

`XmlKeyManager` 依赖于在完成其任务的过程中的其他几个组件：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`这决定使用新密钥的算法。

* `IXmlRepository`其中键将保留在存储中的控件。

* `IXmlEncryptor` [可选]，这允许加密对静止的密钥。

* `IKeyEscrowSink` [可选]，其提供密钥托管服务。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`其中键将保留在存储中的控件。

* `IXmlEncryptor` [可选]，这允许加密对静止的密钥。

* `IKeyEscrowSink` [可选]，其提供密钥托管服务。

---

以下是高级关系图，表明如何这些组件连接在一起内`XmlKeyManager`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![密钥创建](key-management/_static/keycreation2.png)

   *密钥创建 / CreateNewKey*

实现中`CreateNewKey`、`AlgorithmConfiguration`组件用于创建唯一`IAuthenticatedEncryptorDescriptor`，其中然后序列化为 XML。 如果存在密钥托管接收器，则原始 （未加密） 的 XML 到接收器提供适用于长期存储。 然后通过运行的未加密的 XML `IXmlEncryptor` （如果需要） 来生成加密的 XML 文档。 此加密的文档保存到长期存储通过`IXmlRepository`。 (如果没有`IXmlEncryptor`是配置，未加密的文档保存在`IXmlRepository`。)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![密钥创建](key-management/_static/keycreation1.png)

   *密钥创建 / CreateNewKey*

实现中`CreateNewKey`、`IAuthenticatedEncryptorConfiguration`组件用于创建唯一`IAuthenticatedEncryptorDescriptor`，其中然后序列化为 XML。 如果存在密钥托管接收器，则原始 （未加密） 的 XML 到接收器提供适用于长期存储。 然后通过运行的未加密的 XML `IXmlEncryptor` （如果需要） 来生成加密的 XML 文档。 此加密的文档保存到长期存储通过`IXmlRepository`。 (如果没有`IXmlEncryptor`是配置，未加密的文档保存在`IXmlRepository`。)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![密钥检索](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![密钥检索](key-management/_static/keyretrieval1.png)

---

   *密钥检索 / GetAllKeys*

实现中`GetAllKeys`、 XML 文档表示键和从基础读取吊销`IXmlRepository`。 如果这些文档进行加密，系统将自动解密。 `XmlKeyManager` 创建适当`IAuthenticatedEncryptorDescriptorDeserializer`实例进行反序列化文档回`IAuthenticatedEncryptorDescriptor`实例，然后将封装在单个`IKey`实例。 此集合`IKey`实例返回给调用方。

在找不到在特定的 XML 元素上的进一步信息[密钥存储格式文档](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format)。

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository`接口表示可保留 XML 到和从一个后备存储检索 XML 的类型。 它公开两个 Api:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement （XElement 元素，字符串 friendlyName）

实现`IXmlRepository`不需要通过其传递对 XML 进行分析。 它们应视为不透明 XML 文档，让较高层担心生成和分析文档。

有两个内置的具体类型实现`IXmlRepository`:`FileSystemXmlRepository`和`RegistryXmlRepository`。 请参阅[密钥存储提供程序文档](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers)有关详细信息。 注册的自定义`IXmlRepository`将适当的方式为使用不同的后备存储，例如，Azure Blob 存储。

若要更改默认存储库应用程序级，注册一个自定义`IXmlRepository`实例：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor`接口表示可以加密纯文本 XML 元素的类型。 它公开单个 API:

* 加密 (XElement plaintextElement): EncryptedXmlInfo

如果一个序列化`IAuthenticatedEncryptorDescriptor`包含任何元素标记为"需要加密"，然后`XmlKeyManager`将通过配置运行这些元素`IXmlEncryptor`的`Encrypt`方法，和它将保留 enciphered 的元素而不是为纯文本元素`IXmlRepository`。 输出`Encrypt`方法是`EncryptedXmlInfo`对象。 此对象是包装器，其中包含两个所产生的 enciphered`XElement`和表示类型`IXmlDecryptor`可用于解密的相应元素。

有四个内置的具体类型实现`IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

请参阅[rest 文档的密钥加密](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)有关详细信息。

若要更改默认密钥加密在 rest 机制整个应用程序范围，注册一个自定义`IXmlEncryptor`实例：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor`接口表示一种类型，知道如何解密`XElement`，已通过 enciphered `IXmlEncryptor`。 它公开单个 API:

* 解密 (XElement encryptedElement): XElement

`Decrypt`方法撤消由执行加密`IXmlEncryptor.Encrypt`。 通常情况下，每个具体`IXmlEncryptor`实现将具有相应的具体`IXmlDecryptor`实现。

类型可实现`IXmlDecryptor`应该具有以下两个公共构造函数之一：

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider`传递给构造函数可能为 null。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink`接口表示可以执行托管的敏感信息的类型。 回想一下，序列化的描述符可能包含敏感信息 （如加密材料），这是什么导致了引入[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)键入第一个位置。 但是，意外的发生，并且密钥环可以删除或损坏。

托管接口提供了允许访问原始序列化的 XML 转换由任何配置前紧急转义阴影[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)。 接口公开单个 API:

* 应用商店 （Guid keyId、 XElement 元素）

这就需要通过`IKeyEscrowSink`实现，以安全的方式与业务策略一致处理提供的元素。 一个可能的实现可能是为托管接收器使用已知的企业 X.509 证书的 XML 元素进行加密其中已托管证书的私钥;`CertificateXmlEncryptor`类型可以对此有帮助。 `IKeyEscrowSink`实现程序还负责适当地保留提供的元素。

默认情况下没有托管机制启用，但服务器管理员可以[全局配置此](xref:security/data-protection/configuration/machine-wide-policy)。 它还可以配置以编程方式通过`IDataProtectionBuilder.AddKeyEscrowSink`方法，如下面的示例中所示。 `AddKeyEscrowSink`方法重载镜像`IServiceCollection.AddSingleton`和`IServiceCollection.AddInstance`重载，为`IKeyEscrowSink`实例都应是单一实例。 如果选择多个`IKeyEscrowSink`注册实例，每个过程中将调用密钥生成，因此键可以同时托管多个机制。

没有任何 API 来读取中的材料`IKeyEscrowSink`实例。 这是与托管机制的设计理论一致： 它具有用于受信任的颁发机构，方便密钥材料，并且应用程序本身不是受信任颁发机构，因为它不应具有访问自己保管材料。

下面的示例代码演示如何创建和注册`IKeyEscrowSink`其中托管密钥，以便只有"CONTOSODomain 管理员"的成员可以恢复它们。

> [!NOTE]
> 若要运行此示例，你必须是在已加入域的 Windows 8 / Windows Server 2012 计算机和域控制器必须是 Windows Server 2012 或更高版本。

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
