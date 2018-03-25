---
title: "在 ASP.NET 核心中配置数据保护"
author: rick-anderson
description: "了解如何在 ASP.NET 核心中配置数据保护。"
manager: wpickett
ms.author: riande
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 0fe1fd7b81a0e5aa00ae14c7e6fdbd9cc88ec4fe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>在 ASP.NET 核心中配置数据保护

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

初始化数据保护系统时，它将应用[默认设置](xref:security/data-protection/configuration/default-settings)基于的操作环境。 这些设置并在一台计算机上运行的应用程序通常适用。 在开发人员可能需要更改默认设置，可能是因为其应用程序分布在多个计算机之间或者出于合规原因的情况下。 对于这些情况下，数据保护系统提供丰富的配置 API。

扩展方法没有[AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection)返回[IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder)。 `IDataProtectionBuilder`显示扩展方法，你可以链接在一起以配置数据保护选项。

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

将密钥存储在 UNC 共享而不是在*%LOCALAPPDATA%*默认位置，配置与系统[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> 如果更改密钥持久性位置，系统将不再自动加密组合键，其余部分，因为它不知道 DPAPI 是否合适的加密机制。

## <a name="protectkeyswith"></a>ProtectKeysWith\*

你可以将系统配置为通过调用的任何保护静止的密钥[ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions)配置 Api。 请考虑以下示例中，它将密钥存储的 UNC 共享上并对这些密钥在使用特定的 X.509 证书的其余部分进行加密：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

请参阅[将加密密钥在 Rest](xref:security/data-protection/implementation/key-encryption-at-rest)有关更多示例和讨论的内置密钥加密机制。

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

若要配置系统而不是默认值 90 天使用的密钥生存期为 14 天，使用[SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

默认情况下，数据保护系统隔离应用程序从另一个，即使它们共享相同的物理密钥存储库。 这可以阻止应用了解对方的受保护的负载。 若要共享两个应用程序之间的受保护的负载，使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)与每个应用程序相同的值：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

您可能有不希望自动轮转 （创建新的密钥） 的密钥，因为它们接近过期的应用的方案。 这一个示例可能是应用程序设置在主要/辅助关系中，其中仅主应用程序负责密钥管理问题，而辅助应用只需密钥环的只读视图。 可以配置辅助应用程序通过配置与系统以只读方式处理密钥环[DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>每个应用程序隔离

数据保护系统提供了 ASP.NET 核心主机，但它自动隔离了从另一个，应用程序，即使这些应用在相同的工作进程帐户下运行，并且使用相同的主密钥材料。 这是某种程度上类似于 IsolateApps 修饰符从 System.Web 的 **\<machineKey >**元素。

隔离机制的工作原理是作为唯一的租户，因此考虑本地计算机上的每个应用[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)取得 root 权限的任何给定的应用会自动包括为鉴别器的应用程序 ID。 应用程序的唯一 ID 来自两个位置之一：

1. 如果在 IIS 中托管应用的唯一标识符是应用程序的配置路径。 如果在 web 场环境中部署应用后，此值应为稳定，前提是在 web 场中的所有计算机同样配置 IIS 环境。

2. 如果应用程序不在 IIS 中承载的的唯一标识符是应用程序的物理路径。

唯一标识符旨在得以重置&mdash;同时的各个应用程序和计算机本身。

此隔离机制假设应用不可恶意。 恶意应用程序始终可以影响在相同的工作进程帐户下运行的任何其他应用程序。 在共享宿主环境中应用是相互不受信任，托管提供商应采取措施来确保应用，包括分离应用的基础密钥的存储库之间的操作系统级别隔离。

如果由 ASP.NET 核心主机未提供数据保护系统 (例如，如果通过其实例化`DataProtectionProvider`具体类型) 应用程序隔离在默认情况下处于禁用状态。 当禁用应用程序隔离时，相同的密钥材料作为后盾的所有应用可以都共享的负载，只要它们提供相应[目的](xref:security/data-protection/consumer-apis/purpose-strings)。 若要提供在此环境中的应用程序隔离，调用[SetApplicationName](#setapplicationname)方法的配置对象，并提供每个应用程序的唯一名称。

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>更改与 UseCryptographicAlgorithms 算法

数据保护堆栈可以更改默认的算法使用的新生成的键。 执行此操作的最简单方法是调用[UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms)从配置回调：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

默认值 EncryptionAlgorithm AES 256 CBC，且默认 ValidationAlgorithm HMACSHA256。 默认策略可以设置由系统管理员通过[计算机范围策略](xref:security/data-protection/configuration/machine-wide-policy)，但显式调用`UseCryptographicAlgorithms`将替代默认策略。

调用`UseCryptographicAlgorithms`允许你指定从预定义的内置列表所需的算法。 你不必担心算法的实现。 在上述方案中，数据保护系统会尝试在 Windows 上运行使用 AES 的 CNG 实现。 否则，则会返回到托管[System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes)类。

你可以手动指定通过调用实现[UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms)。

> [!TIP]
> 更改算法不会影响现有的密钥在密钥环。 它只影响新生成的键。

### <a name="specifying-custom-managed-algorithms"></a>指定自定义托管的算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自定义托管的算法，创建[ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration)指向的实现类型的实例：

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定自定义托管的算法，创建[ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings)指向的实现类型的实例：

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

通常\*类型属性必须指向具体，可实例化 （通过公共的无参数 ctor) 实现的[SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm)和[KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm)，不过系统特殊的情况下等某些值`typeof(Aes)`为方便起见。

> [!NOTE]
> SymmetricAlgorithm 必须 ≥ 128 位的密钥长度和块大小的 ≥ 64 位，并且它必须支持使用 PKCS #7 填充 CBC 模式下的加密。 KeyedHashAlgorithm 的摘要大小必须 > = 128 位，并且它必须支持密钥长度等于 length 哈希算法的摘要。 KeyedHashAlgorithm 并非是严格要求，要 HMAC。

### <a name="specifying-custom-windows-cng-algorithms"></a>指定自定义 Windows CNG 算法

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自定义 Windows CNG 算法通过 HMAC 验证使用 CBC 模式下加密，创建[CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration)实例，其中包含的算法的信息：

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定自定义 Windows CNG 算法通过 HMAC 验证使用 CBC 模式下加密，创建[CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings)实例，其中包含的算法的信息：

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> 对称块加密算法的密钥长度必须 > = 128 位的块大小 > = 64 位，并且它必须支持使用 PKCS #7 填充 CBC 模式下的加密。 哈希算法的摘要大小必须 > = 128 位并且必须支持与 BCRYPT 打开\_ALG\_处理\_HMAC\_标志标志。 \*提供程序属性可以设置为空，默认的提供程序用于为指定的算法。 请参阅[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文档以了解更多信息。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要指定自定义 Windows CNG 算法通过验证使用 Galois/计数器模式加密，创建[CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration)实例，其中包含的算法的信息：

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要指定自定义 Windows CNG 算法通过验证使用 Galois/计数器模式加密，创建[CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings)实例，其中包含的算法的信息：

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> 对称块加密算法的密钥长度必须 > = 128 位的块大小为完全 128 位，并且它必须支持 GCM 加密。 你可以设置[EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider)属性为 null，以便使用默认提供程序指定的算法。 请参阅[BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx)文档以了解更多信息。

### <a name="specifying-other-custom-algorithms"></a>指定其他自定义算法

尽管不作为第一类 API 公开，则数据保护系统是算法的可扩展，足以允许指定几乎任何类型。 例如，很可能需要包含在硬件安全模块 (HSM) 的所有键，并为核心的自定义实现加密和解密的例程。 请参阅[IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor)中[核心加密可扩展性](xref:security/data-protection/extensibility/core-crypto)有关详细信息。

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>保留密钥： 当在 Docker 容器中承载

在中承载时[Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/)容器，应在维护密钥：

* 是一个 Docker 卷容器的生存期结束，例如共享的卷或者主机已装入卷后仍然存在，一个文件夹。
* 外部提供程序，如[Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)或[Redis](https://redis.io/)。

## <a name="see-also"></a>请参阅

* [非 DI 感知方案](xref:security/data-protection/configuration/non-di-scenarios)
* [计算机范围的策略](xref:security/data-protection/configuration/machine-wide-policy)
