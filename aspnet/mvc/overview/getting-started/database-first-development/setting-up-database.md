---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "开始使用 Entity Framework 6 数据库 First 使用 MVC 5 |Microsoft 文档"
author: tfitzmac
description: "使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 此教程系列..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>借助实体框架 6 数据库优先使用 MVC 5 入门
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 实体框架和 ASP.NET 基架，可以创建的 web 应用程序提供了一个接口到现有数据库。 本系列教程演示了如何自动生成代码，使用户能够显示、 编辑、 创建和删除驻留在数据库表中的数据。 生成的代码对应于数据库表中的列。 在序列的最后一部分，将将站点和数据库部署到 Azure。
> 
> 此系列的一部分的重点介绍创建数据库并使用数据填充它。
> 
> Tom Dykstra 和 Rick Anderson 情况下，这一系列是编写使用的。 它已从注释部分中的用户的反馈以提高基于。


## <a name="introduction"></a>介绍

本主题说明如何开始时使用的现有数据库并快速创建的 web 应用程序使用户能够与数据交互。 它使用 Entity Framework 6 和 MVC 5 构建 web 应用程序。 ASP.NET 基架功能可以自动生成用于显示、 更新、 创建和删除数据的代码。 使用 Visual Studio 中的发布工具，你可以轻松地将站点和数据库向 Azure 部署。

本主题介绍这种情况，其中有一个数据库并想要生成基于该数据库的字段上的 web 应用程序的代码。 这种方法称为数据库优先开发。 如果你还没有现有数据库，则可以使用此方式称为 Code First 开发，这就需要定义数据类，并从类属性生成数据库。

Code First 开发的介绍性示例，请参阅[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。 有关更高级的示例，请参阅[为 ASP.NET MVC 4 应用程序创建实体框架数据模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

有关选择使用实体框架方法的指南，请参阅[实体框架开发方法](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。

## <a name="prerequisites"></a>系统必备

Visual Studio 2013 or Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>将数据库设置

若要模拟具有现有数据库的环境，你将首先使用一些预先填充的数据，创建数据库，，然后创建 web 应用程序连接到数据库。

本教程被开发使用 LocalDB Visual Studio 2013 或 Visual Studio Express 2013 for Web。 你可以使用现有的数据库服务器，而不是 LocalDB，但具体取决于版本的 Visual Studio 和你的数据库的类型，所有 Visual Studio 中的数据工具可能不支持。 如果工具不可用于你的数据库，你可能需要执行一些管理套件中的特定于数据库的步骤为你的数据库。

如果你的 Visual Studio 版本中存在数据库工具问题，请确保已安装数据库工具的最新版本。 有关更新或安装数据库工具的信息，请参阅[Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)。

启动 Visual Studio 并创建**SQL Server 数据库项目**。 将项目**ContosoUniversityData**。

![创建数据库项目](setting-up-database/_static/image1.png)

你现在有一个空数据库项目。 你将此将数据库部署到 Azure 稍后在本教程中，因此你需要将 Azure SQL Database 设置为项目的目标平台。 设置目标平台不实际部署该数据库。它只意味着数据库项目将验证数据库设计为与目标平台兼容。 若要设置的目标平台，打开**属性**为该项目并选择**Microsoft Azure SQL Database**为目标平台。

![设置目标平台](setting-up-database/_static/image2.png)

你可以创建本教程需要通过添加定义表的 SQL 脚本的表。 右键单击你的项目并添加新项。

![添加新项](setting-up-database/_static/image3.png)

添加名为学生的新表。

![添加学生表](setting-up-database/_static/image4.png)

表文件中替换下面的代码以创建的表 T-SQL 命令。

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

请注意，设计窗口会自动与代码同步。 你可以使用代码或设计器。

![显示代码和设计](setting-up-database/_static/image5.png)

添加另一个表。 这次将其命名为课程，然后使用以下 T-SQL 命令。

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

然后，重复一次，以创建名为注册的表。

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

你可以通过部署数据库之后运行的脚本的数据与你数据库中填充。 将后期部署脚本添加到项目。 你可以使用默认名称。

![添加后期部署脚本](setting-up-database/_static/image6.png)

将下面的 T-SQL 代码添加到后期部署脚本。 此脚本只需将数据添加到数据库时不找到任何匹配的记录。 不覆盖或删除您可能输入到数据库的任何数据。

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

请务必请注意每次部署您的数据库项目时，运行后期部署脚本。 因此，你需要编写此脚本时请仔细考虑你的要求。 在某些情况下，你可能想要每次部署项目时，通过从一组已知的数据启动。 在其他情况下，你可能不想要更改任何方式中的现有数据。 根据你的要求，你可以决定是否需要后期部署脚本或你需要在脚本中包括。 有关填充后期部署脚本与你数据库的详细信息，请参阅[包括 SQL Server 数据库项目中的数据](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)。

你现在具有 4 个 SQL 脚本文件，但没有实际的表。 你就可以将您的数据库项目部署到 localdb。 在 Visual Studio 中，单击开始按钮 （或 F5） 以生成并部署您的数据库项目。 检查输出选项卡，验证生成和部署成功完成。

![显示输出](setting-up-database/_static/image7.png)

若要查看已创建新数据库，请打开**SQL Server 对象资源管理器**并查找正确的本地数据库服务器中项目的名称 (在这种情况下**(localdb) \ProjectsV12**)。

![显示新的数据库](setting-up-database/_static/image8.png)

若要查看表中填充了数据，右键单击一个表，然后选择**查看数据**。

![显示表数据](setting-up-database/_static/image9.png)

显示表数据的可编辑视图。

![显示表数据的结果](setting-up-database/_static/image10.png)

现在将对你的数据库进行设置，并将其填充数据。 在下一步的教程中，将创建数据库的 web 应用程序。

>[!div class="step-by-step"]
[下一篇](creating-the-web-application.md)
