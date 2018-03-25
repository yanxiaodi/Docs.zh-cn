---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: "配置和检测 |Microsoft 文档"
author: microsoft
description: "有配置中的重大更改和 ASP.NET 2.0 中的检测。 使用新的 ASP.NET 配置 API，配置更改进行 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2018
---
<a name="configuration-and-instrumentation"></a>配置和检测
====================
by [Microsoft](https://github.com/microsoft)

> 有配置中的重大更改和 ASP.NET 2.0 中的检测。 新的 ASP.NET 配置 API 允许以编程方式进行配置更改。 此外，许多新的配置设置存在允许对新的配置和检测。


有配置中的重大更改和 ASP.NET 2.0 中的检测。 新的 ASP.NET 配置 API 允许以编程方式进行配置更改。 此外，许多新的配置设置存在允许对新的配置和检测。

在此模块中，我们将讨论 ASP.NET 配置 API，它与读取和写入 ASP.NET 配置文件，以及我们还将介绍 ASP.NET 检测。 我们将介绍在 ASP.NET 跟踪中的新功能。

## <a name="aspnet-configuration-api"></a>ASP.NET 配置 API

ASP.NET 配置 API 允许你开发、 部署和管理通过使用一个编程接口的应用程序配置数据。 配置 API 可用于开发和以编程方式修改完成 ASP.NET 配置，而无需直接编辑配置文件中的 XML。 此外，你可以使用配置 API 在控制台应用程序和开发的脚本、 基于 Web 的管理工具以及 Microsoft 管理控制台 (MMC) 管理单元。

以下两个配置管理工具使用的配置 API，并且会随附.NET Framework 2.0 版：

- ASP.NET MMC 管理单元中，使用配置 API 以简化管理任务，提供本地配置数据从配置层次结构的所有级别的集成的视图。
- 网站管理工具，将允许你管理本地和远程应用程序的配置设置，包括托管的站点。

ASP.NET 配置 API 包含一组可用于以编程方式配置网站和应用程序的 ASP.NET 管理对象。 管理对象是为.NET Framework 类库实现的。 配置 API 编程模型有助于确保通过在编译时强制数据类型的代码的一致性和可靠性。 若要更加轻松地管理应用程序配置，配置 API，你可以查看作为单个集合，而不是将这些数据视为通过不同的单独集合情况下从配置层次结构中的所有点合并的数据配置文件。 此外，配置 API 使你能够无需直接编辑配置文件中的 XML 处理整个应用程序配置。 最后，该 API 简化了通过支持管理工具，如网站管理工具的配置任务。 配置 API 简化了通过支持的配置文件的计算机上创建和运行配置脚本在多台计算机的部署。

> [!NOTE]
> 配置 API 不支持 IIS 应用程序的创建。


## <a name="working-with-local-and-remote-configuration-settings"></a>使用本地和远程配置设置

配置对象表示应用于特定的物理实体，如一台计算机，或逻辑实体，如应用程序或网站的配置设置的合并的视图。 在本地计算机或远程服务器上，可以存在指定的逻辑实体。 当配置文件不存在为指定的实体时，配置对象表示由 Machine.config 文件定义的默认配置设置。

你可以通过使用以下类中的打开配置方法之一来获取配置对象：

1. ConfigurationManager 类中，如果你的实体是一个客户端应用程序。
2. WebConfigurationManager 类中，如果你的实体是一个 Web 应用程序。

这些方法将返回配置对象，它又提供所需的方法和属性，以处理基础的配置文件。 你可以访问这些文件进行读取或写入。

### <a name="reading"></a>阅读

GetSection 或 GetSectionGroup 方法用于读取配置信息。 用户或读取的进程必须具有读取所有层次结构中的配置文件上的权限。

> [!NOTE]
> 如果你使用一个带 path 参数的静态 GetSection 方法，path 参数必须引用在其中运行代码的应用程序。 否则为将忽略此参数并返回当前正在运行的应用程序的配置信息。


### <a name="writing"></a>写入

你可以使用一种保存方法以编写配置信息。 用户或进程的写入操作必须具有写入权限的配置文件和目录在当前配置层次结构级别，以及读取所有层次结构中的配置文件上的权限。

若要生成表示指定的实体的继承的配置设置的配置文件，请使用保存配置的以下方法之一：

1. Save 方法来创建新的配置文件。
2. 要生成新的配置文件在另一个位置的另存为方法。

## <a name="configuration-classes-and-namespaces"></a>配置类和命名空间

很多配置类和方法都类似于彼此。 下表描述的最常用的配置类和命名空间。

| **配置类或命名空间** | **说明** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) namespace | 包含所有.NET Framework 应用程序的主要配置类。 部分处理程序类用于从方法，如 GetSection 和 GetSectionGroup 获取节的配置数据。 这两种方法是非静态。 |
| System.Configuration.Configuration 类 | 表示一组对计算机、 应用程序、 Web 目录或其他资源的配置数据。 此类包含有用的方法，如 GetSection 和 GetSectionGroup，用于更新配置设置和获取对节和节组的引用。 此类用作获取设计时配置数据，如 WebConfigurationManager 和 ConfigurationManager 类方法的方法的返回类型。 |
| System.Web.Configuration namespace | ASP.NET 配置节定义在包含节处理程序类[ASP.NET 配置设置](https://msdn.microsoft.com/library/b5ysx397.aspx)。 部分处理程序类用于从方法，如 GetSection 和 GetSectionGroup 获取节的配置数据。 |
| System.Web.Configuration.WebConfigurationManager class | 提供用于获取指向运行时和设计时的配置设置的引用的有用方法。 这些方法将该 System.Configuration.Configuration 类用作返回类型。 你可以交替使用此类的静态 GetSection 方法或 System.Configuration.ConfigurationManager 类的非静态 GetSection 方法。 对于 Web 应用程序配置，而不是 System.Configuration.ConfigurationManager 类建议 System.Web.Configuration.WebConfigurationManager 类。 |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) namespace | 使您能够自定义和扩展的配置提供程序。 这是配置系统中的所有提供程序类的基类。 |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | 包含类和接口用于管理和监视的 Web 应用程序的运行状况。 严格地说，此命名空间不被视为配置 API 的一部分。 例如，跟踪和事件触发做出是通过此命名空间中的类实现。 |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx)命名空间 | 提供所需的检测应用程序公开其管理信息和潜在的使用者通过 Windows Management Instrumentation (WMI) 事件的类。 ASP.NET 运行状况监视使用 WMI 来提供事件。 严格地说，此命名空间不被视为配置 API 的一部分。 |

