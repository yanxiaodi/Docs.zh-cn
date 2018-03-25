---
title: "Azure Active Directory B2C ASP.NET 核心中使用云身份验证"
author: camsoper
description: "了解如何设置与 ASP.NET 核心的 Azure Active Directory B2C 身份验证。"
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Azure Active Directory B2C ASP.NET 核心中使用云身份验证

作者：[Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是为 web 和移动应用的云标识管理解决方案。 服务提供用于在云中和本地托管的应用的身份验证。 身份验证类型包括个人帐户，社交网络帐户，以及联合企业帐户。 此外，Azure AD B2C 可以提供以最小配置多因素身份验证。

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C 包括单独的产品。 Azure AD 租户表示是一个组织，而 Azure AD B2C 租户表示标识要用于信赖方应用程序的集合。 若要了解详细信息，请参阅[Azure AD B2C： 常见问题 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。

在本教程中，学习如何：

> [!div class="checklist"]
> * 创建 Azure Active Directory B2C 租户
> * 在 Azure AD B2C 注册某个应用程序
> * 使用 Visual Studio 创建 ASP.NET 核心 web 应用配置为使用 Azure AD B2C 租户进行身份验证
> * 配置控制 Azure AD B2C 租户的行为的策略

## <a name="prerequisites"></a>系统必备

对于本演练具备以下条件：

* [Microsoft Azure 订阅](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任意版本）

## <a name="create-the-azure-active-directory-b2c-tenant"></a>创建 Azure Active Directory B2C 租户

创建 Azure Active Directory B2C 租户[文档中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出现提示时，将租户与 Azure 订阅相关联是可选的出于本教程。

## <a name="register-the-app-in-azure-ad-b2c"></a>在 Azure AD B2C 中注册应用程序

新创建的 Azure AD B2C 租户中注册你的应用使用[文档中的步骤](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**注册 web 应用**部分。 在停止**创建 web 应用程序客户端密钥**部分。 出于本教程中，无需客户端密钥。 

使用以下值：

| 设置                       | “值”                     | 说明                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名称**                      | *&lt;应用程序名称&gt;*        | 输入**名称**描述你的应用对使用者的应用。                                                                                                                                 |
| **包括 web 应用程序/web API** | 是                       |                                                                                                                                                                                                    |
| **允许隐式流**       | 是                       |                                                                                                                                                                                                    |
| **回复 URL**                 | `https://localhost:44300` | 答复 Url 是 Azure AD B2C 其中返回您的应用程序请求的任何令牌的终结点。 Visual Studio 提供要使用的答复 URL。 现在，输入`https://localhost:44300`完成窗体。 |
| **应用程序 ID URI**                | 将保留为空               | 无需进行本教程。                                                                                                                                                                    |
| **包括本机客户端**     | 否                        |                                                                                                                                                                                                    |

> [!WARNING]
> 如果设置非 localhost 答复 URL，请注意[了允许的答复 URL 列表中的约束](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)。 

注册应用后，显示的租户中的应用列表。 选择刚刚注册的应用。 选择**复制**右侧的图标**应用程序 ID**字段可将它复制到剪贴板。

执行任何操作的详细信息在此时，可以在 Azure AD B2C 租户中配置，但使浏览器窗口保持打开状态。 创建 ASP.NET Core 应用后，没有更多的配置。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>在 Visual Studio 2017 年 1 中创建 ASP.NET Core 应用

Visual Studio Web 应用程序模板可以配置为使用 Azure AD B2C 租户进行身份验证。

在 Visual Studio 中：

1. 创建新的 ASP.NET Core Web 应用程序。 
2. 选择**Web 应用程序**从模板列表中。
3. 选择**更改身份验证**按钮。
    
    ![更改身份验证按钮](./azure-ad-b2c/_static/changeauth.png)

4. 在**更改身份验证**对话框中，选择**单个用户帐户**，然后选择**连接到现有用户存储在云中**下拉列表中。 
    
    ![更改身份验证对话框](./azure-ad-b2c/_static/changeauthdialog.png)

5. 完成窗体具有以下值：
    
    | 设置                       | “值”                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **域名**               | *&lt;B2C 租户的域名&gt;*          |
    | **应用程序 ID**            | *&lt;粘贴剪贴板中的应用程序 ID&gt;* |
    | **回调路径**             | *&lt;使用默认值&gt;*                       |
    | **注册或登录策略** | `B2C_1_SiUpIn`                                        |
    | **重置密码策略**     | `B2C_1_SSPR`                                          |
    | **编辑配置文件策略**       | *&lt;将保留为空&gt;*                                 |
    
    选择**复制**旁边链接**答复 URI**将答复 URI 复制到剪贴板。 选择**确定**关闭**更改身份验证**对话框。 选择**确定**创建 web 应用。

## <a name="finish-the-b2c-app-registration"></a>完成 B2C 应用程序注册

返回到 B2C 应用程序属性仍处于打开状态的浏览器窗口。 更改临时**答复 URL**从 Visual Studio 指定更早版本的值复制。 选择**保存**在窗口的顶部。

> [!TIP]
> 如果未复制的答复 URL，在 web 项目属性中，使用调试选项卡的 SSL 地址并追加**CallbackPath**值从*appsettings.json*。

## <a name="configure-policies"></a>配置策略

使用到的 Azure AD B2C 文档中的步骤[创建注册或登录策略](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)，，然后[创建密码重置策略](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)。 使用的文档中提供的示例值**标识提供程序**，**注册属性**，和**应用程序声明**。 使用**立即运行**是可选的按钮以测试这些策略，如文档中所述。

> [!WARNING]
> 确保策略名称严格按说明在文档中，并与这些策略中使用**更改身份验证**Visual Studio 中的对话框。 策略名称可以验证在*appsettings.json*。

## <a name="run-the-app"></a>运行应用

在 Visual Studio 中，按**F5**生成并运行应用程序。 Web 应用启动后，选择**登录**。

![登录到应用](./azure-ad-b2c/_static/signin.png)

浏览器将重定向到 Azure AD B2C 租户。 使用现有帐户登录 （如果已创建一个测试策略） 或选择**立即注册**创建新帐户。 **忘记了密码？**链接用于重置忘记了的密码。

![Azure AD B2C 登录](./azure-ad-b2c/_static/b2csts.png)

已成功登录后，浏览器将重定向到 web 应用。

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
> * 创建 Azure Active Directory B2C 租户
> * 在 Azure AD B2C 注册某个应用程序
> * 使用 Visual Studio 创建 ASP.NET 核心 Web 应用程序配置为使用 Azure AD B2C 租户进行身份验证
> * 配置控制 Azure AD B2C 租户的行为的策略

现在，ASP.NET Core 应用程序配置为使用 Azure AD B2C 进行身份验证， [Authorize 属性](xref:security/authorization/simple)可用来保护你的应用。 在继续开发你的应用到学习：

* [自定义 Azure AD B2C 用户界面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [配置密码复杂性要求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [启用多因素身份验证](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 配置其他标识提供程序，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。
* [使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)从 Azure AD B2C 租户中检索其他用户信息，例如组成员身份。
* [保护 web API 使用 Azure AD B2C 的 ASP.NET Core](xref:security/authentication/azure-ad-b2c-webapi)。
* [从使用 Azure AD B2C 的.NET web 应用程序调用.NET web API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
