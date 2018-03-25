---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: "如何升级 ASP.NET MVC 4 和 Web API 项目到 ASP.NET MVC 5 和 Web API 2 |Microsoft 文档"
author: Rick-Anderson
description: "ASP.NET MVC 5 和 Web API 2 会将新功能，包括的属性路由、 身份验证筛选器和其他更多的主机。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 05a3189cf105d1230b96e90b46ea5ab60fef1bf1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>如何将 ASP.NET MVC 4 和 Web API 项目升级到 ASP.NET MVC 5 和 Web API 2
====================
通过[Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 和 Web API 2 会将新功能，包括的属性路由、 身份验证筛选器和其他更多的主机。 请参阅[https://www.asp.net/vnext](https://www.asp.net/core)有关详细信息。
> 
> 本演练将指导你执行升级到最新版本的应用程序所需的步骤。  
> 
> > [!NOTE]
> > 请参阅[ASP.NET 和 Web Tools for Visual Studio 2013 发行说明](../../../visual-studio/overview/2013/release-notes.md)有关的重大更改到下一个版本 MVC 4 和 Web API 的信息。
> 
>   
> 
> 通过 Youngjune 香港和 Rick Anderson 撰写本文时 ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>升级步骤

1. 备份你的项目。 本演练将要求你对你的项目文件、 包配置和 web.config 文件进行更改。
2. 从 Web API 升级到 Web API 2 中 global.asax，更改：

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

 更改为

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. 请确保你的项目使用的所有包都都与 MVC 5 和 Web API 2 兼容。 以下表所示 MVC 4 和 Web API 与相关包的不是需要更改。 如果你具有的依赖于一个下面列出的包的包，请联系要获取与 MVC 5 和 Web API 2 兼容的较新版本的发布者。 如果你有这些包的源代码，应使用新的程序集的 MVC 5 和 Web API 2 时将它们重新编译。   

    | **包 Id** | **旧版本** | **新版本** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x。 | 2.2.x。 |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o:p >< / o:p > | 已删除 |
    | Microsoft.AspNet.WebPages.Administration | < o:p >< / o:p > | 已删除 |
    | Microsoft Web 帮助 | < o:p >< / o:p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > 用 Microsoft.AspNet.WebHelpers 替换 Microsoft Web 帮助。 您应首先，删除旧的包，然后安装较新的包。   
    >   
    > 没有在主要 ASP.NET 包之间交叉的版本兼容性。 例如，MVC 5 是与仅 Razor 3 和 Razor 2 不兼容。
4. 在 Visual Studio 2013 中打开你的项目。
5. 删除任何已安装的以下 ASP.NET NuGet 包。 你将删除这些使用包管理器控制台 (PMC)。 若要打开 PMC，选择**工具**菜单，然后选择**库程序包管理器，**然后选择**程序包管理器控制台**。 你的项目可能不包括所有这些。

    1. `Microsoft.AspNet.WebPages.Administration`  
 从 MVC 3 升级到 MVC 4 时，通常将特性添加此包。 若要删除它，请在 PMC 中运行以下命令：  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
 此包重新命名为`Microsoft.AspNet.WebHelpers`。 若要删除它，请在 PMC 中运行以下命令：  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
 此程序包包含解决 MVC 5 中已修复的 MVC 4 中的 bug。 若要删除它，请在 PMC 中运行以下命令：  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. 升级使用 PMC 的所有 ASP.NET NuGet 包。 在 PMC，运行以下命令：  
    `Update-Package`  
 `Update-Package`命令没有任何参数将更新每个包。 可以通过使用 ID 参数单独更新包。 有关更新命令的详细信息，请运行`get-help update-package`。

## <a name="update-the-application-webconfig-file"></a>更新应用程序*web.config*文件

请务必在应用程序进行这些更改*web.config*文件，不*web.config*文件中*视图*文件夹。

找到`<runtime>/<assemblyBinding>`部分，然后进行以下更改：

1. 在名称属性"System.Web.Mvc"具有的元素，将更改版本介于"4.0.0.0"和"5.0.0.0"。 （该元素中的两个更改。）
2. 具有名称特性的元素中&quot;System.Web.Helpers"和&quot;System.Web.WebPages&quot;更改版本介于"2.0.0.0"和"3.0.0.0"。 四次更改，将发生两个在每个元素。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. 找到`<appSettings>`部分并更新到 3.0.0.0 从 2.0.0.0.0 webpages:version，如下所示：

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. 删除任何而不是完整的信任级别。 例如: 

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>更新*web.config*视图文件夹下的文件

如果你的应用程序使用的区域，将需要更新每个*web.config*文件中*视图*的每个区域文件夹的子文件夹。

1. 更新到版本"5.0.0.0"从版本"4.0.0.0"包含"System.Web.Mvc"的所有元素。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. 更新包含版本"3.0.0.0"至"System.Web.WebPages.Razor"版本"2.0.0.0"的所有元素。 本部分包含"System.Web.WebPages"，如果更新到版本"3.0.0.0"版本"2.0.0.0"从这些元素  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. 如果你删除`Microsoft-Web-Helpers`在上一步骤中，NuGet 包安装`Microsoft.AspNet.WebHelpers`与 PMC 中的以下命令：  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. 如果你的应用程序使用[User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)方法，添加到以下*Web.config*文件。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>最后的步骤

生成并测试应用程序。

从项目文件中删除的 MVC 4 项目类型 GUID。

1. 在解决方案资源管理器，右键单击项目名称，然后选择**卸载项目**。
2. 右键单击该项目并选择编辑 ProjectName.csproj。
3. 找到`ProjectTypeGuids`元素，然后删除 MVC 4 项目 GUID， `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`。
4. 保存并关闭打开的项目文件。
5. 右键单击项目并选择**重新加载项目**。