## <a name="reading-from-aspnet-configuration-files"></a>从 ASP.NET 配置文件读取

WebConfigurationManager 类是从 ASP.NET 配置文件进行读取的核心类。 有三个步骤实质上是读取 ASP.NET 配置文件：

1. 获取配置对象使用 OpenWebConfiguration 方法。
2. 获取对所需的部分配置文件中的引用。
3. 从配置文件中读取所需的信息。

对象表示的配置不表示特定配置文件。 相反，它表示计算机、 应用程序或网站的配置的合并的视图。 下面的代码示例实例化一个表示调用一个 Web 应用程序的配置的配置对象*ProductInfo*。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> 请注意，是否 /ProductInfo 路径不存在，上面的代码将在 machine.config 文件中返回指定的默认配置。


配置对象之后，可以使用 GetSection 或 GetSectionGroup 方法以向下钻取的配置设置。 下面的示例获取了对上述 ProductInfo 应用程序的模拟设置的引用：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>写入 ASP.NET 配置文件

如下所示从配置文件中读取，WebConfigurationManager 类是用于写入到 Asp.NET 配置文件的核心。 也有三个步骤向 ASP.NET 配置文件写入。

1. 获取配置对象使用 OpenWebConfiguration 方法。
2. 获取对所需的部分配置文件中的引用。
3. 从使用保存或另存为配置文件将写入所需的信息方法。

下面的代码更改**调试**属性&lt;编译&gt;为 false 的元素：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

此代码执行时，**调试**属性&lt;编译&gt;元素将设置为 false *webApp*应用程序的 web.config 文件。

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

System.Web.Management 命名空间提供的类和接口用于管理和监视 ASP.NET 应用程序的运行状况。

日志记录通过定义将事件与提供程序相关联的规则。 规则定义发送到提供程序的事件的类型。 下面的基本事件是可供你登录：

