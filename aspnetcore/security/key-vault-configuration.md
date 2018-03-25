---
title: "在 ASP.NET 核心的 azure 密钥保管库配置提供程序"
author: guardrex
description: "了解如何使用 Azure 密钥保管库配置提供程序配置应用程序使用在运行时加载的名称-值对。"
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: e1a4be77417f0a74182f1b123bfba429737d4330
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>在 ASP.NET 核心的 azure 密钥保管库配置提供程序

通过[Luke Latham](https://github.com/guardrex)和[Andrew Stanton 护士](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

查看或下载 2.x 的示例代码：

* [基本示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取到应用的密钥值。
* [密钥名称前缀示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取机密值使用密钥名称作为前缀表示版本的应用，这允许你加载一组不同的每个应用程序版本的机密值。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

查看或下载 1.x 的示例代码：

* [基本示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取到应用的密钥值。
* [密钥名称前缀示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x)([如何下载](xref:tutorials/index#how-to-download-a-sample))-读取机密值使用密钥名称作为前缀表示版本的应用，这允许你加载一组不同的每个应用程序版本的机密值。 

---

本文档说明如何使用[Microsoft Azure 密钥保管库](https://azure.microsoft.com/services/key-vault/)配置提供程序以从 Azure 密钥保管库机密加载应用程序配置值。 Azure 密钥保管库是一种基于云的服务，可帮助你保护加密密钥和机密由应用程序和服务。 常见方案包括控制对敏感的配置数据的访问和会议的必要条件 FIPS 140-2 级别 2 硬件安全模块 (HSM) 时验证存储配置数据。 此功能是可用的应用程序面向 ASP.NET 核心 1.1 或更高版本。

## <a name="package"></a>Package
若要使用的提供程序，添加到引用[Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/)包。

## <a name="application-configuration"></a>应用程序配置
你可以浏览的提供程序[示例应用](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples)。 一旦建立密钥保管库，并在保管库中创建机密，该示例应用将安全地加载到其配置的密钥值和在网页中显示它们。

提供程序添加到`ConfigurationBuilder`与`AddAzureKeyVault`扩展。 在示例应用中，则该扩展将从加载的三个配置值*appsettings.json*文件。

| 应用程序设置    | 描述                    | 示例                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure 密钥保管库名称           | contosovault                                 |
| `ClientId`     | Azure Active Directory App Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory App Key | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>创建密钥保管库密码和加载配置值 （basic 示例）
1. 创建密钥保管库并设置应用程序的指南的 Azure Active Directory (Azure AD)[开始使用 Azure 密钥保管库](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
  * 将机密添加到密钥保管库使用[AzureRM 密钥保管库 PowerShell 模块](/powershell/module/azurerm.keyvault)可从[PowerShell 库](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure 密钥保管库 REST API](/rest/api/keyvault/)，或[Azure 门户](https://portal.azure.com/)。 机密创建为*手动*或*证书*机密。 *证书*机密是使用应用程序和服务的证书，但不是支持配置提供程序。 应使用*手动*选项可创建带有配置提供程序使用的名称-值对机密。
    * 创建简单的机密的名称-值对。 Azure 密钥保管库密钥名称被限制为字母数字字符和短划线。
    * 分层值 （配置节） 使用`--`（两个短划线） 作为分隔符，在此示例。 通常用于分隔的子项中的一部分的冒号[ASP.NET 核心配置](xref:fundamentals/configuration/index)，机密名称中不允许出现。 因此，两个短划线的使用和机密加载到应用程序的配置时交换冒号。
    * 创建两个*手动*具有以下名称-值对的机密。 第一个密钥是一个简单的名称和值，并第二个密钥创建一个部分和子项中的密钥名称机密值：
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * 向 Azure Active Directory 注册示例应用程序。
  * 授权应用程序访问密钥保管库。 当你使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet 来授权应用程序访问密钥保管库，提供`List`和`Get`访问与机密`-PermissionsToSecrets list,get`。
2. 更新应用程序的*appsettings.json*的值的文件`Vault`， `ClientId`，和`ClientSecret`。
3. 运行示例应用程序，它可以通过获取其配置值从`IConfigurationRoot`机密的名称与同名。
  * 非分层值： 的值`SecretName`一起被获取`config["SecretName"]`。
  * 分层值 （节）： 使用`:`（冒号） 表示法或`GetSection`扩展方法。 使用这些方法之一获取配置值：
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

运行应用程序时，网页将显示加载的机密值：

![通过 Azure 密钥保管库配置提供程序加载浏览器窗口中显示密钥值](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>创建带前缀的密钥保管库密码和加载配置值 （密钥的名称的前缀的示例）
`AddAzureKeyVault` 此外提供了一个接受的实现重载`IKeyVaultSecretManager`，这样，你可以控制如何密钥保管库密码将转换为配置密钥。 例如，你可以实现接口后，可加载基于你在应用启动时提供的前缀值的密钥值。 这使你，例如，加载基于应用程序的版本的机密。

> [!WARNING]
> 不要使用前缀在密钥保管库密码放到同一个密钥保管库的多个应用的机密，或将环境机密 (例如，*开发*与*生产*机密) 入同一保管库。 我们建议，不同的应用程序和开发/生产环境使用单独的密钥保管库来隔离应用程序环境安全的最高级别。

使用第二个示例应用程序，为密钥保管库中创建一个机密`5000-AppSecret`（密钥保管库密钥名称中不允许句点） 表示你的应用程序的版本为 5.0.0.0 应用程序密钥。 有关另一个版本，5.1.0.0，创建密钥`5100-AppSecret`。 每个应用程序版本将自己机密的值加载到作为其配置`AppSecret`、 关闭版本剥离加载时为它的机密。 示例的实现如下所示：

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load`方法调用通过循环访问保管库密钥来查找那些具有版本前缀的提供程序算法。 当版本前缀找到具有`Load`，该算法使用`GetKey`方法返回的配置名称的机密的名称。 它剥离的机密的名称从版本前缀，并返回到应用程序的配置名称-值对的加载的机密名称的其余部分。

当您实现此方法：

1. 密钥保管库密码进行加载。
2. 字符串密钥`5000-AppSecret`匹配。
3. 版本， `5000` （替换为短划线），也会丢失从离开的键名称中移出`AppSecret`包含机密的值加载到应用程序的配置。

> [!NOTE]
> 你还可以提供你自己`KeyVaultClient`实现`AddAzureKeyVault`。 提供自定义客户端，可共享的配置提供程序和你的应用程序的其他部件之间的客户端的单个实例。

1. 创建密钥保管库并设置应用程序的指南的 Azure Active Directory (Azure AD)[开始使用 Azure 密钥保管库](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)。
  * 将机密添加到密钥保管库使用[AzureRM 密钥保管库 PowerShell 模块](/powershell/module/azurerm.keyvault)可从[PowerShell 库](https://www.powershellgallery.com/packages/AzureRM.KeyVault)、 [Azure 密钥保管库 REST API](/rest/api/keyvault/)，或[Azure 门户](https://portal.azure.com/)。 机密创建为*手动*或*证书*机密。 *证书*机密是使用应用程序和服务的证书，但不是支持配置提供程序。 应使用*手动*选项可创建带有配置提供程序使用的名称-值对机密。
    * 分层值 （配置节） 使用`--`（两个短划线） 作为分隔符。
    * 创建两个*手动*具有以下名称-值对的机密：
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * 向 Azure Active Directory 注册示例应用程序。
  * 授权应用程序访问密钥保管库。 当你使用`Set-AzureRmKeyVaultAccessPolicy`PowerShell cmdlet 来授权应用程序访问密钥保管库，提供`List`和`Get`访问与机密`-PermissionsToSecrets list,get`。
2. 更新应用程序的*appsettings.json*的值的文件`Vault`， `ClientId`，和`ClientSecret`。
3. 运行示例应用程序，它可以通过获取其配置值从`IConfigurationRoot`带前缀的机密名称与同名。 在此示例中，该前缀是应用程序的版本，它提供给`PrefixKeyVaultSecretManager`添加 Azure 密钥保管库配置提供程序时。 值`AppSecret`一起被获取`config["AppSecret"]`。 该应用程序生成的网页显示加载的值：

   ![通过 Azure 密钥保管库配置提供程序加载的浏览器窗口中显示一个机密的值，如果应用程序的版本，则 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. 更改中的项目文件中的应用程序集版本`5.0.0.0`到`5.1.0.0`并再次运行该应用。 此时，返回的机密值是`5.1.0.0_secret_value`。 该应用程序生成的网页显示加载的值：

   ![通过 Azure 密钥保管库配置提供程序加载的浏览器窗口中显示一个机密的值，如果应用程序的版本，则 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>控制对 ClientSecret 的访问
使用[机密管理器工具](xref:security/app-secrets)维护`ClientSecret`外部项目源树。 使用密钥管理器中，你将应用程序机密关联与特定项目并共享跨多个项目。

在开发时支持证书的环境中的.NET Framework 应用，可以使用 X.509 证书验证到 Azure 密钥保管库。 X.509 证书的私钥由操作系统管理。 有关详细信息，请参阅[使用而不是客户端密钥证书的身份验证](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret)。 使用`AddAzureKeyVault`接受的重载`X509Certificate2`。

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>重新加载机密
机密缓存直到`IConfigurationRoot.Reload()`调用。 过期，禁用，并且应用程序之前未遵从密钥保管库中的更新的机密`Reload`执行。

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>禁用和过期机密
禁用和过期机密引发`KeyVaultClientException`。 若要防止你的应用引发，替换你的应用程序或更新的过期已禁用/机密。

## <a name="troubleshooting"></a>疑难解答
当应用程序失败时加载使用提供程序的配置时，将一条错误消息写入到[ASP.NET 日志记录基础结构](xref:fundamentals/logging/index)。 以下条件将阻止从加载的配置：
* 应用程序未正确配置 Azure Active Directory 中。
* Azure 密钥保管库中不存在密钥保管库。
* 应用程序未授权访问密钥保管库。
* 访问策略不包括`Get`和`List`权限。
* 在密钥保管库，配置数据 （名称-值对） 被命名不正确，缺少，禁用，或已过期。
* 应用程序出现错误的密钥保管库名称 (`Vault`)，Azure AD 应用程序 Id (`ClientId`)，或 Azure AD 密钥 (`ClientSecret`)。
* Azure AD 密钥 (`ClientSecret`) 已过期。
* 配置密钥 （名称） 不正确的应用中的你正在试图加载的值。

## <a name="additional-resources"></a>其他资源
* [配置](xref:fundamentals/configuration/index)
* [Microsoft Azure： 密钥保管库](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure： 密钥保管库文档](https://docs.microsoft.com/azure/key-vault/)
* [如何生成和传输受 HSM 保护密钥的 Azure 密钥保管库](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient 类](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
