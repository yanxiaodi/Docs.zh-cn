---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "运行不同版本的 ASP.NET Web 页 (Razor) 并排 |Microsoft 文档"
author: tfitzmac
description: "此文章介绍了如何在相同的计算机或服务器上运行 ASP.NET Web 页 (Razor) 网站，在网站均配置为使用不同版本..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>并行运行不同版本的 ASP.NET Web 页 (Razor)
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何在相同的计算机或服务器上运行 ASP.NET Web 页 (Razor) 网站，在网站均配置为使用不同版本的 ASP.NET Web 页。
> 
> 你将学习：
> 
> - 新增功能的默认行为是在 ASP.NET 中你必须生成与 ASP.NET Web 页的站点。
> - 如何配置新站点以运行与旧版本的 ASP.NET Web 页。
>   
> 
> 这是文章中引入的 ASP.NET 功能：
> 
> - `webPages:Version`配置设置。
>   
> 
> ## <a name="software-versions"></a>软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。


ASP.NET Web Pages 支持能够运行网站并排显示。 这使你能够继续运行较旧的 ASP.NET Web Pages 应用程序，生成新的 ASP.NET Web Pages 应用程序，并在同一台计算机上运行所有这些。

下面是一些需要使用 WebMatrix 安装 Web 页时，应注意的事项：

- 默认情况下，现有的 Web 页面应用程序将在计算机上运行的最新版本。 （程序集安装在全局程序集缓存 (GAC) 并自动使用。）
- 如果你想要运行站点使用不同版本的 ASP.NET 网页，你可以配置站点后，若要这样做。 如果你的站点还没有*web.config*文件在站点的根目录中，创建一个新然后将以下 XML 复制到其中，覆盖现有的内容。 如果该站点已包含*web.config*文件中，添加`<appSettings>`到以下所示的元素`<configuration>`部分。

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
-如果不指定中的版本*web.config*文件，站点部署为最新版本。 (程序集复制到*bin*已部署的站点中的文件夹。)
- Web 矩阵中使用的站点模板创建的新应用程序包括在站点中的网页版程序集*bin*文件夹。

一般情况下，你可以始终控制哪个版本的 Web 页面以通过使用 NuGet 将相应的程序集安装到站点的使用与您的网站*bin*文件夹。 若要查找程序包，请访问[NuGet.org](http://NuGet.org)。

## <a name="additional-resources"></a>其他资源

[在 ASP.NET Web Pages 2 靠前的特征](top-features-in-web-pages-2.md)