| **WebBaseEvent** | 基事件类的所有事件。 包含所需属性的所有事件，如事件代码、 事件详细信息代码、 日期和时间引发该事件时，序列号、 事件消息和事件的详细信息。 |
| --- | --- |
| **WebManagementEvent** | 基事件类，对于管理事件，如应用程序生命周期、 请求、 错误和审核事件。 |
| **WebHeartbeatEvent** | 生成的固定时间间隔中的应用程序，以捕获有用的运行时状态信息事件。 |
| **WebAuditEvent** | 安全审核事件，用于将标记条件授权失败，解密失败，如类的基类*等。* |
| **WebRequestEvent** | 所有信息性请求事件的基类。 |
| **WebBaseErrorEvent** | 类的基类，该值指示错误条件的所有事件。 |

可用提供程序的类型，可以将事件输出发送到事件查看器、 SQL Server、 Windows Management Instrumentation (WMI) 和电子邮件。 预配置的提供程序和事件映射减少记录的事件输出所需的工作量。

ASP.NET 2.0 使用事件日志提供程序的的现成记录事件基于应用程序域启动和停止，以及日志记录任何未经处理的异常。 这可帮助介绍一些基本方案。 例如，假设你的应用程序引发异常，但用户不会保存错误，你不能再现它。 使用默认的事件日志规则，你将能够收集要更好地了解哪种错误发生的异常和堆栈的信息。 如果你的应用程序丢失会话状态，另一个示例将应用。 在这种情况下，你可以查看事件日志以确定是否回收应用程序域，以及应用程序域首先停止的原因。

此外，监视系统的运行状况是可扩展的。 例如，你可以定义自定义 Web 事件、 应用程序中激发它们，然后定义一个规则以将事件信息发送到提供程序，例如你的电子邮件。 这使您轻松地将你检测到监视提供程序的运行状况。 再举一例，无法触发事件每次处理订单并将其设置将每个事件发送到 SQL Server 数据库的规则。 也可以激发事件时用户无法登录多次在行中，并将事件设置为使用基于电子邮件的提供程序。

默认的提供程序和事件的配置存储在全局的 Web.config 文件中。 全局的 Web.config 文件将存储所有的基于 Web 的设置的字符串存储在 Machine.config 文件在 ASP.NET 中 1 x。 全局的 Web.config 文件位于以下目录：

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt;全局的 Web.config 文件的一部分提供了默认配置设置。 你可以重写这些设置或配置你自己的设置方法是实现&lt;healthMonitoring&gt;你的应用程序的 Web.config 文件中的部分。

&lt;HealthMonitoring&gt;部分的全局的 Web.config 文件包含以下各项：

| **提供程序** | 包含提供程序的事件查看器、 WMI、 和 SQL Server 设置。 |
| --- | --- |
| **eventMappings** | 包含各种 WebBase 类的映射。 如果你生成自己事件的类，你可以扩展此列表。 生成您自己的事件类使能够你更细的粒度对发送到的信息的提供程序。 例如，你可以配置未经处理的异常时将自己的自定义事件发送到电子邮件发送到 SQL Server。 |
| **规则** | 链接到提供程序 eventMappings。 |
| **buffering** | 与 SQL Server 和电子邮件提供程序使用，来确定如何通常刷新到提供程序的事件。 |

下面是全局的 Web.config 文件中的一个代码示例。

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>如何将存储到事件查看器的事件

如前所述，来记录事件提供程序在事件查看器将为您配置在全局的 Web.config 文件中。 默认情况下，所有事件都基于**WebBaseErrorEvent**和**WebFailureAuditEvent**记录。 你可以添加其他规则，以记录到事件日志中的其他信息。 例如，如果你想要记录所有事件 (*即*，每个事件基于**WebBaseEvent**)，无法添加到 Web.config 文件的以下规则：

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

此规则都可以链接**所有事件**事件映射到事件日志提供程序。 EventMapping 和提供程序包含在全局的 Web.config 文件中。

## <a name="how-to-store-events-to-sql-server"></a>如何将存储到 SQL Server 事件

此方法使用**ASPNETDB**数据库，生成的 Aspnet\_regsql.exe 工具。 默认的提供程序使用 LocalSqlServer 连接字符串，它在应用程序中使用这两个基于文件的数据库\_数据文件夹或 SQL Server 的本地 SQLExpress 实例。 LocalSqlServer 连接字符串和 SqlProvider 中全局的 Web.config 文件进行配置。

全局的 Web.config 文件中的 LocalSqlServer 连接字符串如下所示：

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

