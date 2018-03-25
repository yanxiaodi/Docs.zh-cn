# <a name="key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>密钥保管库配置提供程序示例应用程序 (ASP.NET Core 2.x)

此示例演示如何使用 Azure 密钥保管库配置提供程序的 ASP.NET Core 2.x。 有关 ASP.NET 核心 1.x 示例，请参阅[密钥保管库配置提供程序示例应用程序 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x)。

此示例的工作原理的详细信息，请参阅[Azure 密钥保管库配置提供程序](xref:security/key-vault-configuration)主题。

## <a name="using-the-sample"></a>使用示例
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
