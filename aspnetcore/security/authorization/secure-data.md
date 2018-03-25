---
title: "使用受授权的用户数据创建 ASP.NET Core 应用"
author: rick-anderson
description: "了解如何创建使用受授权的用户数据的 Razor 页应用。 包括 HTTPS、 身份验证、 安全性、 ASP.NET 核心标识。"
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 5acb65be078fd39b9e7a17ce2d8167b8f7b7db22
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>使用受授权的用户数据创建 ASP.NET Core 应用

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Joe Audette](https://twitter.com/joeaudette)

本教程演示如何使用受授权的用户数据创建 ASP.NET 核心 web 应用。 它显示的身份验证的 （注册） 的用户的联系人列表已创建。 有三个安全组：

* **注册用户**可以查看所有已批准的数据并可以编辑/删除其自己的数据。
* **管理器**可以批准或拒绝联系人数据。 仅批准的联系人是对用户可见。
* **管理员**可以批准/拒绝和编辑/删除任何数据。

在下图中，用户 Rick (`rick@example.com`) 登录。 Rick 只能查看被批准的联系人和**编辑**/**删除**/**新建**他联系人的链接。 仅最新的记录，创建由 Rick，显示**编辑**和**删除**链接。 其他用户不会看到的最新记录，直到经理或管理员的状态更改为"已批准"。

![映像所述前面](secure-data/_static/rick.png)

在下图中，`manager@contoso.com`进行签名和管理员角色中：

![映像所述前面](secure-data/_static/manager1.png)

下图显示了管理器的联系人的详细信息视图：

![映像所述前面](secure-data/_static/manager.png)

**批准**和**拒绝**按钮仅显示经理和管理员。

在下图中，`admin@contoso.com`进行签名和管理员角色中：

![映像所述前面](secure-data/_static/admin.png)

管理员具有所有权限。 她可以读取、 编辑或删除任何联系人，并更改联系人的状态。

应用程序由[基架](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)以下`Contact`模型：

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

该示例包含以下授权处理程序：

* `ContactIsOwnerAuthorizationHandler`： 可确保用户只能编辑其数据。
* `ContactManagerAuthorizationHandler`： 允许管理器批准或拒绝联系人。
* `ContactAdministratorsAuthorizationHandler`： 允许管理员批准或拒绝联系人，还可以编辑/删除联系人。

## <a name="prerequisites"></a>系统必备

本教程被高级。 你应熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [身份验证](xref:security/authentication/index)
* [帐户确认和密码恢复](xref:security/authentication/accconfirm)
* [授权](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

请参阅[此 PDF 文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET 核心 MVC 版本。 本教程的 ASP.NET 核心 1.1 版本是[这](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)文件夹。 ASP.NET 核心示例是在 1.1[示例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。

## <a name="the-starter-and-completed-app"></a>初学者和已完成应用程序

[下载](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)应用。 [测试](#test-the-completed-app)已完成的应用程序使你熟悉其安全功能。

### <a name="the-starter-app"></a>初学者应用

[下载](xref:tutorials/index#how-to-download-a-sample)[初学者](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)应用。

运行应用，点击**ContactManager**链接，并验证是否可以创建、 编辑和删除联系人。

## <a name="secure-user-data"></a>保护用户数据

下列各节具有所有主要的步骤以创建安全的用户数据应用。 你可能会发现已完成的项目是指很有帮助。

### <a name="tie-the-contact-data-to-the-user"></a>将向用户的联系人数据

使用 ASP.NET[标识](xref:security/authentication/identity)用户 ID，以确保用户可以编辑其数据，而非其他用户数据。 添加`OwnerID`和`ContactStatus`到`Contact`模型：

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` 是从用户的 ID`AspNetUser`表中[标识](xref:security/authentication/identity)数据库。 `Status`字段确定是否可由常规用户查看联系人。

创建一个新迁移并更新数据库：

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>需要 HTTPS 和经过身份验证的用户

添加[IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)到`Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

在`ConfigureServices`方法*Startup.cs*文件中，添加[RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)授权筛选器：

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

如果你使用 Visual Studio，则启用 HTTPS。

若要将 HTTP 请求重定向到 HTTPS，请参阅[URL 重写中间件](xref:fundamentals/url-rewriting)。 如果你使用 Visual Studio Code 或不包括为支持 HTTPS 的测试证书的本地平台上测试：

  设置`"LocalTest:skipSSL": true`中*appsettings。Developement.json*文件。

### <a name="require-authenticated-users"></a>要求经过身份验证的用户

设置默认身份验证策略以要求用户进行身份验证。 你可以选择不在与 Razor 页、 控制器或操作方法级别的身份验证`[AllowAnonymous]`属性。 设置默认身份验证策略以要求用户进行身份验证保护新添加的 Razor 页和控制器。 默认情况下所需的身份验证为比依赖于新控制器和 Razor 页，以包含更安全`[Authorize]`属性。 

与所有用户进行身份验证，要求[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)和[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0)调用不是必需的。

更新`ConfigureServices`有以下更改：

* 注释掉`AuthorizeFolder`和`AuthorizePage`。
* 设置默认身份验证策略以要求用户进行身份验证。

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

添加[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)到索引中，因此匿名用户可以获取有关站点的信息，然后它们注册的有关，和联系人页面。 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

添加`[AllowAnonymous]`到[LoginModel 和 RegisterModel](https://github.com/aspnet/templating/issues/238)。

### <a name="configure-the-test-account"></a>配置测试帐户

`SeedData`类将创建两个帐户： 管理员和管理员。 使用[机密管理器工具](xref:security/app-secrets)设置这些帐户的密码。 从项目目录设置密码 (目录包含*Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

更新`Main`使用测试密码：

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>创建测试帐户和更新联系人

更新`Initialize`中的方法`SeedData`类，以创建测试帐户：

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

添加管理员的用户 ID 和`ContactStatus`向联系人。 先创建一个"已提交"和一个"已拒绝"的联系人。 将用户 ID 和状态添加到所有联系人。 只能有一个联系人是所示：

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>创建所有者、 管理器中，和管理员授权处理程序

创建`ContactIsOwnerAuthorizationHandler`类*授权*文件夹。 `ContactIsOwnerAuthorizationHandler`验证对资源进行操作的用户拥有的资源。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`调用[上下文。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)当前经过身份验证的用户是否联系人的所有者。 授权处理程序通常：

* 返回`context.Succeed`满足的要求。
* 返回`Task.CompletedTask`当不满足要求。 `Task.CompletedTask` 既不成功或失败&mdash;它允许运行其他授权处理程序。

如果你需要显式失败，返回[上下文。失败](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

该应用让联系人所有者到编辑/删除/创建其自己的数据。 `ContactIsOwnerAuthorizationHandler` 不需要请检查在要求参数中传递的操作。

### <a name="create-a-manager-authorization-handler"></a>创建管理器授权处理程序

创建`ContactManagerAuthorizationHandler`类*授权*文件夹。 `ContactManagerAuthorizationHandler`验证用户对资源进行操作是一个管理器。 只有经理可以批准或拒绝内容更改 （新的或已更改的）。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>创建一个管理员授权处理程序

创建`ContactAdministratorsAuthorizationHandler`类*授权*文件夹。 `ContactAdministratorsAuthorizationHandler`验证对资源进行操作的用户是管理员。 管理员可以执行所有操作。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>注册的授权处理程序

必须为注册服务使用实体框架核心[依赖关系注入](xref:fundamentals/dependency-injection)使用[AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。 `ContactIsOwnerAuthorizationHandler`使用 ASP.NET Core[标识](xref:security/authentication/identity)，这基于实体框架核心。 注册服务集合的处理程序，因此要对其可供`ContactsController`通过[依赖关系注入](xref:fundamentals/dependency-injection)。 将以下代码添加到末尾`ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`添加为单一实例。 它们是单一实例，因为它们不使用 EF 和所需的所有信息都，请参阅`Context`参数`HandleRequirementAsync`方法。

## <a name="support-authorization"></a>支持授权

在本部分中，你将更新 Razor 的网页，并添加操作要求类。

### <a name="review-the-contact-operations-requirements-class"></a>查看联系人的操作要求类

查看`ContactOperations`类。 此类包含要求应用程序支持：

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>创建用于 Razor 页面的基类

创建一个包含在联系人 Razor 页中所使用的服务的基本类。 基的类将该初始化代码放在一个位置：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

前面的代码：

* 将添加`IAuthorizationService`服务访问的授权处理程序。
* 添加标识`UserManager`服务。
* 添加 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新创建页模型构造函数以使用`DI_BasePageModel`基类：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

更新`CreateModel.OnPostAsync`方法：

* 添加到的用户 ID`Contact`模型。
* 调用授权处理程序，以验证用户有权创建联系人。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，以便仅被批准的联系人显示在常规用户：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

添加授权处理程序以验证用户拥有联系人。 正在验证资源授权，因为`[Authorize]`属性不足够。 评估属性时，此应用程序没有对资源的访问。 基于资源的授权必须是命令性。 一旦应用程序有权访问该资源，通过在页模型中加载或加载内处理程序本身，则必须执行检查。 你经常访问的资源，通过传入的资源键。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新删除页模型，使用授权处理程序来验证用户对联系人拥有删除权限。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>将授权服务注入到视图

目前，UI 显示编辑和删除用户无法修改的数据的链接。 用户界面是通过将授权处理程序应用于视图固定的。

插入中的授权服务*Views/_ViewImports.cshtml*文件以便它可供所有视图：

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

前面的标记添加了多种`using`语句。

更新**编辑**和**删除**链接*Pages/Contacts/Index.cshtml*以便它们在仅呈现具有适当权限的用户：

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> 隐藏来自没有更改数据的权限的用户的链接不安全应用程序。 隐藏链接进行应用程序更加友好的用户显示唯一有效的链接。 用户可以 hack 生成的 Url 来调用编辑和删除在它们自己的数据的操作。 Razor 页或控制器必须强制执行访问检查，以保护数据。

### <a name="update-details"></a>更新详细信息

更新的详细信息视图，以便经理可以批准或拒绝联系人：

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

更新详细信息页模型：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>测试已完成的应用程序

如果你使用 Visual Studio Code 或不包括为支持 HTTPS 的测试证书的本地平台上测试：

* 设置`"LocalTest:skipSSL": true`中*appsettings。Developement.json*文件中跳过 HTTPS 要求。 跳过仅在开发计算机上的 HTTPS。

如果应用了联系人：

* 删除中的所有记录`Contact`表。
* 重新启动应用植入到数据库。

浏览联系人注册用户。

若要测试已完成的应用程序的简单方法是以启动三个不同的浏览器 （或 incognito/InPrivate 版本）。 一个在浏览器中注册一个新用户 (例如， `test@example.com`)。 登录到每个浏览器了不同的用户。 验证以下操作：

* 已注册的用户可以查看所有已批准的联系人数据。
* 已注册的用户可以编辑/删除其自己的数据。
* 经理可以批准或拒绝联系人数据。 `Details`视图显示**批准**和**拒绝**按钮。
* 管理员可以批准/拒绝和编辑/删除任何数据。

| “用户”| 选项 |
| ------------ | ---------|
| test@example.com | 可以编辑/删除自己的数据 |
| manager@contoso.com | 可以批准/拒绝和编辑/删除拥有数据 |
| admin@contoso.com | 可以编辑/删除和批准/拒绝的所有数据|

在管理员的浏览器中创建一个联系人。 将删除该 URL 复制和编辑从管理员的联系信息。 将这些链接粘贴到测试用户的浏览器以验证测试用户无法执行这些操作。

## <a name="create-the-starter-app"></a>创建初学者应用

* 创建名为"ContactManager"Razor 页应用

  * 创建应用程序与**单个用户帐户**。
  * 请将其命名"ContactManager"使你的命名空间匹配示例中使用的命名空间。

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld` 指定而不是 SQLite LocalDB

* 添加以下`Contact`模型：

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* 基架`Contact`模型：

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* 更新**ContactManager**中锚定*Pages/_Layout.cshtml*文件：

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* 构建基架初始迁移并更新数据库：

```console
dotnet ef migrations add initial
dotnet ef database update
```

* 测试应用程序通过创建、 编辑和删除联系人

### <a name="seed-the-database"></a>设定数据库种子

添加`SeedData`类到*数据*文件夹。 如果你已下载示例，你可以复制*SeedData.cs*文件为*数据*初学者项目的文件夹。

调用`SeedData.Initialize`从`Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

测试应用程序设定种子的数据库。 如果在联系人中数据库的任何行，不能运行 seed 方法。

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>其他资源

* [ASP.NET 核心授权实验室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。 在本教程中引入的安全功能，此实验室将进入更多详细信息。
* [在 ASP.NET Core 中的授权： 基于声明的和自定义的简单，角色](xref:security/authorization/index)
* [基于自定义策略的授权](xref:security/authorization/policies)