如果你想要使用另一个 SQL Server 实例，你将需要使用 Aspnet\_regsql.exe 工具，可在 %windir%\microsoft.net\framework\v2.0。\*文件夹。 使用 Aspnet\_regsql.exe 工具生成一个自定义**ASPNETDB**数据库的 SQL Server 实例上，然后将连接字符串添加到你的应用程序配置文件，然后使用新的添加提供程序连接字符串。 一旦**ASPNETDB**创建的数据库，你将需要设置规则，将 eventMapping 链接到 sqlProvider。

是否使用默认 SqlProvider 或配置自己的提供程序时，你将需要添加链接具有事件映射的提供程序的规则。 以下规则链接到上面创建的新提供程序**所有事件**事件映射。 此规则将日志基于的所有事件**WebBaseEvent**并将它们发送给将使用 MYASPNETDB 连接字符串 MySqlWebEventProvider。 下面的代码添加规则以将具有事件映射的提供程序的链接：

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

如果你想要仅将错误发送给 SQL Server，则可以添加以下规则：

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>如何将事件转发到 WMI

你还可以将事件转发到 WMI。 WMI 提供程序为你在全局的 Web.config 文件中配置，默认情况下。

下面的代码示例将添加一个规则以将事件转发到 WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

你将需要添加规则以将提供程序，而且还需要 WMI 侦听器应用程序侦听事件 eventMapping 相关联。 下面的代码示例将添加一个规则以链接到 WMI 提供程序**所有事件**事件映射：

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>如何将事件转发到电子邮件

你还可以转发电子邮件事件。 注意有关你可以将其映射到你的电子邮件提供商，你可以无意中向自己发送大量的信息，如哪些事件规则可能是更好地适用于 SQL Server 或事件日志。 有两个电子邮件提供商;SimpleMailWebEventProvider 和 TemplatedMailWebEventProvider。 每个具有相同的配置属性，除了的"模板"和"detailedTemplateErrors"的属性，这两种才 TemplatedMailWebEventProvider 上可用。

> [!NOTE]
> 这些电子邮件提供商任一是为你配置的。 你将需要将它们添加到你的 Web.config 文件。


这些两个电子邮件提供商之间的主要区别是 SimpleMailWebEventProvider 泛型模板不能修改中发送电子邮件。 示例 Web.config 文件使用以下规则将此电子邮件提供程序添加到配置提供程序的列表：

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

此外添加以下规则以将绑定到电子邮件提供商**所有事件**事件映射：

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 跟踪

有三个主要增强功能，与 ASP.NET 2.0 中的跟踪。

1. 集成的跟踪功能
2. 以编程方式访问跟踪消息
3. 改进了应用程序级跟踪

## <a name="integrated-tracing-functionality"></a>集成跟踪功能

现在可以将消息发出到 ASP.NET 跟踪输出，System.Diagnostics.Trace 类的路由和路由到 System.Diagnostics.Trace ASP.NET 跟踪发出的消息。 你还可以将 ASP.NET 检测事件转发到 System.Diagnostics.Trace。 此功能提供新的**writeToDiagnosticsTrace**属性&lt;跟踪&gt;元素。 当此布尔值为 true 时，ASP.NET 跟踪消息将转发到使用 System.Diagnostics 跟踪基础结构，通过注册来显示跟踪消息的任何侦听器。

## <a name="programmatic-access-to-trace-messages"></a>编程访问以跟踪消息

ASP.NET 2.0 允许以编程方式访问通过的所有跟踪消息**TraceContextRecord**类和**TraceRecords**集合。 访问跟踪消息的最有效的方式是注册**TraceContextEventHandler** （ASP.NET 2.0 中还新） 的委托来处理新**TraceFinished**事件。 根据需要，然后可以循环访问的跟踪消息。

下面的代码示例阐释了这一点：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

在上面的示例中，我 TraceRecords 集合中循环，然后将每条消息写入到响应流。

## <a name="improved-application-level-tracing"></a>改进了应用程序级跟踪

通过引入新改进应用程序级跟踪**mostRecent**属性&lt;跟踪&gt;元素。 此属性指定是否显示最新的应用程序级跟踪输出，由 requestLimit 限制以外的较旧的跟踪数据将被丢弃。 如果为 false，跟踪数据将显示的请求，直到达到 requestLimit 属性。

## <a name="aspnet-command-line-tools"></a>ASP.NET 命令行工具

