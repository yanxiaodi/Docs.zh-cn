---
uid: mvc/overview/advanced/custom-mvc-templates
title: "自定义 MVC 模板 |Microsoft 文档"
author: joeloff
description: "创建模板作为 VSIX 扩展。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>自定义 MVC 模板
====================
通过[Jacques Eloff](https://github.com/joeloff)

对于 Visual Studio 2010 的 MVC 3 Tools 更新版本引入了单独的项目向导的 MVC 项目。 之所以更改是两个因素。 首先，在 MVC 3 和支持的其他视图引擎，如 Razor 的新模板的引入导致 overcrowding Visual Studio 中的新建项目对话框。 其次，必须一直客户要求提供扩展点和新的 MVC 项目向导将提供我们机会响应这些请求。

添加自定义模板时棘手的过程依赖于使用注册表来使新模板对 MVC 项目向导可见。 新模板的作者必须将其包装在 MSI，确保将在安装时创建必要的注册表项。 替代项是确保包含可用模板的 ZIP 文件并且没有最终用户手动创建必需的注册表项。

前面提到的方法都不能是理想的因此，我们决定利用提供的现有基础结构的某些[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)扩展要更加方便会作者，分发和安装自定义从 MVC 4 开始的 MVC 模板Visual Studio 2012。 此方法提供的好处包括：

- 在 VSIX 扩展可以包含多个模板，它们支持不同的语言 （C# 和 Visual Basic） 和多个视图引擎 （ASPX 和 Razor）。
- 在 VSIX 扩展可以面向多个 Sku 的 Visual Studio 包括 Express Sku。
- [Visual Studio 库](https://visualstudiogallery.msdn.microsoft.com/)促进分发给广泛的受众的扩展。
- 使其更轻松地创作更正和更新到自定义模板，可以升级 VSIX 扩展。

## <a name="prerequisites"></a>系统必备

- 用户需要熟悉创作项目模板，其中包括所需的标记 vstemplate 文件，等等。
- 用户将需要具有 Visual Studio 专业版和更高的版本。 Express Sku 不支持创建 VSIX 项目。
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)安装。

## <a name="example"></a>示例

第一步是创建使用 C# 或 Visual Basic 的新 VSIX 项目。 选择**文件 > 新建项目**，然后单击**扩展性**在左窗格中，选择**VSIX 项目**。

![新建项目](custom-mvc-templates/_static/image1.jpg)

创建项目后，将打开 VSIX 设计器。

![项目设计器元数据](custom-mvc-templates/_static/image2.jpg)

设计器可以用于编辑某些时它们安装扩展或浏览已安装的扩展 Visual Studio 中向用户显示的扩展的常规属性 (**工具 > 扩展和更新**)。 完成后单击的常规信息**安装目标选项卡**。

![项目设计器的安装目标](custom-mvc-templates/_static/image3.jpg)

此选项卡用于指定的 Sku 和由你的扩展支持版本的 Visual Studio。 选中的复选框**的所有用户安装此 VSIX**若要启用每台计算机安装的 VSIX。 单击**新建**右侧添加其他 Sku 如 Web 开发人员 Express (VWD) 按钮。

![添加新的安装目标](custom-mvc-templates/_static/image4.jpg)

如果你想要支持所有专业版和更高版本的 Sku （Professional、 Premium 和 Ultimate） 只需在系列中，选择最小 SKU **Microsoft.VisualStudio.Pro**。 请记住保存所有更改完成后安装的目标。

![项目设计器的安装目标](custom-mvc-templates/_static/image5.jpg)

**资产**选项卡用于将所有内容文件添加到 VSIX。 因为 MVC 需要自定义元数据将编辑而不是使用 VSIX 清单文件的原始 XML**资产**选项卡添加内容。 首先，通过添加到 VSIX 项目的模板内容。 很重要的文件夹和内容的结构镜像项目的布局。 下面的示例包含从基本的 MVC 项目模板派生的四个项目模板。 请确保构成你的项目模板 （ProjectTemplates 文件夹下的所有内容） 的所有文件将都添加到**内容**o u p 在 VSIX 项目文件和每个项包含**CopyToOutputDirectory**和**IncludeInVsix**设置元数据，如下面的示例中所示。

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

否则，IDE 将尝试时生成 VSIX 并可能会看到如下错误编译模板的内容。 通常，在模板中的代码文件包含特殊[模板参数](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)时项目模板实例化并因此不能在 IDE 中编译使用 Visual Studio。

![“解决方案资源管理器”](custom-mvc-templates/_static/image6.jpg)

关闭 VSIX 设计器中，然后右键单击**source.extension.manifest**文件中**解决方案资源管理器**和选择**打开**选择**XML (文本） 编辑器**选项。

![打开与对话框](custom-mvc-templates/_static/image7.jpg)

创建**&lt;资产&gt;**元素和添加**&lt;资产&gt;**必须包括在 VSIX 每个文件的元素。 **类型**每个属性**&lt;资产&gt;**元素必须设置为**Microsoft.VisualStudio.Mvc.Template**。 这是 MVC 的项目向导理解的自定义命名空间。 请参阅有关其他信息的结构和清单文件布局的 VSIX 2.0 架构文档。

只需将文件添加到 VSIX 不足以使用 MVC 向导注册模板。 你需要向 MVC 向导提供如模板名称、 描述、 受支持的视图引擎和编程语言的信息。 与相关联的自定义特性中携带有此信息**&lt;资产&gt;**每个元素**vstemplate**文件。

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

标题 =&quot;自定义基本 Web 应用程序&quot;

说明 =&quot;自定义模板派生自基本 MVC web 应用程序 (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

下面是必须存在的自定义属性的说明：

- **ProjectType**必须设置为 MVC。
- **语言**指定模板支持的开发语言。 有效值为 C# 或 VB.
- **ViewEngine**指定支持的模板，例如 Aspx 或 Razor 视图引擎。 你可以指定此字段的自定义值。
- **TemplateId**用于对模板进行分组。 如果值与匹配的现有模板 ID，它将重写使用 MVC 向导以前注册的模板。
- **标题**指定下每个项目模板 MVC 向导中显示的简短描述。
- **说明**指定模板的更详细说明。

所有的文件添加到清单并保存它，你将注意到，后**资产**设计器中的选项卡将显示所有文件，但不是的自定义特性添加到**&lt;资产&gt;**元素**vstemplate**文件。

![项目设计器资产](custom-mvc-templates/_static/image8.jpg)

所有这些剩下现在是编译 VSIX 项目并将其安装。

请确保在计算机上的 Visual Studio 的所有实例均已都关闭的你想要测试此 VSIX 扩展。 Visual Studio 扫描的新扩展在启动期间，因此如果 IDE 打开安装 VSIX 时你将需要重新启动 Visual Studio。 在资源管理器中，双击该 VSIX 文件以启动**VSIX Installer**，单击**安装**，然后启动 Visual Studio。

![VSIX 安装程序](custom-mvc-templates/_static/image9.jpg)

从菜单中，选择**工具 > 扩展和更新**若要确认你的扩展已安装。 如果 VSIX 安装扩展的安装过程中报告任何错误，则可以查看 VSIX 安装程序日志以了解更多信息。 通常在创建日志**%temp%**文件夹的用户安装该扩展，例如**C:\Users\Bob\AppData\Local\Temp**。

![扩展和更新](custom-mvc-templates/_static/image10.jpg)

关闭窗口后可以创建 MVC 4 项目以查看是否在 MVC 向导中显示新模板。

![新的 ASP.NET MVC 4 项目](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>限制

1. MVC 向导不支持本地化的自定义模板。
2. 未能查找自定义模板，该向导将不会报告任何错误。 如果任何所需的自定义属性都不存在，将只需从向导中排除该模板。
