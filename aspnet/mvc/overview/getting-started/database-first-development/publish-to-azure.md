---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "将 MVC 数据库第一个站点发布到 Azure |Microsoft 文档"
author: tfitzmac
description: "使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 此教程系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>将 MVC 数据库第一个站点发布到 Azure
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 本系列教程演示了如何自动生成代码，使用户能够显示、 编辑、 创建和删除驻留在数据库表中的数据。 生成的代码对应于数据库表中的列。
> 
> 此系列的一部分注重于发布到 Azure 的 web 应用和数据库。 你可以阅读本主题了解有关发布的 web 应用和数据库，但实际执行的步骤，你必须在本教程开始时启动。 请参阅[入门](setting-up-database.md)。


## <a name="deploy-your-web-app-on-azure"></a>Web 应用在 Azure 上部署

你需要一个 Azure 帐户来完成本教程：

- 你可以[免费建立一个 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-获取信用额度来试用付费版 Azure 服务，你可以使用和甚至在用完后，最多可以保留帐户和使用免费的 Azure 服务。
- 你可以[激活 MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-MSDN 订阅为你的信用额度可以用于付费版 Azure 服务的每个月。

若要发布你的 web 应用，请右键单击项目，然后选择**发布**。

![发布站点](publish-to-azure/_static/image1.png)

选择 Microsoft Azure 网站。

![选择 Azure](publish-to-azure/_static/image2.png)

如果您未登录到 Azure，提供你的 Azure 帐户凭据。 然后，选择新建以创建新的 web 应用程序。

![新站点](publish-to-azure/_static/image3.png)

创建你的 web 应用的唯一名称。 如果你看到名称字段右侧一个绿色复选标记，将知道名称是唯一的。 选择你的 web 应用的区域。 选择**创建新的服务器**对于数据库，并为此新的数据库服务器提供用户名和密码。 完成后，单击**创建**。

![创建站点](publish-to-azure/_static/image4.png)

你连接的值现在所有设置。 你可以将这些值保持不变。

![connection](publish-to-azure/_static/image5.png)

单击 **“下一步”**。

对于设置，请指定两个数据库连接的通知-ApplicationDBContext 和 ContosoUniversityDataEntities。 ApplicationDBContext 是用户帐户表的连接。 这些值仅显示数据库的连接字符串。 它并不意味着在发布你的站点时，将发布这些数据库。 发布 web 应用完后，将发布数据库项目。

![](publish-to-azure/_static/image6.png)

数据库连接旁边的省略号 （...） 显示连接字符串的详细信息。 单击 ContosoUniversityDataEntities 旁边的省略号。

![](publish-to-azure/_static/image7.png)

记下数据库服务器和数据库的名称。 随机生成的服务器名称。 数据库名称是简单的您的站点名称 **\_db**追加。 在发布你的数据库时，将更高版本需要这两个名称。

单击**确定**以关闭数据库连接字符串窗口。

在发布 Web 窗口中，单击**下一步**要查看预览。

![](publish-to-azure/_static/image8.png)

你可以单击开始预览以查看要发布的文件列表。 由于这是首次发布此站点，列表将为每个相关项目文件中。

单击“发布” 。

输出窗格将显示发布的结果。

![](publish-to-azure/_static/image9.png)

发布后，该站点处于 immedialely 在 web 浏览器中启动。 已部署你的站点，您可以注册新的用户访问该站点;但是，在 ContosoUniversityData 项目中的表具有尚未发布。 如果你单击的学生链接列表将收到错误。

## <a name="publish-database-to-sql-azure"></a>将数据库发布到 SQL Azure

在发布之前你的数据库，你必须确保本地计算机可以连接到数据库服务器。 数据库服务器的防火墙限制哪些计算机可以连接到数据库。 你需要将你的计算机的 IP 地址添加到防火墙允许的 IP 地址。

登录到你通过 Azure 门户的 Azure 帐户。

选择新的数据库，然后选择**管理**。

![管理](publish-to-azure/_static/image10.png)

你必须配置数据库服务器以允许从你的计算机的连接。 当选择管理时，系统会要求你将当前的 IP 地址添加到数据库服务器许可。 选择是。

![添加 ip 地址](publish-to-azure/_static/image11.png)

没有机会在上一步中添加的 IP 地址不是你需要为连接配置的唯一 IP 地址。 你可以尝试登录到数据库以查看是否已正确设置连接。 提供的用户和更早版本创建的密码。

![登录](publish-to-azure/_static/image12.png)

如果你收到一条错误消息，你需要添加另一个 IP 地址。 单击要查看有关错误的详细信息的错误消息。 在详细信息，你将看到你需要添加的 IP 地址。 请注意此 IP 地址。

![不允许](publish-to-azure/_static/image13.png)

关闭此登录窗口中，并返回到 Azure 门户。

导航到你的数据库的仪表板。 单击**管理允许的 IP 地址**。

![管理 ip 地址](publish-to-azure/_static/image14.png)

你现在必须从错误消息中添加的 IP 地址。 更改允许的 IP 地址，以包括错误消息中，从一个范围，或者将该 IP 地址添加为一个单独的条目。

![添加新地址](publish-to-azure/_static/image15.png)

将更改保存到允许的 IP 地址。

单击管理，并尝试再次登录到数据库。 你可能需要等待几分钟，然后允许的 IP 地址正确配置防火墙。 如果成功在数据库中进行登录，你完成设置后数据库的连接。

您可以将此管理窗口设置为打开，因为你将很快检查您的数据库部署的结果。

返回到您的数据库项目。 右键单击项目并选择**发布**。

![发布数据库](publish-to-azure/_static/image16.png)

在发布数据库窗口中，选择**编辑**。

![编辑](publish-to-azure/_static/image17.png)

为服务器提供的数据库服务器和身份验证凭据的名称。 在提供凭据之后, 选择从可用数据库列表创建的数据库。 默认情况下，Visual Studio 将数据库字段的名称设置为你项目，这可能不是你创建的数据库相同的名称。

![](publish-to-azure/_static/image18.png)

单击“确定”。

你将可能想要保存此配置文件，以便您可以将更新在将来发布而无需重新输入的所有连接信息。 选择“创建配置文件”。

![保存配置文件](publish-to-azure/_static/image19.png)

它将在名为的项目中创建文件**ContosoUniversityData.publish.xml**。 下次你想要将数据库发布到 Azure，只需加载该配置文件。

现在，单击**发布**在 Azure 上创建数据库。

![publish](publish-to-azure/_static/image20.png)

运行一段时间之后显示发布的结果。

![results](publish-to-azure/_static/image21.png)

现在，你可以返回到管理门户为你的数据库。 刷新设计视图中，并注意已部署的 3 表使用预先填充的数据。

![新表](publish-to-azure/_static/image22.png)

现在你已准备好测试 web 应用程序部署到 Azure。 导航到 Azure 上的 web 应用程序 （如 http://contosositeexample.azurewebsites.net/)。 单击链接可以获得的学生的列表，您应看到学生的索引视图。

![查看](publish-to-azure/_static/image23.png)

有时，数据库和连接需要一点时间来正确配置。 如果你收到错误，等待几分钟，然后再试。

## <a name="conclusion"></a>结束语

这一系列提供如何从现有数据库，使用户能够编辑、 更新、 创建和删除数据生成代码的简单示例。 它使用 ASP.NET MVC 5，实体框架和 ASP.NET 基架创建项目。

Code First 开发的介绍性示例，请参阅[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。

有关更高级的示例，请参阅[为 ASP.NET MVC 4 应用程序创建实体框架数据模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 请注意，使用第一个数据库中的数据使用 DbContext API 是用于在第一个代码中使用数据的 API 相同。 即使你想要使用第一个数据库，你可以了解如何处理更复杂的方案，如读取和更新相关的数据，处理并发冲突，Code First 教程中，依此类推。 如何创建数据库、 上下文类和实体类中是唯一的区别。

>[!div class="step-by-step"]
[上一篇](enhancing-data-validation.md)