有几个命令行工具，可帮助中的 ASP.NET 的配置。 ASP.NET 开发人员应熟悉 aspnet\_regiis.exe 工具。 ASP.NET 2.0 提供了三个其他命令行工具，可帮助在配置中。

以下命令行工具有：

| **工具** | **使用** |
| --- | --- |
| **aspnet\_regiis.exe** | 允许注册 iis ASP.NET。 有此工具的两个版本所附带 ASP.NET 2.0，32 位系统 （在框架文件夹中） 和 64 位系统 （在 Framework64 文件夹中。）未将在 32 位操作系统上安装 64 位版本。 |
| **aspnet\_regsql.exe** | ASP.NET SQL 服务器注册工具用于在 ASP.NET 中，创建由 SQL Server 提供程序使用 Microsoft SQL Server 数据库，或用于添加或删除从现有数据库的选项。 Aspnet\_regsql.exe 文件位于 [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber Web 服务器上的文件夹。 |
| **aspnet\_regbrowsers.exe** | ASP.NET 浏览器注册工具分析和编译成一个程序集的所有系统级浏览器定义并将该程序集安装到全局程序集缓存。 该工具使用浏览器定义文件 (。浏览器文件） 从.NET Framework 浏览器子目录。 该工具可以 %SystemRoot%\Microsoft.NET\Framework\version\ 目录中找到。 |
| **aspnet\_compiler.exe** | ASP.NET 编译工具，可编译的 ASP.NET Web 应用，就地或部署到目标位置，如生产服务器。 就地编译有助于提高应用程序性能，因为最终用户不会遇到的延迟到应用程序的第一个请求时应用程序编译。 |

因为 aspnet\_regiis.exe 工具不熟悉 ASP.NET 2.0，此处我们将不讨论。

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL 服务器注册工具 aspnet\_regsql.exe

你可以设置多个类型的使用 ASP.NET SQL 服务器注册工具的选项。 可以指定 SQL 连接、 指定的 ASP.NET 应用程序服务使用 SQL Server 来管理信息、 指示哪些数据库或表适用于 SQL 缓存依赖项，并添加或删除有关使用 SQL Server 存储过程和会话状态支持。

多个 ASP.NET 应用程序服务依赖于提供程序来管理存储和从数据源检索数据。 每个提供程序是特定于数据源。 ASP.NET 包括的 SQL Server 提供程序在以下 ASP.NET 功能：

- 成员资格 ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)类)。
- 角色管理 ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)类)。
- 配置文件 ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)类)。
- Web 部件个性化设置 ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)类)。
- Web 事件 ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)类)。

在安装 ASP.NET 时，你的服务器的 Machine.config 文件包括指定每个依赖于提供程序的 ASP.NET 功能的 SQL Server 提供程序的配置元素。 这些提供程序配置，默认情况下，若要连接到 SQL Server Express 2005 的本地用户实例。 如果更改使用的提供程序的默认连接字符串，则你可以使用任何在计算机配置中，配置的 ASP.NET 功能之前必须安装 SQL Server 数据库和数据库元素针对使用 Aspnet你选功能\_regsql.exe。 如果你使用 SQL 注册工具指定的数据库不存在 （aspnetdb 将默认数据库如果未指定命令行上），则当前用户必须具有在 SQL Server 以及采用创建架构 e 中创建数据库的权限在数据库中的元素。

### <a name="sql-cache-dependency"></a>SQL 缓存依赖项

一项高级的功能的 ASP.NET 输出缓存是 SQL 缓存依赖项。 SQL 缓存依赖项支持两个不同的操作模式： 一个使用 ASP.NET 实现的表轮询间隔和使用 SQL Server 2005 的查询通知功能的第二个模式。 SQL 注册工具可以用于配置的表轮询操作模式。

### <a name="session-state"></a>会话状态

默认情况下，在 ASP.NET 进程内的内存中存储会话状态的值和信息。 或者，你可以在 SQL Server 数据库中，其中它可以由多个 Web 服务器共享存储会话数据。 如果你使用 SQL 注册工具的会话状态为指定的数据库不存在，当前用户必须有权在 SQL Server 以及创建一个数据库中的架构元素中创建数据库。 如果数据库不存在，当前用户必须有权在现有的数据库中创建架构元素。

若要安装在 SQL Server 会话状态数据库，运行 Aspnet\_regsql.exe 工具并提供使用命令的以下信息：

