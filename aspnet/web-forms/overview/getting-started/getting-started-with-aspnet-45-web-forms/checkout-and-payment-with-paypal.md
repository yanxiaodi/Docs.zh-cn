---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: "签出和付款与 PayPal |Microsoft 文档"
author: Erikre
description: "本系列教程将教您生成有关我们使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 的 ASP.NET Web 窗体应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: dd975850a3ed3e7b1746d5123572065675a88656
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="checkout-and-payment-with-paypal"></a>签出和与 PayPal 付款
====================
通过[艾力克 Reitan](https://github.com/Erikre)

[下载 Wingtip Toys 示例项目 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下载电子书 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教程将教您构建使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web 窗体应用程序的基础知识。 Visual Studio 2013[包含 C# 源代码项目](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)是可以附带本系列教程。


本教程介绍如何修改 Wingtip Toys 示例应用程序，以包括用户身份验证、 注册和使用 PayPal 的付款。 只有登录的用户将有授权，以购买产品。 ASP.NET 4.5 Web 窗体项目模板的内置用户注册功能已包含大部分所需内容。 你将添加 PayPal Express 签出功能。 在本教程，你使用测试环境，以便将不传输任何实际的资金 PayPal 开发人员。 在本教程结束时，将通过选择要添加到购物车，单击签出按钮，然后将数据传输到 PayPal 测试网站产品来测试应用程序。 在 PayPal 测试网站上，你将确认传送和付款信息，然后返回到本地 Wingtip Toys 示例应用程序以确认和完成购买。

在线购物，该地址可扩展性和安全性中有几个有经验的第三方支付处理器专用化。 ASP.NET 开发人员应考虑利用在实现购物和购买解决方案之前的第三方支付解决方案的优点。

> [!NOTE] 
> 
> Wingtip Toys 示例应用程序设计为向 ASP.NET web 开发人员显示特定 ASP.NET 概念和功能可用。 此示例应用程序不进行了优化的可伸缩性和安全性方面的所有可能的情况。


## <a name="what-youll-learn"></a>你将学习：

- 如何限制对文件夹中的特定页的访问。
- 如何从匿名的购物车创建已知的购物车。
- 如何为项目启用 SSL。
- 如何向项目添加 OAuth 提供程序。
- 如何使用 PayPal 购买产品使用 PayPal 测试环境。
- 如何显示从 PayPal 中的详细信息**说明**控件。
- 如何使用从 PayPal 获取详细信息更新 Wingtip Toys 应用程序的数据库。

## <a name="adding-order-tracking"></a>添加顺序跟踪

在本教程中，你将创建两个新类来从用户已创建的顺序跟踪数据。 类将跟踪有关传送信息、 购买总计和付款确认数据。

### <a name="add-the-order-and-orderdetail-model-classes"></a>添加的顺序和 OrderDetail 模型类

本系列教程中前面定义的类别，产品，架构以及通过创建项目，购物车`Category`， `Product`，和`CartItem`中的类*模型*文件夹。 现在，你将添加两个新类，以定义为产品订单和订单的详细信息的架构。

1. 在**模型**文件夹中，添加一个名为的新类*Order.cs*。   
 编辑器中将显示新的类文件。
2. 默认代码替换为以下：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. 添加*OrderDetail.cs*类到*模型*文件夹。
4. 默认代码替换为以下代码：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order`和`OrderDetail`类包含此架构定义用于购买和传送的顺序信息。

此外，你将需要更新数据库上下文类，管理的实体类并提供对数据库的数据访问。 若要执行此操作，你将添加新创建的顺序和`OrderDetail`模型的类来`ProductContext`类。

1. 在**解决方案资源管理器**，找到并打开*ProductContext.cs*文件。
2. 添加到突出显示的代码*ProductContext.cs*文件如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

如以前在此系列教程的中的代码中所述*ProductContext.cs*文件将添加`System.Data.Entity`命名空间，以便您具有对实体框架的所有核心功能的访问。 此功能包括查询、 插入、 更新和删除通过使用强类型化对象数据的功能。 在上面的代码`ProductContext`类添加到新添加的实体框架访问`Order`和`OrderDetail`类。

## <a name="adding-checkout-access"></a>添加签出访问

Wingtip Toys 示例应用程序允许匿名用户，以查看并添加到购物车的产品。 但是，当匿名用户选择购买它们添加到购物车的产品，它们就必须登录到站点。 它们具有登录后，他们可以访问的受限的页面的 Web 应用程序处理签出和购买过程。 这些限制的页面包含在*签出*应用程序文件夹。

### <a name="add-a-checkout-folder-and-pages"></a>添加签出文件夹和页

您现在将创建*签出*文件夹和它客户将看到在结帐过程中的页。 在本教程后面，将更新这些网页。

1. 右键单击项目名称 (**Wingtip Toys**) 中**解决方案资源管理器**和选择**添加一个新文件夹**。 

    ![签出和付款与 PayPal 的新文件夹](checkout-and-payment-with-paypal/_static/image1.png)
2. 将新文件夹命名*签出*。
3. 右键单击*签出*文件夹，然后选择**添加**-&gt;**新项**。 

    ![签出和付款与 PayPal-新项](checkout-and-payment-with-paypal/_static/image2.png)
4. 随即出现“添加新项”对话框。
5. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 然后，从中间窗格中，选择**包含母版页的 Web 窗体**并将其命名*CheckoutStart.aspx*。 

    ![签出和付款 PayPal-使用添加新项对话框](checkout-and-payment-with-paypal/_static/image3.png)
6. 如前所述，选择*Site.Master*作为主控页文件。
7. 添加到以下的其他页面*签出*文件夹使用上述相同的步骤：   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>添加 Web.config 文件

添加一个新的*Web.config*文件为*签出*文件夹中，你将能够限制对文件夹中包含的所有页的访问。

1. 右键单击*签出*文件夹，然后选择**添加** - &gt; **新项**。  
 随即出现“添加新项”对话框。
2. 选择**Visual C#**  - &gt; **Web**左侧的模板组。 然后，从中间窗格中，选择**Web 配置文件**，接受默认名称的*Web.config*，然后选择**添加**。
3. 替换的现有 XML 内容中*Web.config*替换为以下文件：  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. 保存*Web.config*文件。

*Web.config*文件指定的 Web 应用程序的所有未知的用户必须被拒绝访问中包含的页*签出*文件夹。 但是，如果用户已注册一个帐户，并且用户登录，则它们将是已知的用户，将有权访问中的页*签出*文件夹。

务必要注意，ASP.NET 配置如下所示层次结构，其中每个*Web.config*文件将配置设置应用到中的文件夹和所有它下面的子目录。

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>为项目启用 SSL

 安全套接字层 (SSL) 是定义为允许 Web 服务器和 Web 客户端通过使用加密更安全地通信协议。 不使用 SSL，则客户端和服务器之间发送数据时，具有到网络的物理访问权限的任何人都探查数据包。 此外，几种常见的身份验证方案是不安全通过一般 HTTP。 具体而言，基本身份验证和窗体身份验证发送未加密的凭据。 为确保安全，这些身份验证方案必须使用 SSL。 

1. 在**解决方案资源管理器**，单击**WingtipToys**项目，然后按**F4**以显示**属性**窗口。
2. 更改**已启用 SSL**到`true`。
3. 复制**SSL URL**以便稍后使用。   
 SSL URL 将为`https://localhost:44300/`除非你之前已创建 SSL 网站 （如下所示）。   
    ![项目属性](checkout-and-payment-with-paypal/_static/image4.png)
4. 在**解决方案资源管理器**，右键单击**WingtipToys**项目，然后单击**属性**。
5. 在左侧选项卡中，单击**Web**。
6. 更改**项目 Url**使用**SSL URL**之前保存。   
    ![项目 Web 属性](checkout-and-payment-with-paypal/_static/image5.png)
7. 通过按保存页面**CTRL + S**。
8. 按**Ctrl + F5**运行该应用程序。 Visual Studio 将显示一个选项可用于避免 SSL 警告。
9. 单击**是**以信任 IIS Express SSL 证书并继续。   
    ![IIS Express SSL 证书详细信息](checkout-and-payment-with-paypal/_static/image6.png)  
 将显示安全警告。
10. 单击**是**将证书安装到本地主机。   
    ![安全警告对话框中](checkout-and-payment-with-paypal/_static/image7.png)  
 将显示浏览器窗口。

你现在可以轻松测试 Web 应用程序本地使用 SSL。

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>添加 OAuth 2.0 提供程序

ASP.NET Web 窗体提供了增强的选项，用于成员身份和身份验证。 这些增强功能包括 OAuth。 OAuth 是一种开放协议，允许以一种简单而标准的方法，从 web、 移动和桌面应用程序进行安全授权。 ASP.NET Web 窗体模板使用 OAuth 公开 Facebook、 Twitter、 Google 和 Microsoft 作为身份验证提供程序。 虽然本教程仅使用 Google 作为身份验证提供程序，可轻松修改代码以使用任何提供程序。 实施其他提供程序的步骤都非常类似于将在本教程中看到的步骤。

除了身份验证，本教程还将使用角色实施授权。 你添加到这些用户`canEdit`角色将能够更改数据 （创建、 编辑或删除联系人）。

> [!NOTE] 
> 
> Windows Live 应用程序只接受的实时工作网站，URL，因此不能用于本地网站 URL 测试登录名。


以下步骤将允许你将添加 Google 身份验证提供程序。

1. 打开*应用\_Start\Startup.Auth.cs*文件。
2. 删除注释字符从`app.UseGoogleAuthentication()`方法以便，该方法显示为如下所示： 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. 导航到[Google 开发人员控制台](https://console.developers.google.com/)。 你还需要使用 Google 开发人员电子邮件帐户 (gmail.com) 登录。 如果你没有 Google 帐户，选择**创建帐户**链接。   
 接下来，你将看到**Google 开发人员控制台**。   
    ![Google 开发人员控制台](checkout-and-payment-with-paypal/_static/image8.png)
4. 单击**创建项目**按钮，然后输入项目名称和 ID （可以使用默认值）。 然后，单击**协议复选框**和**创建**按钮。  

    ![Google-新建项目](checkout-and-payment-with-paypal/_static/image9.png)

 几秒钟后将创建新项目和你的浏览器将显示新项目页。
5. 在左侧选项卡中，单击**Api&amp;身份验证**，然后单击**凭据**。
6. 单击**创建新的客户端 ID**下**OAuth**。   
 **创建客户端 ID**将显示对话框。   
    ![Google-创建客户端 ID](checkout-and-payment-with-paypal/_static/image10.png)
7. 在**创建客户端 ID**对话框中，保留默认值**Web 应用程序**的应用程序类型。
8. 设置**授权的 JavaScript 来源**为之前在本教程中使用的 SSL URL (`https://localhost:44300/`除非你已创建其他 SSL 项目)。   
 此 URL 是你的应用程序的来源。 此示例中，你只需输入 localhost 测试 URL。 但是，您可以输入多个 Url，以针对 localhost 和生产。
9. 设置**授权重定向 URI**所示： 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

 此值是 URI ASP.NET OAuth 用户与 google OAuth 服务器通信。 请记住上面使用的 SSL URL (`https://localhost:44300/`除非你已创建其他 SSL 项目)。
10. 单击**创建客户端 ID**按钮。
11. 在 Google 开发人员控制台的左侧菜单上，单击**同意屏幕**菜单项，然后设置你的电子邮件地址和产品名称。 完成窗体后，单击**保存**。
12. 单击**Api**菜单项，向下的滚动，单击**关闭**按钮旁边**Google + API**。   
 接受此选项将启用 Google + API。
13. 你还必须更新**Microsoft.Owin** 3.0.0 版的 NuGet 程序包。   
 从**工具**菜单上，选择**NuGet 包管理器**，然后选择**管理解决方案的 NuGet 包**。  
 从**管理 NuGet 包**窗口中，找到并更新**Microsoft.Owin** 3.0.0 版的包。
14. 在 Visual Studio 中，更新`UseGoogleAuthentication`方法*Startup.Auth.cs*页上通过复制和粘贴**客户端 ID**和**客户端机密**到方法。 **客户端 ID**和**客户端机密**如下所示的值是示例，不起作用。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. 按**CTRL + F5**生成并运行应用程序。 单击**登录**链接。
16. 下**使用另一个服务以要求在登录**，单击**Google**。  
    ![登录](checkout-and-payment-with-paypal/_static/image11.png)
17. 如果你需要输入你的凭据，你将重定向到 google 站点将在其中输入你的凭据。  
    ![Google-登录](checkout-and-payment-with-paypal/_static/image12.png)
18. 输入你的凭据后，系统将提示，为刚创建的 web 应用程序授予权限。  
    ![项目默认服务帐户](checkout-and-payment-with-paypal/_static/image13.png)
19. 单击**接受**。 你现在会定向回**注册**页**WingtipToys**应用程序可以在其中注册你的 Google 帐户。  
    ![注册你的 Google 帐户](checkout-and-payment-with-paypal/_static/image14.png)
20. 你可以选择更改 Gmail 帐户使用的本地电子邮件注册名称，但你通常想要保留的默认电子邮件别名 （即，一个用于身份验证）。 单击**登录**如上所示。

### <a name="modifying-login-functionality"></a>修改登录功能

如前文所述本系列教程中，大部分用户注册功能已包括在 ASP.NET Web 窗体模板默认情况下。 现在将修改默认*Login.aspx*和*Register.aspx*页面调`MigrateCart`方法。 `MigrateCart`方法将新登录的用户与匿名的购物车相关联。 通过将关联用户和购物车，Wingtip Toys 示例应用程序将无法保持购物车访问之间的用户。

1. 在**解决方案资源管理器**，找到并打开*帐户*文件夹。
2. 修改名为的代码隐藏页*Login.aspx.cs*包括以黄色，突出显示的代码，以使其显示，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. 保存*Login.aspx.cs*文件。

现在，你可以忽略没有为定义该警告`MigrateCart`方法。 你将添加它有点稍后在本教程。

*Login.aspx.cs*代码隐藏文件支持 LogIn 方法。 通过检查 Login.aspx 页面，你将看到此页包含"登录"按钮，当单击触发器`LogIn`上隐藏代码处理程序。

当`Login`方法*Login.aspx.cs*调用时，购物车名为的新实例`usersShoppingCart`创建。 已检索的购物车 (GUID) 的 ID 并将其设置为`cartId`变量。 然后，`MigrateCart`调用方法时，将同时传递`cartId`和登录的用户对此方法的名称。 在迁移购物车，用于标识匿名购物车的 GUID 将替换的用户名称。

除了修改*Login.aspx.cs*代码隐藏文件，以将迁移购物车，当用户登录时还必须修改*Register.aspx.cs 代码隐藏文件*迁移购物车当用户创建新帐户并登录。

1. 在*帐户*文件夹中，打开代码隐藏文件命名为*Register.aspx.cs*。
2. 通过以黄色，包括代码修改的代码隐藏文件，以使其显示，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. 保存*Register.aspx.cs*文件。 同样，有关忽略该警告`MigrateCart`方法。

请注意代码中所使用`CreateUser_Click`事件处理程序是非常类似于你使用的代码`LogIn`方法。 当用户注册或登录到站点，调用`MigrateCart`将成为方法。

## <a name="migrating-the-shopping-cart"></a>迁移购物车

现在，已更新的登录和注册过程，你可以添加代码以迁移购物车使用`MigrateCart`方法。

1. 在**解决方案资源管理器**，查找*逻辑*文件夹并打开*ShoppingCartActions.cs*类文件。
2. 添加代码以到中的现有代码的黄色突出显示*ShoppingCartActions.cs*文件，以便中的代码*ShoppingCartActions.cs*文件将出现，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart`方法使用现有 cartId 查找用户购物车。 接下来，该代码循环访问所有购物车项，并替换`CartId`属性 (所指定的`CartItem`架构) 的登录的用户名。

### <a name="updating-the-database-connection"></a>更新数据库连接

如果按照此教程使用**预构建**Wingtip Toys 示例应用程序，你必须重新创建默认成员资格数据库。 通过修改默认连接字符串，成员资格数据库将创建在下次应用程序运行的时。

1. 打开*Web.config*在项目的根目录的文件。
2. 更新的默认连接字符串，以使其显示，如下所示：   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>将 PayPal 集成

PayPal 是接受通过联机商人付款基于 web 的计费平台。 接下来，本教程介绍如何将 PayPal 的 Express 签出功能集成到你的应用程序。 快速签出允许您的客户能够使用 PayPal 支付费用他们添加到其购物车的项。

### <a name="create-paylpal-test-accounts"></a>创建 PaylPal 测试帐户

若要使用测试环境 PayPal，必须创建并验证开发人员测试帐户。 开发人员测试帐户将用于创建购买者，测试帐户和 seller 测试帐户。 开发人员测试帐户凭据还将允许 Wingtip Toys 示例应用程序访问 PayPal 测试环境。

1. 在浏览器中，导航到测试网站 PayPal 开发人员：   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. 如果你没有 PayPal 开发人员帐户，通过单击创建一个新帐户**注册**并按照步骤注册。 如果你有现有的 PayPal 开发人员帐户，登录通过单击**Log In**。 你将需要 PayPal 开发人员帐户测试本教程中稍后 Wingtip Toys 示例应用程序。
3. 如果你只需注册 PayPal 开发人员帐户，你可能需要验证 PayPal 你 PayPal 开发人员帐户。 可以按照 PayPal 发送到电子邮件帐户的步骤来验证你的帐户。 验证 PayPal 开发人员帐户后，请重新登录到测试网站 PayPal 开发人员。
4. 与需要创建一个 PayPal buyer 测试帐户，如果你已不你 PayPal 开发人员帐户登录到 PayPal 开发人员站点后有一个。 若要创建一个 buyer 测试帐户，在 PayPal 网站，请单击**应用程序**选项卡，然后单击**沙盒帐户**。   
 **沙盒测试帐户**显示页。   

    > [!NOTE] 
    > 
    > PayPal 开发人员网站已经提供了一个商人测试帐户。

    ![签出和付款与 PayPal-沙盒测试帐户](checkout-and-payment-with-paypal/_static/image15.png)
5. 在沙盒测试中的帐户页上单击**创建帐户**。
6. 上**创建测试帐户**页上选择购买者，测试帐户电子邮件和你选择的密码。   

    > [!NOTE] 
    > 
    > 你将需要购买者电子邮件地址和密码以测试本教程末尾的 Wingtip Toys 示例应用程序。

    ![签出和付款与 PayPal-沙盒测试帐户](checkout-and-payment-with-paypal/_static/image16.png)
7. 通过单击创建 buyer 测试帐户**创建帐户**按钮。  
 **沙盒测试帐户**显示页。 

    ![签出和与 PayPal-PaylPal 帐户付款](checkout-and-payment-with-paypal/_static/image17.png)
8. 上**沙盒测试帐户**页上，单击**参与实施者已经**电子邮件帐户。  
    **配置文件**和**通知**显示选项。
9. 选择**配置文件**选项，然后单击**API 凭据**查看商人测试帐户 API 的凭据。
10. 将测试 API 凭据复制到记事本。

你将需要测试环境 PayPal 显示经典测试 API 凭据 （用户名、 密码和签名） 可以从 Wingtip Toys 示例应用程序的 API 调用。 你将在下一步中添加凭据。

### <a name="add-paypal-class-and-api-credentials"></a>添加 PayPal 类和 API 凭据

请将大部分 PayPal 代码成一个类。 此类包含用于与 PayPal 进行通信的方法。 此外，你将与此类添加 PayPal 凭据。

1. 在 Visual Studio 中 Wingtip Toys 示例应用程序中，右键单击**逻辑**文件夹，然后选择**添加** - &gt; **新项**。   
 随即出现“添加新项”对话框。
2. 下**Visual C#**从**已安装**左侧窗格中，选择**代码**。
3. 从中间窗格中，选择**类**。 将此新类**PayPalFunctions.cs**。
4. 单击 **“添加”**。  
 编辑器中将显示新的类文件。
5. 默认代码替换为以下代码：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. 添加，以便您可以进行函数调用到 PayPal 测试环境，在本教程中前面显示的 Merchant API 凭据 （用户名、 密码和签名）。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> 在此示例应用程序要只需将凭据添加到 C# 文件 (.cs)。 但是，在实现解决方案中，应考虑加密你的凭据配置文件中。


NVPAPICaller 类包含大多数 PayPal 功能。 在类中的代码提供了进行一次测试 PayPal 测试环境从购买所需的方法。 在以下三个 PayPal 函数用于进行购买时：

- `SetExpressCheckout`函数
- `GetExpressCheckoutDetails`函数
- `DoExpressCheckoutPayment`函数

`ShortcutExpressCheckout`方法收集测试采购信息和产品详细信息从购物车和调用`SetExpressCheckout`PayPal 函数。 `GetCheckoutDetails`方法确认采购详细信息和调用`GetExpressCheckoutDetails`PayPal 函数在进行测试购买之前。 `DoCheckoutPayment`方法通过调用来完成测试环境中的测试购买`DoExpressCheckoutPayment`PayPal 函数。 剩余代码支持 PayPal 方法和过程，例如编码字符串、 解码字符串、 处理数组，以及确定凭据。

> [!NOTE] 
> 
> PayPal 允许你将包含可选采购详细信息基于[PayPal 的 API 规范](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)。 通过扩展 Wingtip Toys 示例应用程序中的代码，可以包括本地化详细信息、 产品说明、 税金、 客户服务编号，以及许多其他可选字段。


请注意，在指定的返回和取消 Url **ShortcutExpressCheckout**方法使用的端口号。

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Visual Web Developer 运行时使用 SSL 的 web 项目，通常使用的端口 44300 是为 web 服务器。 如上所示，端口号是 44300。 运行应用程序时，你可能会看到不同的端口号。 你端口号必须是正确的代码中设置，以便你可以成功运行本教程末尾 Wingtip Toys 示例应用程序。 本教程的下一节说明如何检索本地主机端口号和更新 PayPal 类。

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>更新 PayPal 类中的 LocalHost 端口号

Wingtip Toys 示例应用程序购买产品，通过导航到 PayPal 测试站点，并返回与你的 Wingtip Toys 示例应用程序的本地实例。 为了具有 PayPal 返回到正确的 URL，你需要指定本地运行的端口号示例上面提到的 PayPal 代码中的应用程序。

1. 右键单击项目名称 (**WingtipToys**) 中**解决方案资源管理器**和选择**属性**。
2. 在左栏中，选择**Web**选项卡。
3. 检索中的端口号**项目 Url**框。
4. 如果需要更新`returnURL`和`cancelURL`PayPal 类中 (`NVPAPICaller`) 中*PayPalFunctions.cs*文件以使用 web 应用程序的端口号：   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

现在你添加的代码将与本地 Web 应用程序的预期的端口匹配。 PayPal 将能够在本地计算机上返回到正确的 URL。

### <a name="add-the-paypal-checkout-button"></a>添加 PayPal 签出按钮

现在，主要 PayPal 功能已添加到示例应用程序，你可以开始添加标记和调用这些函数所需的代码。 首先，必须添加的用户将看到在购物车页上的签出按钮。

1. 打开*ShoppingCart.aspx*文件。
2. 滚动到文件的底部并查找`<!--Checkout Placeholder -->`注释。
3. 将与注释`ImageButton`控制，以便标记替换，如下所示：  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. 在*ShoppingCart.aspx.cs*文件，之后`UpdateBtn_Click`该文件末尾附近的事件处理程序添加`CheckOutBtn_Click`事件处理程序：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. 另外，请在*ShoppingCart.aspx.cs*文件中，添加对引用`CheckoutBtn`，以便新的图像按钮引用，如下所示：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. 将所做的更改保存到同时*ShoppingCart.aspx*文件和*ShoppingCart.aspx.cs*文件。
7. 从菜单中，选择**调试**-&gt;**生成 WingtipToys**。  
 将重新生成项目与新添加**ImageButton**控件。

### <a name="send-purchase-details-to-paypal"></a>向 PayPal 发送采购详细信息

当用户单击**签出**购物车页上的按钮 (*ShoppingCart.aspx*)，它们将首先购买过程。 下面的代码调用购买产品所需的第一个 PayPal 函数。

1. 从*签出*文件夹中，打开代码隐藏文件命名为*CheckoutStart.aspx.cs*。   
 请确保打开代码隐藏文件。
2. 将现有代码替换为以下代码：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

当应用程序的用户单击**签出**按钮在购物车页上，浏览器将导航到*CheckoutStart.aspx*页。 当*CheckoutStart.aspx*页上加载，`ShortcutExpressCheckout`调用方法。 此时，用户会传输到 PayPal 测试网站。 在 PayPal 站点上，用户输入他们 PayPal 凭据、 查看采购详细信息、 接受 PayPal 协议和 Wingtip Toys 示例应用程序返回其中`ShortcutExpressCheckout`方法完成。 当`ShortcutExpressCheckout`方法完成，则会重定向到用户*CheckoutReview.aspx*中指定的网页`ShortcutExpressCheckout`方法。 这允许用户查看订单详细信息从 Wingtip Toys 示例应用程序中。

### <a name="review-order-details"></a>查看顺序的详细信息

后从 PayPal，返回*CheckoutReview.aspx* Wingtip Toys 示例应用程序页显示订单详细信息。 此页面允许用户在购买产品之前查看订单详细信息。 *CheckoutReview.aspx*必须创建页，如下所示：

1. 在*签出*文件夹中，打开页上名为*CheckoutReview.aspx*。
2. 现有的标记替换为以下代码：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. 打开名为的代码隐藏页*CheckoutReview.aspx.cs*和将现有代码替换为以下：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**说明**控件用于显示从 PayPal 返回了订单详细信息。 此外，上面的代码将订单详细信息保存到 Wingtip Toys 数据库作为`OrderDetail`对象。 当用户单击**完整 Order**按钮，将他们重定向到*CheckoutComplete.aspx*页。

> [!NOTE] 
> 
> **提示**
> 
> 中的标记*CheckoutReview.aspx*页上，请注意，`<ItemStyle>`标记用于更改中的项的样式**说明**页面底部附近的控件。 通过查看中的页**设计视图**(通过选择**设计**在 Visual Studio 的左下角)，然后选择**说明**控制，并选择**智能标记**(顶部的箭头图标右侧的控件)，你将能够看到**说明任务**。
> 
> ![签出和付款与 PayPal-编辑字段](checkout-and-payment-with-paypal/_static/image18.png)
> 
> 通过选择**编辑字段**、**字段**对话框将出现。 在此对话框中您可以轻松地控制可视属性，如**ItemStyle**的**说明**控件。
> 
> ![签出和付款与 PayPal-字段对话框](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>完成购买

*CheckoutComplete.aspx*页从 PayPal 进行购买。 如上所述，用户必须单击**完整 Order**按钮之前应用程序将会定位到*CheckoutComplete.aspx*页。

1. 在*签出*文件夹中，打开页上名为*CheckoutComplete.aspx*。
2. 现有的标记替换为以下代码：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. 打开名为的代码隐藏页*CheckoutComplete.aspx.cs*和将现有代码替换为以下：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

当*CheckoutComplete.aspx*加载页时，`DoCheckoutPayment`调用方法。 如前所述，`DoCheckoutPayment`方法完成从 PayPal 测试环境购买。 PayPal 完成后的顺序，购买*CheckoutComplete.aspx*页显示付款事务`ID`到购买者。

### <a name="handle-cancel-purchase"></a>处理取消购买

如果用户决定取消购买，它们将定向到*CheckoutCancel.aspx*页上，它们将看到已取消其顺序。

1. 打开名为的页*CheckoutCancel.aspx*中*签出*文件夹。
2. 现有的标记替换为以下代码：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>处理购买错误

在购买过程中的错误将由*CheckoutError.aspx*页。 代码隐藏*CheckoutStart.aspx*页上， *CheckoutReview.aspx*页上，与*CheckoutComplete.aspx*页面将每个重定向到*CheckoutError.aspx*页上，如果发生错误。

1. 打开名为的页*CheckoutError.aspx*中*签出*文件夹。
2. 现有的标记替换为以下代码：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx*在结帐过程中发生错误时，页将显示的错误详细信息。

## <a name="running-the-application"></a>运行应用程序

运行应用程序以了解如何购买产品。 请注意，你会在中运行 PayPal 测试环境。 要不交换的任何实际的资金。

1. 请确保所有在 Visual Studio 中保存你的文件。
2. 打开 Web 浏览器并导航到[https://developer.paypal.com](https://developer.paypal.com/)。
3. 使用你此前在本教程中创建的 PayPal 开发人员帐户登录。  
 对于 PayPal 的开发人员沙盒，你需要在登录[https://developer.paypal.com](https://developer.paypal.com/)测试 express 签出。 这仅适用于测试，不适用于 PayPal 的实时环境 PayPal 的沙盒。
4. 在 Visual Studio 中，按**F5**运行 Wingtip Toys 示例应用程序。  
 浏览器将重新生成数据库后，将打开并显示*Default.aspx*页。
5. 将三个不同的产品添加到购物车中，通过选择产品类别，如"汽车"，然后单击**将添加到购物车**旁边每个产品。  
 购物车将显示所选的产品。
6. 单击**PayPal**签出的按钮。 

    ![签出和付款与 PayPal-购物车](checkout-and-payment-with-paypal/_static/image20.png)

 签出将要求你具有用于 Wingtip Toys 示例应用程序的用户帐户。
7. 单击**Google**在能够使用现有 gmail.com 电子邮件帐户进行登录页的右侧的链接。  
 如果没有 gmail.com 帐户，则可以创建一个用于测试目的在[www.gmail.com](https://www.gmail.com/)。此外可以通过单击"注册"使用标准的本地帐户。 

    ![签出和付款与 PayPal-登录](checkout-and-payment-with-paypal/_static/image21.png)
8. 使用你 gmail 帐户和密码登录。 

    ![签出和付款与 PayPal-Gmail 登录](checkout-and-payment-with-paypal/_static/image22.png)
9. 单击**登录**按钮以将你的 gmail 帐户注册你 Wingtip Toys 示例应用程序的用户名。 

    ![签出和付款与 PayPal-注册帐户](checkout-and-payment-with-paypal/_static/image23.png)
10. 在 PayPal 测试站点上，添加你**buyer**电子邮件地址以及你在本教程中，前面创建的密码，然后单击**Log In**按钮。 

    ![签出和付款与 PayPal-PayPal 登录](checkout-and-payment-with-paypal/_static/image24.png)
11. 同意 PayPal 策略并单击**同意并继续**按钮。  
 请注意，此页才显示第一次使用此 PayPal 帐户。 再次请注意，这是一个测试帐户，没有真正的现金进行交换。 

    ![签出和付款与 PayPal-PayPal 策略](checkout-and-payment-with-paypal/_static/image25.png)
12. 查看在测试环境审阅页面上，单击 PayPal 订单信息**继续**。 

    ![签出和付款与 PayPal-查看信息](checkout-and-payment-with-paypal/_static/image26.png)
13. 上*CheckoutReview.aspx*页上，验证订单量并查看生成的邮寄地址。 然后，单击**完整 Order**按钮。 

    ![签出和付款与 PayPal-订单审核](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx**页显示带有付款事务 id。 

    ![签出和与 PayPal-签出完整的付款](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>查看数据库

通过运行应用程序后查看 Wingtip Toys 示例应用程序数据库中更新后的数据，你可以看到应用程序成功记录购买产品。

你可以检查中包含的数据*Wingtiptoys.mdf*数据库文件使用**数据库资源管理器**窗口 (**服务器资源管理器**Visual Studio 窗口中的) 就像之前在本教程系列。

1. 如果它仍处于打开状态，请关闭浏览器窗口。
2. 在 Visual Studio 中，选择**显示所有文件**顶部的图标**解决方案资源管理器**以便您可以展开**应用\_数据**文件夹。
3. 展开**应用\_数据**文件夹。  
 你可能需要选择**显示所有文件**文件夹图标。
4. 右键单击*Wingtiptoys.mdf*数据库文件并选择**打开**。  
    **服务器资源管理器**显示。
5. 展开**表**文件夹。
6. 右键单击**订单**表，然后选择**显示表数据**。  
 **订单**显示表。
7. 查看**PaymentTransactionID**列以确认成功的事务。 

    ![签出和 PayPal-查看数据库使用的付款](checkout-and-payment-with-paypal/_static/image29.png)
8. 关闭**订单**表窗口。
9. 在服务器资源管理器，右键单击**OrderDetails**表，然后选择**显示表数据**。
10. 查看`OrderId`和`Username`中值**OrderDetails**表。 请注意，这些值匹配`OrderId`和`Username`中包含的值**订单**表。
11. 关闭**OrderDetails**表窗口。
12. 右键单击 Wingtip Toys 数据库文件 (*Wingtiptoys.mdf*)，然后选择**关闭连接**。
13. 如果看不到**解决方案资源管理器**窗口中，单击**解决方案资源管理器**底部**服务器资源管理器**窗口以显示**解决方案资源管理器**试。

## <a name="summary"></a>摘要

在本教程中你添加了订单和订单详细信息架构跟踪购买产品。 你还可以集成到 Wingtip Toys 示例应用程序 PayPal 功能。

## <a name="additional-resources"></a>其他资源

[ASP.NET 配置概述](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[将包含成员资格、 OAuth 和 SQL 数据库的安全的 ASP.NET Web 窗体应用程序部署到 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure 的免费试用版](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>免责声明

本教程包含示例代码。 此类示例代码"按原样"提供，而无需任何形式的保证。 相应地，Microsoft 不保证准确性、 完整性或对示例代码质量。 您同意在你自己的风险时使用的示例代码。 在任何情况下将 Microsoft 的应承担给你以任何方式内容，包括但不是限于任何错误或遗漏的任何示例代码、 内容，或任何丢失或损坏导致的任何示例代码使用产生任何任何的类型示例代码。 特此收到通知，特此同意赔偿，保存并保存 Microsoft 无害从和针对所有损失、 丢失、 受伤或的任何类型的包括但不限于，那些由 occasioned 或因你发布的材料的损坏的声明传输、 使用或依赖于包括但不是限于表示其中的视图。

>[!div class="step-by-step"]
[上一页](shopping-cart.md)
[下一页](membership-and-administration.md)
