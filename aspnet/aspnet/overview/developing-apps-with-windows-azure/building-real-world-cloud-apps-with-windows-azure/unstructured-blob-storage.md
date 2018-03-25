---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "非结构化的 Blob 存储 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 489769533a26c99404c6a5186d66f560385dcffd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>非结构化的 Blob 存储 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


在前一章中我们将查看分区方案，并且说明了如何修复它应用在 Azure 存储 Blob 服务和 Azure SQL 数据库中的其他任务数据中存储图像。 本章我们将深入查看 Blob 服务，并显示如何解决它的项目代码中实现。

## <a name="what-is-blob-storage"></a>什么是 Blob 存储？

Azure 存储 Blob 服务使您能够在云中存储文件。 Blob 服务具有大量通过将文件存储在本地网络文件系统的优点：

- 它是高度可伸缩的。 单个存储帐户可以存储[数百个亿字节](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)，并且可有多个存储帐户。 最大的 Azure 客户的一些存储数百个 petabytes。 Microsoft SkyDrive 使用 blob 存储。
- 它是持久事务。 Blob 服务中存储每个文件自动备份。
- 它提供了高可用性。 [SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409)承诺 99.9%或 99.99%的正常运行时间，根据所选的地域冗余选项。
- 它是 Azure 中，这意味着你只需存储和检索文件，仅为你使用的存储的实际数量付费的平台即服务 (PaaS) 功能和 Azure 会自动负责设置和管理的所有 Vm 和所需的磁盘驱动器服务。
- 通过使用 REST API 或通过使用 API 的编程语言，你可以访问 Blob 服务。 Sdk 都适用于.NET、 Java、 Ruby 和其他人。
- 文件存储在 Blob 服务中时，你可以轻松地使其公开可通过 Internet。
- 你可以保护服务，以便他们可以访问只能由经过授权的用户，Blob 中的文件，或者可以提供使它们可向某人仅为有限的时间段的临时访问令牌。

每当你正在 azure 中生成应用程序并且你想要存储大量数据，在本地环境中将转中文件-如图像、 视频、 Pdf、 电子表格等-请考虑 Blob 服务。

## <a name="creating-a-storage-account"></a>创建存储帐户

若要开始使用 Blob 服务在 Azure 中创建存储帐户。 在门户中，单击**新建** -- **Data Services** -- **存储** -- **快速创建**，然后输入一个 URL，数据中心位置。 数据中心位置应与你的 web 应用相同。

![创建存储帐户](unstructured-blob-storage/_static/image1.png)

你想要存储的内容，并且如果你选择选取主区域[异地复制](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)选项，Azure 在不同的数据中心在国家/地区的另一个区域中创建的所有数据的副本。 例如，如果你选择西方美国数据中心，它还将到美国西部数据中心，但在后台 Azure 文件存储时将其复制到其他美国数据中心之一。 如果在一个国家 / 地区在发生灾难，你的数据是仍安全的。

Azure 不会跨地缘政治边界复制数据： 如果你的主位置是在美国，你的文件将仅复制到美国; 内的另一个区域如果您的主要位置，澳大利亚你的文件将仅复制到在澳大利亚的另一个数据中心。

当然，你还可以创建存储帐户通过从脚本中执行命令如我们前面看到的。 下面是 Windows PowerShell 命令以创建存储帐户：

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

存储帐户之后，你可以立即开始将文件存储在 Blob 服务。

## <a name="using-blob-storage-in-the-fix-it-app"></a>修复它应用中使用 Blob 存储

修复它应用允许你上载照片。

![创建修复它任务](unstructured-blob-storage/_static/image2.png)

当你单击**创建 FixIt**，应用程序将指定的图像文件上载并将其存储在 Blob 服务。

### <a name="set-up-the-blob-container"></a>设置 Blob 容器

若要将文件存储在您需要的 Blob 服务*容器*以将其存储在。 Blob 服务容器对应于文件系统文件夹。 我们回顾了中的环境创建脚本[使一切自动化章](automate-everything.md)创建存储帐户中，但它们不创建容器。 因此的用途`CreateAndConfigure`方法`PhotoService`类是创建一个容器，如果它尚不存在。 此方法调用从`Application_Start`中的方法*Global.asax*。

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