- SQL Server 的名称实例，请使用**-S**选项。
- 具有运行 SQL Server 的计算机上创建数据库的权限的帐户登录凭据。 使用**-E**选项以使用当前登录的用户，或使用**-U**选项以指定用户 ID，并使用**-P**选项以指定密码。
- **-Ssadd**命令行选项用于添加会话状态数据库。

默认情况下，不能使用 Aspnet\_regsql.exe 工具在运行 SQL Server 2005 Express Edition 的计算机上安装的会话状态数据库。

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET 浏览器注册工具-aspnet\_regbrowsers.exe

在 ASP.NET 1.1 版中，Machine.config 文件包含名为一节&lt;browserCaps&gt;。 本部分包含一系列定义基于正则表达式的各种浏览器的配置的 XML 项。 Asp.net 版本 2.0 中，新。浏览器文件定义的特定浏览器使用 XML 项的参数。 您添加新的浏览器上添加一个新的信息。浏览器文件复制到位于你的系统上的 %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers 的文件夹。

由于应用程序没有读取.config 文件每次它需要浏览器信息时，你可以创建一个新。浏览器文件和运行的 Aspnet\_regbrowsers.exe 将所需的更改添加到程序集。 这允许服务器立即访问新的浏览器信息，因此不需要关闭任何应用程序以拾取信息。 应用程序可以访问通过当前 HttpRequest 的浏览器属性浏览器功能。

运行 aspnet 时，还提供以下选项\_regbrowser.exe:

| **选项** | **说明** |
| --- | --- |
| **-?** | 显示 Aspnet\_regbbrowsers.exe 命令窗口中的帮助文本。 |
| **-i** | 创建运行时浏览器功能程序集并将其安装在全局程序集缓存。 |
| **-u** | 从全局程序集缓存中卸载的运行时浏览器功能程序集。 |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET 编译工具-aspnet\_compiler.exe

