---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "第 6 部分： ASP.NET 成员资格 |Microsoft 文档"
author: JoeStagner
description: "本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 6 部分添加 ASP.NET 成员资格。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a>第 6 部分： ASP.NET 成员资格
====================
通过[Joe stagner 将](https://github.com/JoeStagner)

> Tailspin Spyworks 演示创建在.NET 平台的功能强大、 可扩展应用程序是如何非常简单。 它演示如何使用 ASP.NET 4 中出色的新功能构建在线商店，包括购物、 结帐和管理关闭。
> 
> 本系列教程详细介绍所有生成 Tailspin Spyworks 示例应用程序所采取的步骤。 第 6 部分添加 ASP.NET 成员资格。


## <a id="_Toc260221672"></a>使用 ASP.NET 成员资格

![](tailspin-spyworks-part-6/_static/image1.png)

单击安全

![](tailspin-spyworks-part-6/_static/image1.jpg)

请确保，我们将使用窗体身份验证。

![](tailspin-spyworks-part-6/_static/image2.jpg)

使用"创建用户"链接创建几个用户。

![](tailspin-spyworks-part-6/_static/image3.jpg)

完成后，请参阅解决方案资源管理器窗口，刷新视图。

![](tailspin-spyworks-part-6/_static/image2.png)

请注意，ASPNETDB。已创建 MDF 正常。 此文件包含的表，以支持类似成员身份的核心 ASP.NET 服务。

现在我们可以开始实现在结帐过程。

首先创建 CheckOut.aspx 页。

CheckOut.aspx 页应仅可供用户已登录，因此我们将限制对访问登录用户和重定向用户未登录到登录页。

若要这样做我们将添加以下内容到我们的 web.config 文件的配置节。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

ASP.NET Web 窗体应用程序的模板自动添加到我们的 web.config 文件的身份验证部分，并建立的默认登录页。

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

我们必须修改 Login.aspx 代码隐藏文件在用户登录时，将迁移匿名的购物车。 更改页\_加载事件，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

然后添加一个"LoggedIn"事件处理程序类似设置为新登录用户的会话名称，并通过在我们 MyShoppingCart 类中调用 MigrateCart 方法更改为的用户的购物车中的临时会话 id。 （在.cs 文件中实现）

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

实现 MigrateCart() 方法如下所示。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

在 checkout.aspx 我们将使用 EntityDataSource 和 GridView 我们签出页中一样使用我们未在我们购物车页。

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

请注意，我们 GridView 控件指定"ondatabound"事件处理程序名为 MyList\_RowDataBound 因此让我们实现如下该事件处理程序。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

此方法使每个行绑定以及更新 GridView 的最后一行，购物车的购物运行总计。

在此阶段中，我们已实施顺序，以便放置"评论"演示的文稿。

让我们通过将几行代码添加到我们的页面中处理空购物车方案\_负载事件：

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

当用户单击"提交"按钮时我们将在提交按钮单击事件处理程序中执行下面的代码。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

订单提交过程"肉"是在我们 MyShoppingCart 类的 SubmitOrder() 方法中实现。

将订单将：

- 采用所有行项购物车中并使用它们来创建新的顺序记录和相关联的 OrderDetails 记录。
- 计算传送日期。
- 清除购物车。


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

此示例应用程序供我们将通过只需将两天添加到当前日期计算发货日期。

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

运行应用程序现在将允许我们测试从开始到完成购物过程。

>[!div class="step-by-step"]
[上一页](tailspin-spyworks-part-5.md)
[下一页](tailspin-spyworks-part-7.md)
