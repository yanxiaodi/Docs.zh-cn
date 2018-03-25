---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: "配置文件、 主题和 Web 部件 |Microsoft 文档"
author: microsoft
description: "有配置中的重大更改和 ASP.NET 2.0 中的检测。 使用新的 ASP.NET 配置 API，配置更改进行 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 2c6ba11799a5a9be3d8c0037fad5d79d8177c0e8
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2018
---
<a name="profiles-themes-and-web-parts"></a>配置文件、 主题和 Web 部件
====================
by [Microsoft](https://github.com/microsoft)

> 有配置中的重大更改和 ASP.NET 2.0 中的检测。 新的 ASP.NET 配置 API 允许以编程方式进行配置更改。 此外，许多新的配置设置存在允许对新的配置和检测。


ASP.NET 2.0 表示大大改进的区域中的个性化网站。 除了已包括成员资格功能我们，ASP.NET 配置文件、 主题和 Web 部件显著提高在网站中的个性化设置。

## <a name="aspnet-profiles"></a>ASP.NET 配置文件

ASP.NET 配置文件是类似于会话。 不同之处在于，配置文件是永久性的而关闭浏览器时，会话将丢失。 会话和配置文件之间的另一个大的区别是，配置文件都强类型，因此你提供 IntelliSense 在开发过程中。

计算机配置文件或应用程序的 web.config 文件中定义了一个配置文件。 （不能配置文件中定义的子文件夹 web.config 文件。）下面的代码定义要存储的网站访客首先和姓氏的配置文件。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

配置文件属性的默认数据类型为 System.String。 在上述示例中，不指定任何数据类型。 因此 FirstName 和 LastName 属性都属于字符串类型。 如前所述，强类型属性的配置文件。 下面的代码将新属性添加的类型 Int32 的期限。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

配置文件通常用于 ASP.NET 窗体身份验证。 每个用户时与窗体身份验证结合使用，具有单独的配置文件与其用户 id。 但是，还有可能允许在匿名应用程序中使用的配置文件使用&lt;anonymousIdentification&gt;以及配置文件中的元素**allowAnonymous**属性为下面所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

当匿名用户浏览站点时，ASP.NET 创建的实例**ProfileCommon**用户。 此配置文件使用的浏览器上的 cookie 中存储的唯一 ID 来将用户标识为唯一访客。 这种方式，你可以存储用户的匿名浏览的配置文件的信息。

## <a name="profile-groups"></a>配置文件组

它是可能的配置文件的组属性。 通过分组属性，则可以模拟的特定应用程序的多个配置文件。

以下配置将两个组; FirstName 和 LastName 属性购买者和潜在客户。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

然后可对特定组中，如下所示设置属性：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>存储的复杂对象

到目前为止，我们已介绍的示例将存储在一个配置文件的简单数据类型。 还有可能通过指定序列化使用的方法在一个配置文件中存储复杂数据类型**serializeAs**属性，如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

在这种情况下，类型是 PurchaseInvoice。 PurchaseInvoice 类需要将标记为可序列化，并且可以包含任意数量的属性。 例如，如果 PurchaseInvoice 具有一个名为属性**NumItemsPurchased**，可以对代码中的该属性引用，如下所示：

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>配置文件继承

它是可以创建多个应用程序中使用的配置文件。 通过创建派生自 ProfileBase 的配置文件类，则可以通过重用多个应用程序中的配置文件**继承**属性如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

在此情况下，类**PurchasingProfile**如下如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>配置文件提供程序

ASP.NET 配置文件使用提供程序模型。 默认的提供程序将信息存储在应用程序中的 SQL Server Express 数据库\_使用 SqlProfileProvider 提供程序的 Web 应用程序数据文件夹。 如果数据库不存在，ASP.NET 将自动创建它时配置文件尝试存储的信息。

但是，在某些情况下，你可能要开发自己的配置文件提供程序。 ASP.NET 配置文件功能使你能够轻松使用不同的提供程序。

创建自定义配置文件提供程序时：

- 你需要将配置文件信息存储在数据源，如在 FoxPro 数据库或 Oracle 数据库中，程序不支持.NET Framework 附带的配置文件提供程序。
- 你需要管理使用的是不同于使用.NET Framework 附带的提供程序的数据库架构的数据库架构的配置文件信息。 常见示例是你想要与现有 SQL Server 数据库中的用户数据集成配置文件信息。

### <a name="required-classes"></a>所需的类

若要实现配置文件提供程序，你可以创建继承 System.Web.Profile.ProfileProvider 抽象类的类。 **ProfileProvider**抽象类反过来继承 System.Configuration.SettingsProvider 抽象类，该类继承 System.Configuration.Provider.ProviderBase 抽象类。 由于此继承链，除了的所需成员**ProfileProvider**类，则必须实现的所需的成员**SettingsProvider**和**ProviderBase**类。

下表描述的属性和方法，则必须实现从**ProviderBase**， **SettingsProvider**，和**ProfileProvider**抽象类。

### <a name="providerbase-members"></a>ProviderBase 成员

| **成员** | **说明** |
| --- | --- |
| Initialize 方法 | 作为输入提供程序实例的名称和配置设置的 NameValueCollection 利用。 用于设置选项和提供程序实例，包括特定于实现的值和在计算机配置或 Web.config 文件中指定的选项的属性值。 |

### <a name="settingsprovider-members"></a>SettingsProvider 成员

| **成员** | **说明** |
| --- | --- |
| 应用程序名称属性 | 每个配置文件存储应用程序名称。 配置文件提供程序使用的应用程序名称来存储单独为每个应用程序的配置文件信息。 这使多个 ASP.NET 应用程序使用相同的数据源不发生冲突，如果在不同的应用程序中创建相同的用户名。 或者，多个 ASP.NET 应用程序可以通过指定相同的应用程序名称共享配置文件数据源。 |
| GetPropertyValues 方法 | 将作为输入 SettingsContext 和 SettingsPropertyCollection 对象。 **SettingsContext**提供有关用户的信息。 可以使用为主键的信息来检索用户的配置文件属性信息。 使用**SettingsContext**要获取的用户名称以及用户是否是经过身份验证或匿名对象。 **SettingsPropertyCollection**包含 SettingsProperty 对象的集合。 每个**SettingsProperty**对象提供的名称和类型的属性以及其他信息，例如的默认值为该属性并且属性是只读的。 **GetPropertyValues**方法基于 SettingsPropertyValue 对象，并用其填充 SettingsPropertyValueCollection **SettingsProperty**作为输入提供的对象。 每个指定的用户数据源中的值分配给 PropertyValue 属性**SettingsPropertyValue**返回对象和整个集合。 调用该方法还更新为当前日期和时间的指定的用户配置文件的 LastActivityDate 值。 |
| SetPropertyValues 方法 | 接受的输入**SettingsContext**和**SettingsPropertyValueCollection**对象。 **SettingsContext**提供有关用户的信息。 可以使用为主键的信息来检索用户的配置文件属性信息。 使用**SettingsContext**要获取的用户名称以及用户是否是经过身份验证或匿名对象。 **SettingsPropertyValueCollection**包含一套**SettingsPropertyValue**对象。 每个**SettingsPropertyValue**对象提供名称、 类型和值的属性以及其他信息，例如的默认值为该属性并且属性是只读的。 **SetPropertyValues**方法将更新指定的用户的数据源中的配置文件属性值。 调用此方法还更新**LastActivityDate** LastUpdatedDate 的和值为当前日期和时间的指定的用户配置文件。 |

### <a name="profileprovider-members"></a>ProfileProvider 成员

| **成员** | **说明** |
| --- | --- |
| DeleteProfiles 方法 | 使用作为输入的用户的字符串数组名称和从数据源的指定名称的所有配置文件信息和属性值中删除其中的应用程序名称匹配**ApplicationName**属性值。 如果您的数据源支持事务，建议在事务中包括所有的删除操作，并且您回滚事务，任何删除操作失败时引发异常。 |
| DeleteProfiles 方法 | 使用作为输入的 ProfileInfo 集合对象，并从数据源的每个配置文件的所有配置文件信息和属性值中删除其中的应用程序名称匹配**ApplicationName**属性值。 如果您的数据源支持事务，建议你在事务中包含所有的删除操作并回滚事务，以及任何删除操作失败时引发异常。 |
| DeleteInactiveProfiles 方法 | 接受的输入 ProfileAuthenticationOption 值和 DateTime 对象和删除操作从数据源配置文件的所有信息和属性值上一次活动日期是否小于或等于指定的日期和时间以及在其中应用程序名称匹配**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，或所有配置文件将被删除。 如果您的数据源支持事务，建议你在事务中包含所有的删除操作并回滚事务，以及任何删除操作失败时引发异常。 |
| GetAllProfiles 方法 | 接受的输入**ProfileAuthenticationOption**值、 一个整数，指定的页索引、 一个整数，指定页大小，以及对一个整数，它将设置为配置文件的总计数的引用。 返回包含 ProfileInfoCollection **ProfileInfo**对于其中的应用程序名称匹配的数据源中的所有配置文件对象**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，还是要返回所有配置文件。 返回的结果**GetAllProfiles**方法受到的页索引和页大小的值。 该页面大小值指定的最大数**ProfileInfo**对象中返回**ProfileInfoCollection**。 页索引值指定的结果返回，其中 1 标识的第一页的页。 总记录数的参数是一个 out 参数 (你可以使用**ByRef**在 Visual Basic 中) 是否设置为配置文件的总数。 例如，如果数据存储包含应用程序的 13 个配置文件和页索引值为 2，5，页面大小**ProfileInfoCollection**返回包含通过第 10 个配置文件的第六个。 此方法返回时，总记录数的值设置为 13 中。 |
| GetAllInactiveProfiles 方法 | 接受的输入**ProfileAuthenticationOption**值， **DateTime**对象、 一个整数，指定的页索引、 一个整数，指定页大小，以及对一个整数，它将设置的引用到配置文件的总计数。 返回**ProfileInfoCollection**包含**ProfileInfo**为上一次活动日期小于或等于指定的数据源中的所有配置文件对象**DateTime**和其中的应用程序名称匹配**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，还是要返回所有配置文件。 返回的结果**GetAllInactiveProfiles**方法受到的页索引和页大小的值。 该页面大小值指定的最大数**ProfileInfo**对象中返回**ProfileInfoCollection**。 页索引值指定的结果返回，其中 1 标识的第一页的页。 总记录数的参数是一个 out 参数 (你可以使用**ByRef**在 Visual Basic 中) 是否设置为配置文件的总数。 例如，如果数据存储包含应用程序的 13 个配置文件和页索引值为 2，5，页面大小**ProfileInfoCollection**返回包含通过第 10 个配置文件的第六个。 此方法返回时，总记录数的值设置为 13 中。 |
| FindProfilesByUserName 方法 | 接受的输入**ProfileAuthenticationOption**值，包含用户名称、 一个整数，指定的页索引、 一个整数，指定页大小，以及对一个整数，它将设置为的总计数的引用的字符串配置文件。 返回**ProfileInfoCollection**包含**ProfileInfo**对象的数据中的所有配置文件的源的用户名与指定的用户名匹配之处和其中的应用程序名称匹配**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，还是要返回所有配置文件。 如果您的数据源支持附加搜索功能，如通配符字符，你可以为用户名称提供更广泛搜索功能。 返回的结果**FindProfilesByUserName**方法受到的页索引和页大小的值。 该页面大小值指定的最大数**ProfileInfo**对象中返回**ProfileInfoCollection**。 页索引值指定的结果返回，其中 1 标识的第一页的页。 总记录数的参数是一个 out 参数 (你可以使用**ByRef**在 Visual Basic 中) 是否设置为配置文件的总数。 例如，如果数据存储包含应用程序的 13 个配置文件和页索引值为 2，5，页面大小**ProfileInfoCollection**返回包含通过第 10 个配置文件的第六个。 此方法返回时，总记录数的值设置为 13 中。 |
| FindInactiveProfilesByUserName 方法 | 接受的输入**ProfileAuthenticationOption**值、 一个包含用户名，字符串**DateTime**对象、 一个整数，指定的页索引、 一个整数，指定页大小和引用一个整数，它将设置为配置文件的总计数。 返回**ProfileInfoCollection**包含**ProfileInfo**对于其中的用户名匹配指定的用户名称，其中是上一次活动日期数据源中的所有配置文件对象小于或等于指定**DateTime**，和其中的应用程序名称匹配**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，还是要返回所有配置文件。 如果您的数据源支持附加搜索功能，如通配符字符，你可以为用户名称提供更广泛搜索功能。 返回的结果**FindInactiveProfilesByUserName**方法受到的页索引和页大小的值。 该页面大小值指定的最大数**ProfileInfo**对象中返回**ProfileInfoCollection**。 页索引值指定的结果返回，其中 1 标识的第一页的页。 总记录数的参数是一个 out 参数 (你可以使用**ByRef**在 Visual Basic 中) 是否设置为配置文件的总数。 例如，如果数据存储包含应用程序的 13 个配置文件和页索引值为 2，5，页面大小**ProfileInfoCollection**返回包含通过第 10 个配置文件的第六个。 此方法返回时，总记录数的值设置为 13 中。 |
| GetNumberOfInActiveProfiles method | 接受的输入**ProfileAuthenticationOption**值和**DateTime**对象并返回上一次活动日期其中是小于或等于指定数据源中的所有配置文件计数**DateTime**和其中的应用程序名称匹配**ApplicationName**属性值。 **ProfileAuthenticationOption**参数指定是否仅匿名配置文件，仅当身份验证配置文件，或所有配置文件是要计数。 |

### <a name="applicationname"></a>ApplicationName

由于配置文件提供程序存储单独为每个应用程序的配置文件信息，你必须确保你数据的架构，包括应用程序名称，并确保查询和更新也包含应用程序名称。 例如，以下命令可用于从基于用户名称和配置文件是否是匿名的数据库中检索属性值，并确保**ApplicationName**查询中包含值。

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET 主题

## <a name="what-are-aspnet-20-themes"></a>ASP.NET 2.0 主题有哪些？

Web 应用程序的最重要方面之一站点内是一致的外观和感觉。 ASP.NET 1.x 开发人员通常使用级联样式表 (CSS) 来实现一致的外观和感觉。 ASP.NET 2.0 主题显著改进了 CSS，因此 ASP.NET 开发人员可定义 ASP.NET 服务器控件，以及 HTML 元素的外观的功能。 ASP.NET 主题可以应用于单独的控件、 特定的 Web 页上或整个 Web 应用程序。 如果需要映像，主题使用 CSS 文件、 一个可选外观文件和一个可选的映像目录的组合。 外观文件控制 ASP.NET 服务器控件的可视外观。

## <a name="where-are-themes-stored"></a>在哪里？ 主题存储

主题的存储位置的位置不同根据其作用域。 可以应用于任何应用程序的主题存储在以下文件夹：

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

特定于特定应用程序的主题存储在`App\_Themes\<Theme\_Name>`目录 Web 站点的根目录中。

> [!NOTE]
> 外观文件仅应修改会影响外观的服务器控件属性。

全局主题是可以应用于任何应用程序或 Web 服务器上运行的网站的主题。 默认情况下，是在 v2.x.xxxxx 目录内部的 ASP.NETClientfiles\Themes 目录中存储这些主题。 或者，你可以将主题文件移到 aspnet\_客户端/系统\_web / [version] /Themes/ [主题\_名称] 您的网站的根目录中的文件夹。

特定于应用程序的主题可以仅应用于文件驻留在其中应用程序中。 这些文件存储在`App\_Themes/<theme\_name>`目录 Web 站点的根目录中。

## <a name="the-components-of-a-theme"></a>主题的组件

主题由一个或多个 CSS 文件、 一个可选外观文件和可选的映像文件夹组成。 CSS 文件可以是任何名称，你想 （即 default.css 或 theme.css 等），并且必须是主题文件夹的根目录中。 CSS 文件用于定义普通 CSS 类和特定的选择器的属性。 将其中一个 CSS 类应用到页元素， **CSSClass**使用属性。

外观文件是一个包含 ASP.NET 服务器控件的属性定义的 XML 文件。 下面列出的代码是一个示例外观文件。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**图 1**较小的 ASP.NET 页面浏览而无需应用了主题下面所示。 **图 2**应用了主题中显示的同一文件。 通过 CSS 文件配置的背景色和文本颜色。 使用上面列出的外观文件进行配置的按钮和文本框中的外观。


![无主题](profiles-themes-and-web-parts/_static/image1.gif)

**图 1**： 无主题


![应用主题](profiles-themes-and-web-parts/_static/image2.gif)

**图 2**： 应用的主题


上面列出的外观文件定义所有文本框控件和按钮控件的默认外观。 这意味着，每一个在页面上插入的按钮控件和文本框控件将采用此外观。 你还可以定义可应用于这些控件使用的特定实例的外观**SkinID**的控件属性。

下面的代码定义一个按钮控件的外观。 仅按钮控件**SkinID**属性**转**外观的外观将不执行。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

只能有一个默认外观，每个服务器控件类型。 如果需要其他的外观，你应使用 SkinID 属性。

## <a name="applying-themes-to-pages"></a>应用主题到网页

可以使用以下方法之一来应用一个主题：

- 在&lt;页&gt;web.config 文件的元素
- 在@Page页面的指令
- 以编程方式

## <a name="applying-a-theme-in-the-configuration-file"></a>应用配置文件中的主题

若要应用应用程序配置文件中的主题，请使用以下语法：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

此处指定的主题名称必须匹配的主题文件夹的名称。 此文件夹可以存在的任何一种方法在本课程前面提到的位置。 如果你尝试将不存在的主题应用，将发生配置错误。

## <a name="applying-a-theme-in-the-page-directive"></a>应用页面指令中的主题

你还可以应用 @ 页面指令中的主题。 此方法，可使用特定页面的主题。

要应用中的主题@Page指令，使用以下语法：

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

同样，如前所述此处指定的主题必须与匹配的主题文件夹。 如果你尝试将不存在的主题应用，将发生生成失败。 Visual Studio 还将突出显示该属性，并通知你存在任何此类主题。

## <a name="applying-a-theme-programmatically"></a>以编程方式应用主题

若要以编程方式将主题应用，你必须指定**主题**属性中的页**页\_PreInit**方法。

若要以编程方式将主题应用，使用以下语法：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

有必要将 PreInit 方法，因为页面生命周期中的主题。 如果你将其应用自该时间之后，页面主题将包含已应用由运行时和更改在该点为时已晚生命周期中。 如果你将不存在，主题应用**HttpException**时发生。 以编程方式应用主题时，如果所有服务器控件都具有指定的 SkinID 属性，将发生生成警告。 此警告旨在通知你： 无主题以声明方式应用，并可以忽略它。

## <a name="exercise-1--applying-a-theme"></a>练习 1： 将主题应用

在此练习中，你将应用于网站的 ASP.NET 主题。

> [!IMPORTANT]
> 如果你使用 Microsoft Word 外观文件中输入信息，请确保你将不替换为正则引号智能引号。 智能引号将导致问题的外观文件。

1. 创建一个新的 ASP.NET 网站。
2. 右键单击解决方案资源管理器中的项目，然后选择添加新项。
3. 从文件的列表中选择 Web 配置文件并单击添加。
4. 右键单击解决方案资源管理器中的项目，然后选择添加新项。
5. 选择外观文件并单击添加。
6. 单击是当系统询问是否想要将内部应用程序文件放在\_主题文件夹。
7. 右键单击应用程序内的 SkinFile 文件夹\_在解决方案资源管理器中的主题文件夹，然后选择添加新项。
8. 从文件的列表中选择样式表并单击添加。 你现在具有所有必需的文件来实现您的新主题。 但是，Visual Studio 具有名为你的主题文件夹 SkinFile。 右键单击该文件夹，然后将名称更改为 CoolTheme。
9. 打开 SkinFile.skin 文件并添加下面的代码文件的末尾： 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. 保存 SkinFile.skin 文件。
11. 打开 StyleSheet.css。
12. 将所有中它的文本替换为以下： 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. 保存 StyleSheet.css 文件。
14. 打开 Default.aspx 页。
15. 添加 TextBox 控件和一个按钮控件。
16. 保存页。 现在浏览 Default.aspx 页。 它应显示为普通的 Web 窗体。
17. 打开 web.config 文件。
18. 添加正下方打开以下`<system.web>`标记： 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. 保存 web.config 文件。 现在浏览 Default.aspx 页。 它应该会显示与应用的主题。
20. 如果尚未打开，请在 Visual Studio 中打开 Default.aspx 页。
21. 选择按钮。
22. 更改**SkinID**转到的属性。 请注意，Visual Studio 提供了一个具有有效 SkinID 值的下拉列表按钮控件。
23. 保存页。 现在再次预览你的浏览器中的页。 该按钮现在手机应显示"转到"，并应更宽的外观。

使用**SkinID**属性，你可以轻松地配置不同的特定类型的服务器控件的不同实例的外观。

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme 属性

到目前为止，我们讨论仅应用使用的主题属性的主题。 在使用主题属性时, 外观文件将重写服务器控件的任何声明性设置。 例如，在练习 1 中，指定为按钮控件的"转"SkinID 并且该按钮的文本为"go"更改密码。 你可能已经注意到，设计器中的按钮的文本属性设置为"Button"，但主题重写的。 主题将始终覆盖在设计器中的任何属性设置。

如果你想要能够重写在主题的外观文件中定义的属性属性中指定设计器中，你可以使用**StyleSheetTheme**而不是主题属性的属性。 StyleSheetTheme 属性操作的主题属性相同，只不过它不会覆盖所有的显式属性设置类似的主题属性执行的操作。

若要查看此操作中，从练习 1 中的项目中打开 web.config 文件，并将更改`<pages>`到以下元素：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

现在浏览 Default.aspx 页上，你将看到该按钮控件重新具有"按钮"的文本属性。 这是因为在设计器中的显式属性设置会替代由转 SkinID 设置文本属性。

## <a name="overriding-themes"></a>重写主题

可以通过应用主题按相同名称的应用程序中重写全局主题\_主题应用程序文件夹。 但是，该主题在 true 替代方案不应用。 如果运行时遇到应用程序中的主题文件\_主题文件夹，它将应用使用这些文件的主题并将忽略的全球策略主题。

该 StyleSheetTheme 属性是可重写，并可在重写代码，如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web 部件

ASP.NET Web 部件是网页的一组集成的控件，用于创建允许最终用户修改内容、 外观和行为直接从浏览器的网站。 给该站点上的所有用户或单个用户，可以应用进行修改。 当用户修改页面和控件时，可以保存这些设置，以便一种称为个性化功能在将来的浏览器会话之间保留用户的个人首选项。 这些 Web 部件功能意味着开发人员可以使最终用户能够动态，个性化 Web 应用程序，而无需开发人员或管理员干预。

使用 Web 部件控件集，你作为开发人员可以让最终用户：

- 个性化页面内容。 用户可以向页面添加新的 Web 部件控件、 删除它们、 将它们隐藏起来，或它们像普通窗口最小化。
- 个性化页面布局。 用户可以将 Web 部件控件拖动到不同的区域，在页中，或更改其外观、 属性和行为。
- 导出和导入控件。 用户可以导入或导出数据用于其他页面或网站，Web 部件控件设置保留的属性、 外观和甚至在控件中的数据。 这减少了对最终用户的数据输入和配置要求。
- 创建连接。 用户可以建立控件之间的连接，以便，例如，图表控件无法股票行情自动收录器控件中显示的数据的关系图。 用户无法个性化不仅连接本身，但的外观和详细信息的图表控件显示数据的方式。
- 管理和个性化站点级设置。 授权的用户可以配置站点级设置，确定谁可以访问站点或页，设置对控件，基于角色的访问权限等等。 例如，处于管理角色的用户无法将 Web 部件控件的所有用户共享设置，并且可以阻止用户不是从个性化共享的控制的管理员。

你通常将使用以下三种方式中的 Web 部件： 创建的使用 Web 部件控件的页，创建单个 Web 部件控件，或创建完整、 可个性化设置的 Web 应用程序，如门户。

## <a name="page-development"></a>页开发

页开发人员可以使用可视化设计工具，如 Microsoft Visual Studio 2005 创建使用 Web 部件的页面。 中使用一种工具，例如 Visual Studio 为 Web 部件控件集的一个优点提供拖放创建和配置在可视化设计器中的 Web 部件控件的功能。 例如，你可以使用设计器拖到设计图面上的 Web 部件区域中或 Web 部件编辑器控件，，，然后配置向右调整控件在设计器中使用提供的 Web 部件 UI 控件集。 这可以加快 Web 部件应用程序的开发，并减少你必须编写的代码量。

## <a name="control-development"></a>控件开发

用作 Web 部件控件，包括标准 Web 服务器控件、 自定义服务器控件和用户控件，可以使用任何现有的 ASP.NET 控件。 你的环境的最大编程控制，你还可以创建从 web 部件类派生的自定义 Web 部件控件。 为单独的 Web 部件控件开发，你将通常创建用户控件并将其用作 Web 部件控件，或开发自定义 Web 部件控件。

作为开发的自定义 Web 部件控件的示例，你可以创建一个控件，用于提供任何其他 ASP.NET 服务器控件的可能有用，到视为可个性化的 Web 部件控件的包提供的功能： 日历、 列表、 财务信息新闻、 计算器、 更新内容，可编辑的网格的多格式文本控件连接到数据库，图表的动态更新其显示，或将天气和传输信息。 如果你的控件提供可视化设计器中，然后使用 Visual Studio 任何页开发人员只需将控件拖到 Web 部件区域并在设计时将其配置，而无需编写额外的代码。

个性化是 Web 部件功能的基础。 它使用户能够修改-或个性化设置布局、 外观和行为的页面上的 Web 部件控件。 个性化的设置较长： 它们不只是在当前的浏览器会话期间永久保存 （与视图状态一样），但还会在长期存储，以便为将来的浏览器会话以及保存用户的设置。 默认情况下，为 Web 部件页启用个性化设置。

用户界面结构组件依赖于个性化设置，并提供核心结构和所需的所有 Web 部件控件的服务。 在每个 Web 部件页面上所需的一个 UI 结构组件是人员控件。 虽然从来都不可见，则此控件具有协调页面上的所有 Web 部件控件的关键任务。 例如，它跟踪单个 Web 部件控件。 它管理 Web 部件区域 （包含页面上的 Web 部件控件的区域），以及具体控件所在的具体区域。 它还跟踪，并控制无法页面中，浏览、 连接、 编辑或目录模式下，和个性化设置更改应用于所有用户或单个用户使用的不同的显示模式。 最后，它启动并跟踪 Web 部件控件之间的连接和通信。

用户界面结构组件的第二种是区域。 区域充当布局管理器 Web 部件页上。 它们包含和组织控件派生自对部件类 （部件控件），并提供能够执行水平或垂直方向中的模块化页面布局。 区域还提供有关它们包含; 每个控件 （如页眉和页脚样式、 标题、 边框样式、 操作按钮等） 的常见且一致 UI 元素这些常见的元素称为控件的 chrome。 在不同的显示模式中以及各种控件，将使用几种特殊的类型的区域。 在下面的 Web 部件基本控件一节中介绍了不同类型的区域。

Web 部件 UI 控件，它们都派生自**一部分**类中，包含在 Web 部件页面上的主要用户界面。 Web 部件控件集灵活，而且在选项中非独占它可用于创建一部分控件。 除了创建自定义 Web 部件控件，还可以使用现有的 ASP.NET 服务器控件、 用户控件或自定义服务器控件用作 Web 部件控件。 通常情况下用于创建 Web 部件页的基本控件下一节所述。

## <a name="web-parts-essential-controls"></a>Web 部件基本控件

Web 部件控件集很广泛，但某些控制非常重要，因为它们所需的 Web 部件正常工作，或因为它们是最常用于在 Web 部件页上的控件。 在你开始使用 Web 部件，并创建基本的 Web 部件页，最好先熟悉以下表所述的基本 Web 部件控件。

| **Web 部件控件** | **说明** |
| --- | --- |
| WebPartManager | 管理页面上的所有 Web 部件控件。 一个 （且只有一个）**人员**需要每个 Web 部件页进行控制。 |
| CatalogZone | 包含 CatalogPart 控件。 使用此区域可创建用户可以从中选择要添加到页上的控件的 Web 部件控件的目录。 |
| EditorZone | 包含 EditorPart 控件。 使用此区域可使用户能够编辑和个性化设置页面上的 Web 部件控件。 |
| WebPartZone | 包含并提供整体布局撰写页的主 UI 的 web 部件控件。 每当您创建 Web 部件控件具有网页时，请使用此区域。 页面可以包含一个或多个区域。 |
| ConnectionsZone | 包含连接控件和用于管理连接提供的用户界面。 |
| Web 部件 (GenericWebPart) | 呈现的主要用户界面;大多数 Web 部件 UI 控件属于此类别。 对于最大的编程控制，您可以创建派生自基类的自定义 Web 部件控件**web 部件**控件。 用作 Web 部件控件，还可以使用现有的服务器控件、 用户控件或自定义控件。 每当在区域中，放置的任何这些控件的**人员**控件自动换行它们与**GenericWebPart**控件在运行时，以便可以将它们用于 Web 部件功能。 |
| CatalogPart | 包含用户可添加到页面的可用 Web 部件控件的列表。 |
| WebPartConnection | 创建页面上的两个 Web 部件控件之间的连接。 连接定义了作为 （的数据），提供程序，使用者作为另一个 Web 部件控件。 |
| EditorPart | 用作专用的编辑器控件的基类。 |
| EditorPart 控件 （AppearanceEditorPart、 LayoutEditorPart、 BehaviorEditorPart 和 PropertyGridEditorPart） | 允许用户对页面上的 Web 部件 UI 控件的各个方面进行个性化设置 |

## <a name="lab-create-a-web-part-page"></a>实验室： 创建 Web 部件页

在本实验中，你将创建的 Web 部件页将保持通过 ASP.NET 配置文件的信息。

### <a name="creating-a-simple-page-with-web-parts"></a>使用 Web 部件创建一个简单的页面

在演练本部分，你可以创建 Web 部件控件用于显示静态内容的页。 使用 Web 部件的第一步是创建具有两个必需的结构化元素的页。 首先，Web 部件页需要人员控件跟踪并协调所有 Web 部件控件。 其次，Web 部件页需要一个或多个区域，它们是包含 web 部件或其他服务器控件和占用的页指定的区域的复合控件。

> [!NOTE]
> 不需要执行任何操作即可启用 Web 部件个性化;默认情况下，Web 部件控件集的情况下启用它。 当你首次运行站点上的 Web 部件页时，ASP.NET 会设置默认个性化设置提供程序来存储用户个性化设置。 有关个性化设置的详细信息，请参阅 Web 部件个性化概述。


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>若要创建包含 Web 部件控件的页

1. 关闭默认页，并将一个新页面添加到名为 WebPartsDemo.aspx 的站点。
2. 切换到**设计**视图。
3. 从**视图**菜单上，请确保**非可视控件**和**详细信息**会选择的选项，以便你可以查看布局标记和不具有用户界面的控件。
4. 放置一个插入点之前`<div>`在设计图面上，然后按 ENTER 以添加新行上标记。 新行字符之前将插入点的位置，请单击**块格式**下拉列表控件的菜单上，并选择**标题 1**选项。 在标题中，添加文本**Web 部件演示页**。
5. 从**web 部件**选项卡的工具箱拖**人员**拖到页面上，刚刚之后新行字符和之前对它进行定位的控件`<div>`标记。   
  
 **人员**控件不呈现任何输出，因此它显示为灰色的框，在设计器图面上。
6. 在将插入点位置`<div>`标记。
7. 在**布局**菜单上，单击**插入表**，并创建具有一个行和三个列的新表。 单击**单元属性**按钮，选择**顶部**从**垂直对齐**下拉列表中，单击**确定**，然后单击**确定**再次以创建的表。
8. 将 WebPartZone 控件拖到该表左边的列。 右键单击**WebPartZone**控件中，选择**属性**，并设置以下属性：   
  
 ID: SidebarZone   
  
 HeaderText： 侧栏
9. 将另一个**WebPartZone**控制到中间的表列并设置以下属性：   
  
 ID: MainZone   
  
 HeaderText： 主
10. 保存该文件。

现在，页面包含两个不同的区域，你可以单独控制。 但是，两个区域中包含任何内容，因此是下一步创建内容。 对于本演练，你使用只显示静态内容的 Web 部件控件。

通过指定的 Web 部件区域布局&lt;方法&gt;元素。 在区域模板中，你可以添加任何 ASP.NET 控件，它是否自定义 Web 部件控件、 一个用户控件或现有的服务器控件。 请注意，此处使用标签控件中，并向，只需添加静态文本。 当你在常规服务器控件置于**WebPartZone**区域，ASP.NET 将该控件视为 Web 部件控件在运行时，这样在控件上的 Web 部件功能。

**若要创建主区域的内容**

1. 在**设计**视图中，将**标签**控件从**标准**到该区域的内容区域工具箱选项卡其**ID**属性设置为 MainZone。
2. 切换到**源**视图。 请注意，&lt;方法&gt;已添加元素包装**标签**MainZone 中的控件。
3. 添加一个名为特性**标题**到&lt;asp： 标签&gt;元素，并将其值设置为内容。 删除文本 ="Label"属性从&lt;asp： 标签&gt;元素。 开始标记和结束标记之间&lt;asp： 标签&gt;元素，如添加一些文本**欢迎访问主页**一对大之间&lt;h2&gt;元素标记。 你的代码应如下所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. 保存该文件。

接下来，创建用户控件，还可以添加到作为 Web 部件控件的页。

### <a name="to-create-a-user-control"></a>创建用户控件

1. 将新的 Web 用户控件添加到站点，以用作搜索控件。 取消选择选项**将源代码放在单独的文件**。 将其添加为 WebPartsDemo.aspx 页中，相同的目录中并将其命名 SearchUserControl.ascx。   
  
    > [!NOTE]
    > 本演练中的用户控件不实现实际搜索功能;它仅用于说明 Web 部件的功能。
2. 切换到**设计**视图。 从**标准**选项卡的工具箱中，将文本框控件拖动到该页面。
3. 刚添加的文本框之后放置一个插入点，然后按 ENTER 以添加新行。
4. 刚添加的文本框下方新行上拖到页面上的按钮控件。
5. 切换到**源**视图。 确保用户控件的源代码类似于下面的示例。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. 保存并关闭文件。

现在你可以将 Web 部件控件添加到边栏区域。 要将两个控件添加到边栏区域，你在前面的过程中创建一个包含是用户控件的链接，另一个列表。 作为一种标准添加链接**标签**服务器控件，类似于创建主区域的静态文本的方式。 但是，尽管中包含的各个服务器控件无法直接在区域 （如标签控件） 中包含用户控件，它们在这种情况下不是。 相反，它们是你在前面的过程中创建用户控件的一部分。 此示例演示包所需的控件，在用户控件中，所需的额外功能，然后引用作为 Web 部件控件区域中的该控件的常用方法。

在运行时，Web 部件控件集包装这两个控件与 GenericWebPart 控件。 当**GenericWebPart**控件换行的 Web 服务器控件、 通用部件控件是父控件中，和可以通过父控件的 ChildControl 属性访问的服务器控件。 通用部件控件的此使用使标准 Web 服务器控件，使其作为派生自的 Web 部件控件具有相同的基本行为和特性**web 部件**类。

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>将 Web 部件控件添加到边栏区域

1. 打开 WebPartsDemo.aspx 页。
2. 切换到**设计**视图。
3. 将用户控件你创建的页面，SearchUserControl.ascx，拖动从**解决方案资源管理器**进入该区域其**ID**属性设置为 SidebarZone，并将其放在存在。
4. 保存 WebPartsDemo.aspx 页。
5. 切换到**源**视图。
6. 内部&lt;asp: webpartzone&gt; SidebarZone，对你的用户控件的引用的正上方的元素添加&lt;asp： 标签&gt;具有元素包含的链接，如下面的示例中所示。 此外，将添加**标题**到用户控件标签中，值为属性**搜索**，如所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. 保存并关闭文件。

现在，你可以通过浏览到其在浏览器中测试你的页面。 该页面显示两个区域。 下面的屏幕快照显示的页。

**具有两个区域的 web 部件演示页**


![Web 部件 VS 演练 1 屏幕截图](profiles-themes-and-web-parts/_static/image3.gif)

**图 3**: Web 部件 VS 演练 1 屏幕快照


标题栏中的每个控件都提供对可以在控件执行的可用操作的谓词菜单访问的向下箭头。 单击其中一个控件的谓词菜单，然后单击**最小化**谓词，会发现，最小化控件。 从谓词菜单中，单击**还原**，并且控制权将返回到其正常大小。

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>使用户能够编辑页和更改布局

Web 部件提供的用户将其从一个区域中拖动到另一个更改 Web 部件控件的布局的功能。 还允许用户用来移动**web 部件**到另一个区域从控件中的，你可以允许用户编辑控件，包括其外观、 布局和行为的各种特征。 Web 部件控件集提供的基本编辑功能**web 部件**控件。 尽管你不会在本演练中，你还可以创建允许用户编辑的功能的自定义编辑器控件**web 部件**控件。 与更改的位置**web 部件**控件中，编辑控件的属性依赖于 ASP.NET 个性化保存用户所做的更改。

在演练本部分，添加用户若要编辑的任何的基本特征的能力**web 部件**页上的控件。 若要启用这些功能，你将添加另一个自定义用户控件到页上，连同&lt;asp: editorzone&gt;元素和两个编辑控件。

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>若要创建用户控件，可以更改页面布局

1. 在 Visual Studio 中，在**文件**菜单上，选择**新建**子菜单，然后单击**文件**选项。
2. 在**添加新项**对话框中，选择**Web 用户控件**。 新文件 DisplayModeMenu.ascx 的名称。 取消选择选项**将源代码放在单独的文件**。
3. 单击添加创建新的控件。
4. 切换到**源**视图。
5. 在新的文件中，删除所有现有的代码并粘贴下面的代码中。 此用户控件代码使用功能的 Web 部件控件集，使页后，可以更改其视图或显示模式，并且还使你能够更改物理外观和布局页时的特定显示模式中。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. 通过单击保存保存该文件图标的工具栏上，或通过选择**保存**上**文件**菜单。

### <a name="to-enable-users-to-change-the-layout"></a>若要使用户能够更改布局

1. 打开 WebPartsDemo.aspx 页上，并切换到**设计**视图。
2. 在将插入点位置**设计**紧后面查看**人员**前面添加的控件。 在文本之后添加硬回车，因此就后的一个空行**人员**控件。 将插入点放在空白行上。
3. 将你刚创建用户控件拖动 （该文件被命名为 DisplayModeMenu.ascx） 到 WebPartsDemo.aspx 页上，并将其放在空白行上。
4. 将从 EditorZone 控件拖动**web 部件**WebPartsDemo.aspx 页中的剩余打开表单元格工具箱部分。
5. 从**web 部件**部分的工具箱 AppearanceEditorPart 控件将和 LayoutEditorPart 控件拖动到**EditorZone**控件。
6. 切换到**源**视图。 表单元格中生成的代码应类似于下面的代码。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. 保存 WebPartsDemo.aspx 文件。 已创建用户控件，您可以更改显示模式和更改页面布局，并且你引用主网页上的控件。

你现在可以测试编辑页面和更改布局的功能。

### <a name="to-test-layout-changes"></a>若要测试布局更改

1. 加载浏览器中的页。
2. 单击**显示模式**下拉列表菜单，然后选择**编辑**。 显示区域标题。
3. 拖动**我的链接**通过从侧栏区域到主区域底部的标题栏控件。 页的外观应类似于以下屏幕快照。

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Web 部件演示页与移动的我的链接控件


![Web 部件 VS 演练 2 屏幕截图](profiles-themes-and-web-parts/_static/image4.gif)

**图 4**: Web 部件 VS 演练 2 屏幕快照


1. 单击**显示模式**下拉列表菜单，然后选择**浏览**。 刷新页面、 区域名称消失，和**我的链接**控制的保持为您放置的位置。
2. 为了演示的个性化正常工作，关闭浏览器中，然后重新加载页面。 你所做的更改保存为将来的浏览器会话中。
3. 从**显示模式**菜单上，选择**编辑**。   
  
 页面上的每个控件现在显示在其标题栏中，其中包含谓词下拉菜单中向下箭头。
4. 单击箭头以显示上谓词菜单**我的链接**控件。 单击**编辑**谓词。   
  
 **EditorZone**控件的外观、 显示 EditorPart 控制你添加。
5. 在**外观**的编辑控件，更改的部分**标题**到我的收藏夹，使用**Chrome 类型**下拉列表选择**仅标题**，然后单击**应用**。 下面的屏幕快照显示页面处于编辑模式。

### <a name="web-parts-demo-page-in-edit-mode"></a>在编辑模式下的 web 部件演示页


![Web 部件 VS 演练 3 屏幕截图](profiles-themes-and-web-parts/_static/image5.gif)

**图 5**: Web 部件 VS 演练 3 屏幕截图


1. 单击**显示模式**菜单，然后选择**浏览**以返回到浏览模式。
2. 该控件现在具有更新的标题但没有边框，如下面的屏幕快照中所示。

### <a name="edited-web-parts-demo-page"></a>编辑的 Web 部件演示页


![Web 部件 VS 演练 4 屏幕截图](profiles-themes-and-web-parts/_static/image6.gif)

**图 4**: Web 部件 VS 演练 4 屏幕截图


### <a name="adding-web-parts-at-run-time"></a>在运行时添加 Web 部件

你可以允许用户在运行时将 Web 部件控件添加到他们的页面。 为此，请配置与 Web 部件目录，其中包含你想要向用户提供的 Web 部件控件的列表的页面。

**若要允许用户在运行时添加 Web 部件**

1. 打开 WebPartsDemo.aspx 页上，并切换到**设计**视图。
2. 从**web 部件**选项卡的工具箱中，CatalogZone 将控件拖到表中，右侧列下方**EditorZone**控件。   
  
 两个控件可能在相同的表单元，因为它们不会显示在同一时间。
3. 在属性窗格中，将字符串分配**添加 Web 部件**到的 HeaderText 属性**CatalogZone**控件。
4. 从**web 部件**部分的工具箱中，将 DeclarativeCatalogPart 控件拖动到的内容区域的**CatalogZone**控件。
5. 单击中的右上角的箭头**DeclarativeCatalogPart**控件公开其任务菜单中，，然后选择**编辑模板**。
6. 从**标准**部分的工具箱拖**FileUpload**控件和**日历**控制**WebPartsTemplate**部分**DeclarativeCatalogPart**控件。
7. 切换到**源**视图。 检查的源代码&lt;了&gt;元素。 请注意， **DeclarativeCatalogPart**控件包含&lt;webpartstemplate&gt;你将能够从目录添加到页面的两个封装的服务器控件的元素。
8. 添加**标题**到每个你添加到目录中，为每个标题中下面的代码示例使用显示的字符串值的控件的属性。 即使标题不是属性，但你可以正常情况下设置这些这两个服务器控件在设计时，当用户添加到这些控件**WebPartZone**区域从在运行时目录，它们的每个包装与**GenericWebPart**控件。 这可让其充当 Web 部件控件，因此它们将能以显示标题。   
  
 中包含的两个控件的代码**DeclarativeCatalogPart**控件的外观，如下所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. 保存页。

你现在可以测试该目录。

### <a name="to-test-the-web-parts-catalog"></a>若要测试 Web 部件目录

1. 加载浏览器中的页。
2. 单击**显示模式**下拉列表菜单，然后选择**目录**。   
  
 标题为目录**添加 Web 部件**显示。
3. 拖动**我的收藏夹**回侧栏区域中，顶部从主区域控件并将它放存在。
4. 在**添加 Web 部件**目录，选择两个复选框，，然后选择**Main**从下拉列表包含可用区域。
5. 单击**添加**在目录中。 这些控件添加到主区域中。 如果你想，你可以从目录中向页面添加控件的多个实例。   
  
 下面的屏幕快照显示在主区域中的文件上载控件和日历的页。 

![从目录添加到主区域的控件](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. 单击**显示模式**下拉列表菜单，然后选择**浏览**。 该目录将消失，刷新页面。
7. 关闭浏览器。 重新加载页面。 你所做的更改将保留。
