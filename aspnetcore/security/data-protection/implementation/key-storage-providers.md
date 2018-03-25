---
title: "密钥存储提供程序"
author: rick-anderson
description: "了解有关 ASP.NET 核心以及如何配置密钥的存储位置中的密钥存储提供程序。"
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 83e02a19e465b3ff81a0c0c62c2c8b090bfab052
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="key-storage-providers"></a>密钥存储提供程序

<a name="data-protection-implementation-key-storage-providers"></a>

默认情况下，数据保护系统[使用启发式方法](xref:security/data-protection/configuration/default-settings)以确定应将在其中保留加密的密钥材料。 开发人员可以重写启发式方法，并手动指定的位置。

> [!NOTE]
> 如果您指定一个显式的密钥保持位置，将取消注册数据保护系统在 rest 机制启发式方法提供的默认密钥加密，以便密钥将不再加密对静止。 它具有，建议你此外[指定显式密钥加密机制](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)对于生产应用程序。

数据保护系统附带了几个内置密钥存储提供程序。

## <a name="file-system"></a>文件系统

我们预计很多应用程序将使用的文件基于系统的密钥存储库。 若要此配置，调用[PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)配置例程如下所示。 提供`DirectoryInfo`指向应在其中存储密钥的存储库。

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

`DirectoryInfo`可以指向本地计算机上的目录或它可以指向网络共享上的文件夹。 如果指向本地计算机上的目录 （和的方案是，只有本地计算机上的应用程序将需要使用此存储库），请考虑使用[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)来加密存放的密钥。 否则，请考虑使用[X.509 证书](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)来加密存放的密钥。

## <a name="azure-and-redis"></a>Azure 和 Redis

`Microsoft.AspNetCore.DataProtection.AzureStorage`和`Microsoft.AspNetCore.DataProtection.Redis`包允许将你的数据保护密钥存储在 Azure 存储空间或 Redis 缓存。 可以在 web 应用程序的多个实例之间共享密钥。 你的 ASP.NET Core 应用可以跨多个服务器共享身份验证 cookie 或 CSRF 保护。 若要在 Azure 上配置，调用之一[PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs)重载，如下所示。

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

另请参阅[Azure 测试代码](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs)。

若要在 Redis 上配置，调用之一[PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs)重载，如下所示。

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

有关详细信息，请参阅以下主题：

- [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Azure Redis 缓存](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Redis 测试代码](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs)。

## <a name="registry"></a>注册表

有时应用程序可能没有写到文件系统的访问权限。 请考虑其中作为是虚拟服务帐户 （如 w3wp.exe 的应用程序池标识） 运行应用程序的方案。 在这些情况下，管理员可能已设置为服务帐户标识的相应 ACLed 的注册表项。 调用[PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs)配置例程如下所示。 提供`RegistryKey`指向存储加密密钥/值的位置。

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

如果作为持久性机制使用系统注册表，请考虑使用[Windows DPAPI](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)来加密存放的密钥。

## <a name="custom-key-repository"></a>自定义密钥存储库

如果不适合的内置机制，开发人员可以通过提供自定义指定自己的密钥保持机制`IXmlRepository`。