存储帐户名称和访问密钥存储在`appSettings`集合*Web.config*文件中，并在代码`StorageUtils.StorageAccount`方法使用这些值来生成连接字符串和建立连接：

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync`方法会创建一个表示 Blob 服务中，对象和一个对象，表示容器 （文件夹） Blob 服务中名为"images":

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

如果容器名为"images"尚不存在-这将是第一次针对新的存储帐户-运行应用程序代码创建的容器，并将设置设为公开的权限，则返回 true。 （默认情况下，新的 blob 容器是私有，可访问仅向用户有权访问你的存储帐户。）

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>在 Blob 存储中存储的已上载的照片

上载和保存该图像文件，应用程序使用`IPhotoService`接口和一个实现中的接口`PhotoService`类。 *PhotoService.cs*文件包含所有应用中的代码修复它与 Blob 服务进行通信。

下面的 MVC 控制器方法调用当用户单击**创建 FixIt**。 在此代码中，`photoService`的实例是指`PhotoService`类，和`fixittask`的实例是指`FixItTask`将新任务的数据存储的实体类。

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync`中的方法`PhotoService`类将上载的文件存储在 Blob 服务，并返回指向新的 blob 的 URL。

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

作为 in`CreateAndConfigure`方法，代码连接到存储帐户，然后创建一个表示对象的"映像"blob 容器，但此处假定该容器已存在。

然后它将创建要由新的 GUID 值与文件扩展名串联上载的图像的唯一标识符：

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

然后的代码使用 blob 容器对象和新的唯一标识符来创建 blob 对象，则在哪种文件它，，然后使用 blob 对象存储在 blob 存储中的文件，该值指示该对象上设置属性。

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

最后，它获取引用 blob 的 URL。 此 URL 将存储在数据库和可以用于解决它的网页中显示已上载的图像。

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

此 URL 作为一个 FixItTask 表的列存储在数据库中。

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

使用仅在数据库中，URL 和 Blob 存储中的映像中，修复它应用以使数据库保持小、 可伸缩且成本较低时其中存储为比较便宜，能够处理兆兆字节乃至 petabytes 存储映像。 一个存储帐户可以存储数百万亿字节的修复它照片，而且你只需为你的使用付费。 因此你可以首先小付费的 9 美分的第一个千兆字节，并为其他 /gb 便士添加更多的图像。

### <a name="display-the-uploaded-file"></a>显示已上载的文件

显示的任务的详细信息时，修复它应用程序将显示已上载的图像文件。

![修复它的照片的任务详细信息](unstructured-blob-storage/_static/image3.png)

若要显示图像，所有的 MVC 视图都只需是包括`PhotoUrl`HTML 发送到浏览器中的值。 Web 服务器和数据库不会使用循环显示图像，它们仅向图像 URL 提供了几个字节。 在以下 Razor 代码中，`Model`的实例是指`FixItTask`实体类。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

如果你查看上显示的页的 HTML，你将看到直接指向中 blob 存储，如下所示的图像的 URL:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>摘要

你已了解如何解决它应用将映像存储在 Blob 服务和映像 Url 仅在 SQL 数据库中。 使用 Blob 服务使 SQL 数据库不是它否则为有可能增加到几乎无限数量的任务，而可以完成而无需编写大量的代码要小得多。

你可以在存储帐户，有数百个亿字节而存储成本是价格比 SQL 数据库存储，开始大约 3 美分 /gb 情况下，每个月加上较小的事务的费用更便宜。 和，请记住，您不特别的最大容量，但仅针对你实际上存储，因此你的应用已准备好缩放但您不特别所有这些额外的容量的量。

在[下一章](design-to-survive-failures.md)我们将讨论使云应用程序能够正常处理失败的重要性。

## <a name="resources"></a>资源

有关详细信息请参阅以下资源：

- [Azure BLOB 存储空间简介](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)。 Mike 木材的博客。
- [如何在.NET 中使用 Azure Blob 存储服务](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)。 MicrosoftAzure.com 网站上的正式文档。 简要介绍到 blob 存储跟代码示例演示如何连接到 blob 存储创建容器、 上载和下载 blob，等等。
- [防故障： 构建可扩展、 有弹性的云服务](https://channel9.msdn.com/Series/FailSafe)。 通过 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九一部分视频系列。 高级概念和体系结构原理以非常可访问且有趣方式，提供与 Microsoft 客户咨询团队 (CAT) 体验与实际客户从绘制的情景。 有关 Azure 存储服务和 blob 的讨论，请参阅段 5 开始 35:13。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅 Valet 密钥模式。

>[!div class="step-by-step"]
[上一页](data-partitioning-strategies.md)
[下一页](design-to-survive-failures.md)
