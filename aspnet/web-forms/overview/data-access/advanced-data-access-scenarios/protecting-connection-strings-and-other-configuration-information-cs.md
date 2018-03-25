---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: "保护连接字符串和其他配置信息 (C#) |Microsoft 文档"
author: rick-anderson
description: "ASP.NET 应用程序通常将配置信息存储在 Web.config 文件中。 此信息的某些敏感，保证保护。 通过 def。..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e3782e3d4acc2db0e744128dad64fdfae1e8766d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>保护连接字符串和其他配置信息 (C#)
====================
通过[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下载代码](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip)或[下载 PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> ASP.NET 应用程序通常将配置信息存储在 Web.config 文件中。 此信息的某些敏感，保证保护。 默认情况下将不会此文件提供给网站访问者，但管理员或黑客可以获得 Web 服务器的文件系统访问权限，查看文件的内容。 在本教程中我们了解 ASP.NET 2.0，我们可以通过在 Web.config 文件的节进行加密保护敏感信息。


## <a name="introduction"></a>介绍

ASP.NET 应用程序的配置信息通常存储在名为 XML 文件`Web.config`。 我们已更新过程中这些教程`Web.config`少量的次数。 在创建时`Northwind`中的类型化数据集[第一个教程](../introduction/creating-a-data-access-layer-cs.md)，例如，连接字符串信息已自动添加到`Web.config`中`<connectionStrings>`部分。 更高版本，在[母版页和网站的导航](../introduction/master-pages-and-site-navigation-cs.md)教程中，我们手动更新`Web.config`、 添加`<pages>`元素，该值指示所有项目中的 ASP.NET 页应使用`DataWebControls`主题。

由于`Web.config`可能包含敏感数据，例如连接字符串，很重要的内容`Web.config`安全和未经授权的查看者从隐藏保留。 默认情况下，任何 HTTP 请求的文件`.config`扩展由 ASP.NET 引擎，它返回*这种类型的页未被提供服务*图 1 所示的消息。 这意味着访问者不能查看你`Web.config`通过只需其浏览器的地址栏中输入 http://www.YourServer.com/Web.config 文件 s 内容。


[![访问 Web.config 通过浏览器返回此类型的页不提供消息](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**图 1**： 访问`Web.config`通过浏览器返回此类型的页不提供消息 ([单击以查看实际尺寸的图像](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


但如果攻击者能够找到允许她查看一些其他攻击你`Web.config`文件 s 内容？ 哪些可以攻击者使用此信息，以及可以执行的步骤来进一步保护内的敏感信息`Web.config`？ 幸运的是，大多数部分中`Web.config`不包含敏感信息。 哪些损害可以攻击者 perpetrate 如果他们知道默认使用 ASP.NET 页面主题的名称？

某些`Web.config`部分中，但是，包含可能包括连接字符串、 用户名、 密码、 服务器名称、 加密密钥和等的敏感信息。 在下面的示例通常找到此信息`Web.config`各节：

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

在本教程中我们将查看用于保护此类敏感的配置信息的技术。 我们将会看到，.NET Framework 2.0 版包括受保护的配置系统，可轻松的以编程方式加密和解密所选的配置节。

> [!NOTE]
> 本教程到结束看一下用于从 ASP.NET 应用程序连接到数据库的 Microsoft 的建议。 除了加密连接字符串，可以帮助增强安全性，以便通过确保你要连接到以安全方式中的数据库的系统。


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>步骤 1： 浏览 ASP.NET 2.0 s 受保护的配置选项

ASP.NET 2.0 包含一个受保护的配置系统，用于加密和解密配置信息。 这可以用于以编程方式加密或解密配置信息的.NET Framework 中包括的方法。 受保护的配置系统使用[提供程序模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，它允许开发人员选择使用什么加密的实现。

.NET Framework 附带两个受保护的配置提供程序：

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)-使用非对称[RSA 算法](http://en.wikipedia.org/wiki/Rsa)加密和解密。
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-使用 Windows[数据保护 API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)加密和解密。

由于受保护的配置系统实现的提供程序设计模式，它是可以创建自己的受保护的配置提供程序并将其插入你的应用程序。 请参阅[实现保护配置提供程序](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx)有关此过程的详细信息。

在 RSA 和 DPAPI 提供程序对其加密和解密的例程，使用键和这些密钥可存储在计算机或用户的级别。 计算机级密钥是 web 应用程序在其自己的专用服务器的运行所在的方案的理想选择，或如果有多个应用程序需要共享的服务器上加密的信息。 用户级密钥是在其中同一服务器上的其他应用程序不应能够解密你的应用程序受保护的 s 配置节的共享宿主环境中更加安全的选项。

在本教程中我们的示例将使用 DPAPI 提供程序和计算机级密钥。 具体而言，我们将着眼于加密`<connectionStrings>`主题中`Web.config`，但受保护的配置系统可以用于加密大多数任何`Web.config`部分。 在使用用户级密钥或使用 RSA 提供程序的信息，请在本教程末尾查阅进一步读数部分中的资源。

> [!NOTE]
> `RSAProtectedConfigurationProvider`和`DPAPIProtectedConfigurationProvider`中注册提供程序`machine.config`替换为提供程序的名称的文件`RsaProtectedConfigurationProvider`和`DataProtectionConfigurationProvider`分别。 进行加密或解密我们将需要提供相应的提供程序名称的配置信息时 (`RsaProtectedConfigurationProvider`或`DataProtectionConfigurationProvider`) 而不是实际类型名称 (`RSAProtectedConfigurationProvider`和`DPAPIProtectedConfigurationProvider`)。 你可以找到`machine.config`文件中`$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`文件夹。


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>步骤 2： 以编程方式加密和解密配置节

使用几行代码中，我们可以加密或解密使用指定的提供程序特定的配置节。 该代码，我们很快将会看到只是需要以编程方式引用相应的配置部分中，调用其`ProtectSection`或`UnprotectSection`方法，，然后调用`Save`方法以保留所做的更改。 此外，.NET Framework 包括一有用的命令行实用工具，可以加密和解密配置信息。 我们将探讨在步骤 3 中的该命令行实用程序。

若要说明以编程方式保护的配置信息，让 s 创建 ASP.NET 页，其中包含用于加密和解密的按钮`<connectionStrings>`主题中`Web.config`。

首先打开`EncryptingConfigSections.aspx`页面`AdvancedDAL`文件夹。 将 TextBox 控件从工具箱中拖动到设计器中，设置其`ID`属性`WebConfigContents`，将其`TextMode`属性`MultiLine`，并将其`Width`和`Rows`为 95%和 15 之间，属性分别。 此文本框控件将显示的内容`Web.config`使我们能够快速查看是否对内容进行加密。 当然，在实际应用中你将永远不会想要显示的内容`Web.config`。

在文本框中，下面添加两个名为的按钮控件`EncryptConnStrings`和`DecryptConnStrings`。 将其文本属性设置为加密连接字符串和解密连接字符串。

此时你的屏幕应类似于图 2。


[![向页面添加一个文本框和两个按钮 Web 控件](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**图 2**： 向页面添加一个文本框和两个按钮 Web 控件 ([单击以查看实际尺寸的图像](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


接下来，我们需要编写代码来加载和显示的内容`Web.config`中`WebConfigContents`文本框中第一个页面时加载。 将以下代码添加到页的代码隐藏类。 此代码将添加一个名为方法`DisplayWebConfig`和调用它从`Page_Load`事件处理程序时`Page.IsPostBack`是`false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig`方法使用[`File`类](https://msdn.microsoft.com/library/system.io.file.aspx)以打开应用程序 s`Web.config`文件， [ `StreamReader`类](https://msdn.microsoft.com/library/system.io.streamreader.aspx)其内容读入一个字符串和[`Path`类](https://msdn.microsoft.com/library/system.io.path.aspx)生成到的物理路径`Web.config`文件。 这三个类在中找到[`System.IO`命名空间](https://msdn.microsoft.com/library/system.io.aspx)。 因此，你将需要添加`using``System.IO`语句的代码隐藏类或，或者，这些类具有的名称的前缀页首`System.IO.`。

接下来，我们需要添加两个按钮控件的事件处理程序`Click`事件并添加必要的代码来加密和解密`<connectionStrings>`部分使用 DPAPI 提供程序的计算机级密钥。 从设计器中，双击每个要添加的按钮`Click`的代码隐藏文件中的事件处理程序类，然后添加以下代码：


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

在两个事件处理程序中使用的代码是几乎完全相同。 它们都入手，获取有关当前的应用程序 s 信息`Web.config`文件通过[`WebConfigurationManager`类](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx)s [ `OpenWebConfiguration`方法](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)。 此方法返回指定的虚拟路径的 web 配置文件。 接下来，`Web.config`文件 s`<connectionStrings>`部分访问通过[`Configuration`类](https://msdn.microsoft.com/library/system.configuration.configuration.aspx)s [ `GetSection(sectionName)`方法](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)，它将返回[ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx)对象。

`ConfigurationSection`对象包括[`SectionInformation`属性](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx)，提供其他信息和有关的配置节的功能。 作为上面所示的代码，我们可以确定是否通过检查加密配置节`SectionInformation`属性的`IsProtected`属性。 此外，加密或解密通过明`SectionInformation`属性 s`ProtectSection(provider)`和`UnprotectSection`方法。

`ProtectSection(provider)`方法接受输入作为一个字符串，指定要在加密时使用的受保护的配置提供程序的名称。 在`EncryptConnString`按钮的事件处理程序，我们将传递到 DataProtectionConfigurationProvider`ProtectSection(provider)`方法，以便使用 DPAPI 提供程序。 `UnprotectSection`方法可以确定的提供程序用于加密的配置节，因此不需要任何输入参数。

在调用`ProtectSection(provider)`或`UnprotectSection`方法时，必须调用`Configuration`对象 s [ `Save`方法](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx)以保留更改。 已加密或解密的配置信息并保存更改，我们调用后`DisplayWebConfig`加载更新`Web.config`到文本框控件的内容。

输入上面的代码后, 对其进行测试访问`EncryptingConfigSections.aspx`通过浏览器的页。 你最初应看到列出的内容的页面`Web.config`与`<connectionStrings>`以纯文本形式显示的部分 （请参见图 3）。


[![向页面添加一个文本框和两个按钮 Web 控件](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**图 3**： 向页面添加一个文本框和两个按钮 Web 控件 ([单击以查看实际尺寸的图像](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


现在，单击加密连接字符串按钮。 如果启用请求验证，则标记发布从`WebConfigContents`文本框中将生成`HttpRequestValidationException`，它会显示消息，具有潜在危险`Request.Form`从客户端检测到值。 请求验证，默认情况下，ASP.NET 2.0 中启用，它禁止将包括未编码 HTML 的回发，旨在帮助防止脚本注入攻击。 在页面或应用程序级别，可以禁用此检查。 若要将其关闭此页，设置`ValidateRequest`将设置为`false`中`@Page`指令。 `@Page`指令找到页面 s 声明性标记顶部。


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

有关详细信息请求验证，其用途，如何禁用它在页级和应用程序级，如以及如何为 HTML 编码标记，请参阅[请求验证-防止脚本攻击](../../../../whitepapers/request-validation.md)。

禁用之后请求验证页，请尝试再次单击加密连接字符串按钮。 将回发时，访问该配置文件并将其`<connectionStrings>`加密使用 DPAPI 提供程序的部分。 文本框中然后更新以显示新`Web.config`内容。 如图 4 所示，`<connectionStrings>`现在加密信息。


[![单击加密连接字符串按钮加密&lt;connectionString&gt;部分](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**图 4**： 单击加密连接字符串按钮加密`<connectionString>`部分 ([单击以查看实际尺寸的图像](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


加密`<connectionStrings>`在我的计算机上生成的部分遵循，尽管中的内容的某些`<CipherData>`为简便起见已移除元素：


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>`元素指定用于执行加密的提供程序 (`DataProtectionConfigurationProvider`)。 通过使用此信息`UnprotectSection`按钮解密连接字符串的方法。


当从访问连接字符串信息时`Web.config`，无论是由代码中，我们编写，从 SqlDataSource 控件或从 Tableadapter 中我们类型化数据集的自动生成的代码-是自动解密。 简单地说，我们不需要添加任何额外代码或逻辑进行解密的加密`<connectionString>`部分。 若要进行此演示，请访问早期教程之一在此时间，例如从基本报告部分的简单显示教程 (`~/BasicReporting/SimpleDisplay.aspx`)。 如图 5 所示，本教程适用完全如我们所料，，该值指示，自动正在加密的连接字符串信息解密了 ASP.NET 页。


[![数据访问层自动解密的连接字符串信息](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**图 5**： 数据访问层自动解密连接字符串信息 ([单击以查看实际尺寸的图像](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


若要还原`<connectionStrings>`部分回其纯文本表示形式中，单击解密连接字符串按钮。 在回发时，你应看到中的连接字符串`Web.config`以纯文本。 此时像第一次访问此页 （请参见图 3 中） 时，应查看你的屏幕。

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>步骤 3： 加密使用的配置节`aspnet_regiis.exe`

.NET Framework 包括中的命令行工具的各种`$WINDOWS$\Microsoft.NET\Framework\version\`文件夹。 在[使用 SQL 缓存依赖项](../caching-data/using-sql-cache-dependencies-cs.md)教程中，例如，我们看使用`aspnet_regsql.exe`命令行工具，用于添加 SQL 缓存依赖项所需的基础结构。 此文件夹中的另一个有用的命令行工具是[ASP.NET IIS 注册工具 (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)。 顾名思义，ASP.NET IIS 注册工具主要用于向 Microsoft 的专业级 Web 服务器，IIS 注册 ASP.NET 2.0 应用程序。 除了其与 IIS 相关的功能，ASP.NET IIS 注册工具还可用于加密或解密中的指定的配置节`Web.config`。

以下语句显示用来加密配置节的常规语法`aspnet_regiis.exe`命令行工具：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*部分*是加密 （如 connectionStrings) 的配置节*物理\_目录*是 web 应用程序的根目录的完整的物理路径和*提供程序*是受保护的配置提供程序 （如 DataProtectionConfigurationProvider) 上使用的名称。 或者，如果在 IIS 中注册 web 应用程序可以输入而不是使用以下语法的物理路径的虚拟路径：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

以下`aspnet_regiis.exe`示例对进行加密`<connectionStrings>`部分使用 DPAPI 提供程序和计算机级密钥：


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

同样，`aspnet_regiis.exe`命令行工具可用于解密配置节。 而不是使用`-pef`切换，请使用`-pdf`(或代替`-pe`，使用`-pd`)。 另请注意，提供程序名称时不需要解密。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> 因为我们要使用 DPAPI 提供程序，后者使用特定于的计算机的键时，必须运行`aspnet_regiis.exe`来自相同的计算机正在提供 web 页。 例如，如果从本地开发计算机上运行此命令行程序，然后将加密的 Web.config 文件上载到生产服务器时，生产服务器将不能进行解密的连接字符串信息，因为它已加密使用特定于你的开发计算机的密钥。 RSA 提供程序不具有此限制，因为它是可以将 RSA 密钥导出到另一台计算机。


## <a name="understanding-database-authentication-options"></a>了解数据库身份验证选项

任何应用程序可以发出之前`SELECT`， `INSERT`， `UPDATE`，或`DELETE`到 Microsoft SQL Server 数据库，数据库查询首先必须标识请求者。 此过程被称为*身份验证*和 SQL Server 提供的身份验证的两种方法：

- **Windows 身份验证**-在其下运行应用程序的过程用于与数据库通信。 当运行时通过 Visual Studio 2005 的 ASP.NET Development Server ASP.NET 应用程序，则 ASP.NET 应用程序假定当前登录用户的标识。 对于 ASP.NET 应用程序在 Microsoft Internet 信息服务器 (IIS)，ASP.NET 应用程序通常采用的标识`domainName``\MachineName`或`domainName``\NETWORK SERVICE`，但这可以自定义。
- **SQL 身份验证**-作为身份验证的凭据提供用户 ID 和密码值。 使用 SQL 身份验证连接字符串中提供的用户 ID 和密码。

Windows 身份验证通过 SQL 身份验证中都是首选的因为它是更安全。 Windows 身份验证的连接字符串是空闲的用户名和密码，而如果 web 服务器和数据库服务器驻留在两台不同计算机上，通过纯文本中的网络不发送凭据。 使用 SQL 身份验证，但是，身份验证凭据硬编码在连接字符串中并且将从 web 服务器传输到纯文本中的数据库服务器。

这些教程已使用 Windows 身份验证。 你可以判断检查连接字符串正在使用的身份验证模式。 中的连接字符串`Web.config`已被为我们的教程：

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

集成安全性 = True 和缺乏用户名和密码将指示正在使用 Windows 身份验证。 在某些连接字符串术语受信任连接 = 是或集成的安全性 = SSPI 使用而不是集成安全性 = True，但所有三个指示使用 Windows 身份验证。

下面的示例演示一个使用 SQL 身份验证的连接字符串。 请注意在连接字符串内嵌入的凭据：

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

假设攻击是能够查看你的应用程序 s`Web.config`文件。 如果你使用 SQL 身份验证连接到的数据库，则可以通过 Internet 访问，攻击者可以使用此连接字符串以连接到数据库通过 SQL Management Studio，或从其自己的网站上的 ASP.NET 页。 为了帮助减轻此威胁，加密中的连接字符串信息`Web.config`使用受保护的配置系统。

> [!NOTE]
> 关于 SQL Server 中提供的身份验证的不同类型的详细信息，请参阅[生成安全 ASP.NET 应用程序： 身份验证、 授权和安全通信](https://msdn.microsoft.com/library/aa302392.aspx)。 有关其他连接字符串示例演示 Windows 和 SQL 身份验证语法之间的差异，请参阅[ConnectionStrings.com](http://www.connectionstrings.com/)。


## <a name="summary"></a>摘要

默认情况下，文件都具有`.config`无法通过浏览器访问 ASP.NET 应用程序中的扩展。 这些类型的文件不会返回，因为它们可能包含敏感信息，如数据库连接字符串、 用户名和密码，依次类推。 .NET 2.0 中受保护的配置系统可帮助进一步保护通过允许指定的配置节进行加密的敏感信息。 有两个内置的受保护的配置提供程序： 另一个使用 RSA 算法，另一个使用 Windows 数据保护 API (DPAPI)。

在本教程中我们介绍了如何加密和解密使用 DPAPI 提供程序的配置设置。 这可以实现这两种以编程方式，正如我们在步骤 2 中看到，以及通过`aspnet_regiis.exe`步骤 3 中所涉及的命令行工具。 使用用户级密钥或改为使用 RSA 提供程序的详细信息，请参阅其他阅读材料部分中的资源。

尽情享受编程 ！

## <a name="further-reading"></a>其他阅读材料

在本教程中讨论的主题的详细信息，请参阅以下资源：

- [生成安全的 ASP.NET 应用程序： 身份验证、 授权和安全通信](https://msdn.microsoft.com/library/aa302392.aspx)
- [加密 ASP.NET 2.0 中的配置信息的应用程序](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [加密`Web.config`ASP.NET 2.0 中的值](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [如何： 加密 ASP.NET 2.0 中的配置节使用 DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [如何： 加密 ASP.NET 2.0 中的配置节，可使用 RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 中的配置 API](http://www.odetocode.com/Articles/418.aspx)
- [Windows 数据保护](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>关于作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七个 ASP/ASP.NET 书籍和的创始人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年使用与 Microsoft Web 技术。 Scott 的作用是作为独立的顾问、 培训师和编写器。 最新书籍是[ *Sam 教授自己 ASP.NET 2.0 24 小时内*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以达到在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或通过他的博客，其中可以找到在[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特别感谢

本教程系列已由许多有用的审阅者评审。 本教程中的前导审阅者已 Teresa 墨和徐 Schmidt。 对感兴趣查看我即将到来的 MSDN 文章？ 如果是这样，删除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一页](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
[下一页](debugging-stored-procedures-cs.md)