ASP.NET 编译工具可以使用两种常规方式： 进行就地编译和部署，其中指定目标输出目录的编译。

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[编译位置中的应用程序](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET 编译工具可以编译位置中的应用程序，即，它模仿的应用程序，从而导致正则编译发出多个请求的行为。 预编译网站的用户不会通过编译上第一次请求的页面而导致的延迟。

当你进行预编译的位置中的站点时，应该注意下列事项：

- 站点将保留其文件和目录结构。
- 你必须使用的服务器上的站点的所有编程语言的编译器。
- 如果任何文件的编译将失败，整个站点编译将失败。

你可以重新编译应用程序就地后向其添加新的源文件。 此工具将编译仅将新的或更改的文件，除非你包括**-c**选项。

> [!NOTE]
> 编译的应用程序包含嵌套的应用程序不编译嵌套的应用程序。 必须单独编译嵌套的应用程序。


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[编译为部署的应用程序](https://msdn.microsoft.com/library/ms229863.aspx)

通过指定 targetDir 参数编译的应用程序部署 （编译到的目标位置）。 TargetDir 可以对 Web 应用程序的最终位置，也可以进一步部署编译的应用程序。 使用**-u**选项编译的方式，你可以无需重新编译它，编译的应用程序中的某些文件进行更改的应用程序。 Aspnet\_compiler.exe 进行静态和动态文件类型之间的差异，并创建生成的应用程序时以不同方式处理它们。

- 静态文件类型是指没有关联的编译器或生成提供程序，如文件的那些命名具有如.css、.gif、.htm、.html、.jpg、.js 的扩展，依此类推。 这些文件只需复制到目标位置，并且它们保留的目录结构中的相对位置。
- 动态文件类型是指那些具有关联的编译器或生成提供程序，包括具有特定于 ASP.NET 等.asax、.ascx、.ashx、.aspx、.browser、.master，文件扩展名的文件。 ASP.NET 编译工具从这些文件中生成程序集。 如果**-u**省略选项，该工具还会创建文件的文件扩展名。映射到其程序集的原始源文件的编译。 若要确保保留应用程序源的目录结构，该工具，请生成目标应用程序中的相应位置中的占位符文件。

必须使用**-u**选项以指示是否可以修改已编译应用程序的内容。 否则为后续的修改将被忽略，或导致运行时错误。

下表描述了如何将 ASP.NET 编译工具处理不同的文件类型时**-u**已包含选项。

| **文件类型** | **编译器操作** |
| --- | --- |
| .ascx、.aspx、.master | 这些文件将被拆分为标记和源的代码，其中包括代码隐藏文件和任何代码都括在&lt;脚本 runat ="server"&gt;元素。 源代码编译到程序集中，使用派生自哈希算法的名称和程序集放置在 Bin 目录中。 任何内联代码，即，代码括 **&lt; %** 和 **% &gt;** 方括号，附带标记，不进行编译。 与源文件同名的新文件进行创建以包含标记，并在相应的输出目录中放入。 |
| .ashx、.asmx | 这些文件不进行编译，移动到输出目录，只要是，不进行编译。 如果你想要具有编译的处理程序代码，将代码放入应用程序中的源代码文件\_代码目录。 |
| .cs、.vb、.jsl、.cpp （不包括前面列出的文件类型的代码隐藏文件） | 编译并在它们引用的程序集作为资源包括这些文件。 源文件不复制到输出目录中。 如果未引用代码文件，则它不会编译。 |
| 自定义的文件类型 | 这些文件不会编译。 这些文件复制到相应的输出目录。 |
| 源应用程序中的代码文件\_Code 子目录 | 这些文件进行编译到程序集中，并放入的 Bin 目录中。 |
| 在应用程序的.resx 和.resource 文件\_GlobalResources 子目录 | 这些文件进行编译到程序集中，并放入的 Bin 目录中。 任何应用程序\_GlobalResources 子目录下的主输出目录中，创建并将位于源目录没有.resx 或.resources 文件复制到输出目录。 |
| 在应用程序的.resx 和.resource 文件\_LocalResources 子目录 | 这些文件不编译并复制到相应的输出目录。 |
| 在应用程序的.skin 文件\_主题子目录 | .Skin 文件和静态主题文件不进行编译，复制到相应的输出目录。 |
| .browser Web.config 静态文件类型的 Bin 目录中已存在的程序集 | 这些文件复制到输出目录。 |

下表描述了如何将 ASP.NET 编译工具处理不同的文件类型时**-u**省略选项。

| **文件类型** | **编译器操作** |
| --- | --- |
| .aspx、.asmx、.ashx、.master | 这些文件将被拆分为标记和源的代码，其中包括代码隐藏文件和任何代码都括在&lt;脚本 runat ="server"&gt;元素。 源代码会编译成程序集，使用派生自哈希算法的名称。 生成的程序集放置在 Bin 目录中。 任何内联代码，即，代码括 **&lt; %** 和 **% &gt;** 方括号，附带标记，不进行编译。 编译器将创建新文件以包含与源文件同名的标记。 这些生成的文件放置在 Bin 目录中。 具有与源文件相同名称但扩展名为，编译器还会创建文件。包含的映射信息的编译。 。已编译的文件放置在源代码文件的原始位置所对应的输出目录。 |
| .ascx | 这些文件被拆分为标记和源代码。 源代码编译到程序集和放置在 Bin 目录中，使用派生自哈希算法的名称。 生成没有标记文件。 |
| .cs、.vb、.jsl、.cpp （不包括前面列出的文件类型的代码隐藏文件） | 从.ascx、.ashx 或.aspx 文件生成的程序集引用的源代码是编译为程序集并放置在 Bin 目录中。 不源复制文件。 |
| 自定义的文件类型 | 像动态文件一样，这些文件进行编译。 具体取决于其所基于的文件的类型，编译器可以在输出目录中放置映射文件。 |
| 在应用程序的文件\_Code 子目录 | 此子目录中的源代码文件进行编译到程序集中，并放入的 Bin 目录中。 |
| 在应用程序的文件\_GlobalResources 子目录 | 这些文件进行编译到程序集中，并放入的 Bin 目录中。 任何应用程序\_在主输出目录下创建 GlobalResources 子目录。 如果配置文件指定 appliesTo ="All"，.resx 和.resources 文件复制到输出目录。 因此不会复制如果它们由引用[BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)。 |
| 在应用程序的.resx 和.resource 文件\_LocalResources 子目录 | 这些文件进行编译到程序集中具有唯一名称，并放入的 Bin 目录中。 没有文件.resx 或.resource 文件将复制到输出目录。 |
| 在应用程序的.skin 文件\_主题子目录 | 主题进行编译到程序集中，并放入的 Bin 目录中。 存根 （stub） 文件进行为.skin 文件创建并放入相应的输出目录中。 （如.css) 的静态文件将复制到输出目录。 |
| .browser Web.config 静态文件类型的 Bin 目录中已存在的程序集 | 这些文件复制到输出目录。 |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[固定的程序集名称](https://msdn.microsoft.com/library/ms229863.aspx##)

某些情况下，如部署 Web 应用程序使用 MSI Windows 安装程序，需要使用一致的文件名称和内容，以及一致的目录结构，以确定程序集或更新的配置设置。 在这些情况下，你可以使用**-fixednames**选项以指定 ASP.NET 编译工具应编译的程序集的每个源文件而不是使用 where 多页被编译到程序集。 这可能会导致大量的程序集，因此如果您担心可伸缩性应小心使用此选项。

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[强名称编译](https://msdn.microsoft.com/library/ms229863.aspx##)

**-Aptca**， **-delaysign**， **-keycontainer**和**-keyfile**选项提供，以便你可以使用 Aspnet\_compiler.exe 创建强命名程序集而无需使用[强名称工具 (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx)单独。 这些选项，分别对应到**AllowPartiallyTrustedCallersAttribute**， **AssemblyDelaySignAttribute**， **AssemblyKeyNameAttribute**，和**AssemblyKeyFileAttribute**。

讨论这些属性是本课程的范围之外。

## <a name="labs"></a>实验室

每个以下的实验室基于以前实验室。 你将需要按顺序执行这些操作。

## <a name="lab-1-using-the-configuration-api"></a>实验室 1： 使用配置 API

1. 创建新的网站名*mod9lab*。
2. 将新的 Web 配置文件添加到站点。
3. 将以下代码添加到 web.config 文件：


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

这将确保您有权将更改保存到 web.config 文件。

1. 将新的标签控件添加到 Default.aspx 并将 ID 更改为**lblDebugStatus**。
2. 将新的按钮控件添加到 Default.aspx。
3. 按钮控件将 ID 更改为**btnToggleDebug**和到文本**切换调试状态**。
4. 打开 Default.aspx 的代码隐藏文件的代码视图并添加**使用**语句**System.Web.Configuration** ，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. 将两个私有变量添加到类，另一个页面\_Init 方法如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. 将以下代码添加到页\_负载：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. 保存并浏览 default.aspx。 请注意标签控件显示的当前的调试状态。
2. 双击按钮控件设计器中，将以下代码添加到按钮控件的单击事件：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. 保存并浏览 default.aspx，然后单击按钮。
2. 打开 web.config 文件之后每个按钮单击，然后观察,**调试**属性中&lt;编译&gt;部分。

## <a name="lab-2-logging-application-restarts"></a>实验室 2： 日志记录应用程序重新启动

在本实验中，你将创建将可用于切换日志记录的应用程序关闭、 启动和事件查看器中的重新编译的代码。

1. 将 DropDownList 添加到 default.aspx，并将 ID 更改为 ddlLogAppEvents。
2. 设置**有些**属性到 DropDownList **true**。
3. 将三个项添加到下拉列表的项集合。 请**文本**第一个项*Select Value*和值-1。 请**文本**和**值**的第二个项**True**和**文本**和**值**的第三个项**False**。
4. 将新的标签添加到 default.aspx。 将 ID 更改为**lblLogAppEvents**。
5. 打开 default.aspx 的代码隐藏视图并添加新的声明的变量的类型 HealthMonitoringSection，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. 将以下代码添加到页中的现有代码\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. 双击下拉列表，将以下代码添加到 SelectedIndexChanged 事件：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. 浏览 default.aspx。
2. 设置下拉列表中为**False**。
3. 清除事件查看器中的应用程序日志。
4. 单击按钮以更改应用程序的调试属性。
5. 刷新事件查看器中的应用程序日志。 

    1. 记录任何事件？
    2. 为什么或为什么？
6. 设置下拉列表中为**True。**
7. 单击该按钮以切换为应用程序的调试属性。
8. 刷新应用程序登录事件查看器。 

    1. 记录任何事件？
    2. 应用程序关闭的原因是什么？
9. 试验启用和禁用日志记录，并查看对 web.config 文件所做的更改。

## <a name="more-information"></a>详细信息：

ASP.NET 2.0 的提供程序模型允许你创建自己的提供程序不仅应用程序检测，但对于许多其他用途也例如成员身份、 配置文件，等等。有关编写自定义提供程序以将应用程序事件记录到文本文件的详细信息，请访问[此链接](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp)。
