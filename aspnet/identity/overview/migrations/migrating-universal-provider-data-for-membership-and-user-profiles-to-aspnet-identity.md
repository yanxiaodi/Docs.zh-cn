---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: "将通用提供程序的数据迁移的成员身份和到 ASP.NET 标识 (C#) 的用户配置文件 |Microsoft 文档"
author: rustd
description: "本教程介绍的步骤所需迁移用户和角色数据并创建使用的现有应用程序的通用提供程序的用户配置文件数据..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: e00bcfc111425d5dd26c7ff341eaf87fd969e089
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>将通用提供程序的数据迁移的成员身份和用户配置文件到 ASP.NET 标识 (C#)
====================
通过[Pranav Rastogi](https://github.com/rustd)， [Rick Anderson](https://github.com/Rick-Anderson)， [Robert McMurray](https://github.com/rmcmurray)， [Suhas Joshi](https://github.com/suhasj)

> 本教程介绍了所需迁移用户和角色数据并创建使用 ASP.NET 标识模型到现有应用程序的通用提供程序的用户配置文件数据的步骤。 若要迁移用户配置文件数据可以使用 SQL 成员身份的应用程序中，此处提及方法。


Visual Studio 2013 的版本中，ASP.NET 团队引入新的 ASP.NET 标识系统，你可以阅读更多有关该版本[此处](../../index.md)。 后续项目后，将从 web 应用程序迁移[到新的标识系统的 SQL 成员资格](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)，本文将演示迁移现有应用程序，请遵循用户和角色的管理提供程序模型的步骤为新的标识模型。 本教程重点介绍将主要在迁移用户配置文件数据，以无缝地将它挂接到新系统上。 迁移用户和角色的信息是类似的 SQL 成员资格。 可以使用 SQL 成员资格以及应用程序中使用遵循迁移配置文件数据的方法。

例如，我们将开始使用 Visual Studio 2012 使用的提供程序模型中创建 web 应用程序。 我们然后添加配置文件管理的代码、 注册用户、 添加用户的配置文件数据、 迁移数据库架构，然后将更改应用程序使用的标识系统为用户和角色管理。 为迁移的测试，使用通用提供程序创建的用户应该能够以要求在登录和新用户应该能够注册。

> [!NOTE]
> 你可以找到完整的示例在[https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations)。


## <a name="profile-data-migration-summary"></a>配置文件数据迁移摘要

在开始之前进行迁移，让我们看一下存储配置文件数据提供程序模型中的体验。 应用程序可以采用多种方式存储用户的配置文件数据，最常见它们之间正在使用内置的配置文件提供程序以及通用提供程序提供。 这些步骤将包括

1. 添加具有用来存储配置文件数据的属性的类。
2. 添加扩展 ProfileBase 并实现方法以获取用户的上述的配置文件数据的类。
3. 使用默认配置文件中的提供程序启用*web.config*文件，并定义用于访问配置文件信息的第 2 步中声明的类。

作为序列化的 xml，并在数据库中的配置文件表中的二进制数据存储的配置文件信息。

迁移后应用程序以使用新的 ASP.NET 标识系统，配置文件信息是反序列化和存储为用户类上的属性。 然后，每个属性可映射到用户表中的列中。 此处的优点是，可以直接使用用户类除了无需序列化/反序列化数据信息进行处理的属性对其进行访问的时间。

## <a name="getting-started"></a>入门

1. 在 Visual Studio 2012 中创建新的 ASP.NET 4.5 Web 窗体应用程序。 当前的示例使用 Web 窗体模板，但你也可以使用 MVC 应用程序。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. 创建一个新文件夹模型来存储配置文件信息  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. 作为示例，让我们在配置文件中存储的出生、 市/县、 高度和的用户的权重的日期。 一个名为 PersonalStats 的自定义类作为存储的高度和权重。 若要存储和检索配置文件，我们需要扩展 ProfileBase 的类。 让我们创建一个新类 AppProfile 获取和存储配置文件信息。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. 启用配置文件中的*web.config*文件。 输入要用于存储/检索在步骤 #3 中创建的用户信息的类名称。

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. 在帐户文件夹，从用户获取配置文件数据并将其存储中添加 web 窗体页。 右键单击项目并选择添加新项。 添加新 webforms 页与母版页 AddProfileData.aspx。 在主内容部分中复制下列文本：

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

 在代码隐藏部分添加以下代码：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

 添加下哪些 AppProfile 类定义以删除编译错误的命名空间。
6. 运行应用并使用用户名创建新用户**olduser。** 导航到 AddProfileData 页，并添加用户的配置文件信息。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

你可以验证，将数据存储为使用服务器资源管理器窗口的配置文件表中的序列化的 xml。 在 Visual Studio 中，从视图菜单上，选择服务器资源管理器。 应定义中的数据库的数据连接*web.config*文件。 单击数据连接上显示不同的子类别。 展开表以显示在不同的表在数据库中，然后右键单击配置文件并选择显示表数据以查看配置文件表中存储的配置文件数据。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>迁移数据库架构

若要使现有数据库使用的标识系统，我们需要更新标识数据库，以支持我们将添加到原始数据库的字段中的架构。 这可以使用 SQL 脚本以创建新表并将复制现有的信息。 在服务器资源管理器窗口中，展开 DefaultConnection 以显示表。 右键单击表，然后选择新建查询

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

将从 SQL 脚本粘贴[https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt)并运行它。 如果刷新 DefaultConnection，则我们可以看到，添加新表。 你可以检查以查看已迁移信息的表中的数据。

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>迁移应用程序以使用 ASP.NET 标识

1. 安装 ASP.NET 标识所需的 Nuget 包：

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

 找不到管理 Nuget 包的详细信息[此处](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. 若要使用表中的现有数据，我们需要创建模型类映射回表和挂钩标识系统中。 作为标识协定的一部分，模型类应实现 Identity.Core dll 中定义的接口，也可以扩展 Microsoft.AspNet.Identity.EntityFramework 中提供这些接口的现有实现。 我们将角色、 用户登录名和用户声明使用现有的类。 我们需要将自定义用户用于我们的示例。 右键单击项目并创建新文件夹 IdentityModels。 添加新的 'User' 类，如下所示：

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

 请注意，ProfileInfo 现在是用户类上的属性。 因此我们可以使用用户类来直接处理配置文件数据。

复制中的文件**IdentityModels**和**IdentityAccount**下载从源文件夹 ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) )。 这些具有剩余模型类和所需的用户和角色管理使用 ASP.NET 标识 Api 的新页。 使用的方法类似于 SQL 成员资格，可以找到详细的说明[此处](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)。

## <a name="copying-profile-data-to-the-new-tables"></a>配置文件将数据复制到新表

如前所述，我们需要反序列化配置文件表中的 xml 数据并将其存储在 AspNetUsers 表的列。 因此剩下的就是填充那些具有所需数据的列在上一步中的用户表中创建新的列。 若要执行此操作，我们将使用一次运行来填充用户表中新创建的列的控制台应用程序。

1. 正在退出的解决方案中创建新的控制台应用程序。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. 安装最新版本的实体框架包。
3. 添加上面创建控制台应用程序的引用该 web 应用程序。 若要执行此右键单击项目，然后添加引用，然后解决方案，然后单击该项目并单击确定。
4. 复制下面的 Program.cs 类中的代码。 此逻辑读取的每个用户配置文件数据、 将其序列化为作为 ProfileInfo 对象和将其存储到数据库。

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

 所使用的模型的某些文件夹中定义 IdentityModels 的 web 应用程序项目使用，因此你必须将包含相应的命名空间。
5. 上面的代码适用于应用程序中的数据库文件\_在前面的步骤中创建的 web 应用程序项目的数据文件夹。 若要引用的请使用 web 应用程序的 web.config 中的连接字符串更新控制台应用程序的 app.config 文件中的连接字符串。 此外提供 AttachDbFilename 属性中的完整物理路径。
6. 打开命令提示符并导航到上面的控制台应用程序的 bin 文件夹。 运行可执行文件，并查看日志输出，如下图中所示。  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. 在服务器资源管理器中打开 AspNetUsers 表，并验证保存属性的新列中的数据。 它们应使用相应的属性值更新。

## <a name="verify-functionality"></a>验证功能

使用使用 ASP.NET 标识登录旧的数据库中的用户实现新添加的成员资格页。 用户应该能够使用相同的凭据登录。 尝试的其他功能，例如添加 OAuth，创建一个新的用户，更改密码，添加角色，将用户添加到角色，等等。

应检索旧的用户和新用户的配置文件数据，并将其存储在用户表中。 应不再引用旧表。

## <a name="conclusion"></a>结束语

本文介绍了迁移的 web 应用程序用于为 ASP.NET 标识的成员资格提供程序模型的过程。 此外，文章所述用户要挂钩到的标识系统的迁移配置文件数据。 请将下面的问题和迁移您的应用程序时遇到的问题的注释。

*感谢 Rick Anderson 和 Robert McMurray 有关本文的审阅。*
