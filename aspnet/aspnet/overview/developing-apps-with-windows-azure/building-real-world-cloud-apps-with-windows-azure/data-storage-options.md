---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: "数据存储选项 （使用 Azure 构建真实世界云应用） |Microsoft 文档"
author: MikeWasson
description: "构建真实世界云应用程序与 Azure 的电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 88f57244bfbfdf33df3bb265d8aa2c93689b2f24
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>数据存储选项 （使用 Azure 构建真实世界云应用）
====================
通过[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下载修复此错误项目](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下载电子书](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **构建真实世界云应用程序与 Azure**电子书基于由 Scott Guthrie 的演示。 它还说明了 13 模式和实践，从而帮助你为成功开发适用于云中的 web 应用。 有关电子书的信息，请参阅[第一章](introduction.md)。


大多数人习惯于关系数据库，并他们往往会忽略其他数据存储选项，当它们设计云应用程序时。 将会导致并非最佳的性能、 高支出或较差，因为[NoSQL](http://en.wikipedia.org/wiki/NoSQL) （非关系） 数据库可以处理某些任务比关系数据库更为高效。 如果客户寻求帮助解决关键数据存储问题，通常是因为它们具有其中 NoSQL 选项之一会起作用更好地关系数据库。 在这些情况下客户将做得更好关闭如果他们已在应用程序部署到生产环境之前实施 NoSQL 解决方案。

另一方面，这也是一个错误假定 NoSQL 数据库可以执行一切操作，良好，甚至还不够。 所有数据存储任务; 没有单个最佳数据管理选项不同的数据管理解决方案为不同任务进行了优化。 大多数真实世界云应用程序具有的多种数据存储要求，并且通常由提供服务最佳组合的多个数据存储解决方案。

本章旨在为你提供可用的数据存储选项的更广泛的意义上云应用程序，以及有关如何选择适合你方案的一些基本指导。 最好的选项供你了解并考虑其优点和缺点之后再开发应用程序。 更改在生产应用程序中的数据存储选项可能会极其困难，如同拥有平面处于飞行状态时更改 jet 引擎。

## <a name="data-storage-options-on-azure"></a>在 Azure 上的数据存储选项

云可以相对轻松，使用各种关系和 NoSQL 数据存储。 以下是一些可以在 Azure 中使用的数据存储平台。

![](data-storage-options/_static/image1.png)

下表显示了四种类型的 NoSQL 数据库：

- [键/值数据库](https://msdn.microsoft.com/library/dn313285.aspx#sec7)存储每个键值的单个序列化的对象。 它们适合存储大量数据，其中你想要针对给定的密钥值获取一个项，你不具有基于项的其他属性的查询。

    [Azure Blob 存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/)是函数类似文件存储在云中，使用对应文件夹和文件名称的密钥值的键/值数据库。 检索由其文件夹和文件名称，而不是在搜索文件内容中的值的文件。

    [Azure 表存储](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)也是一个键/值的数据库。 每个值称为*实体*（类似于一个行，由分区键和行键标识） 和包含多个*属性*(类似于列，但并非所有表中的实体都必须共享相同列）。 对与键不同的列的查询非常低效，应当避免。 例如，你可以使用存储有关单个用户的信息的一个分区来存储用户配置文件数据。 在单独的一个实体的属性或在同一个分区中的不同实体，你可以存储数据，例如用户名、 密码哈希、 出生日期等。 您不想要为所有用户提供的出生日期，给定范围查询，但无法之间配置文件表与另一个表执行联接查询。 表存储是更具伸缩性和成本高于关系数据库中，但它并不启用复杂的查询或联接。
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8)的值是在其中的键/值数据库*文档*。 "文档"此处未使用在 Word 或 Excel 的文档的意义上，但意味着命名的字段和值，其中的任何可能是子文档的集合。 例如，订单历史记录表中可能具有的订单文档，订单编号、 订单日期和客户字段;和客户字段可能具有名称和地址字段。 数据库将编码格式如 XML、 YAML、 JSON 或 BSON; 中的字段数据也可以使用纯文本。 设置文档键/值数据库之外的数据库的一个功能是能够在非键字段上查询和定义辅助索引，以使查询更高效。 此功能使得文档数据库更适合应用程序需要检索基于条件比文档键的值更复杂的数据。 例如，销售订单历史记录文档数据库中您可以查询如产品 ID、 客户 ID、 客户名称和等的各个字段上。 [MongoDB](http://www.mongodb.org/)是常用文档数据库。
- [列系列数据库](https://msdn.microsoft.com/library/dn313285.aspx#sec9)是键/值数据存储，使您能够在结构数据存储到的相关列称为列系列的集合。 例如，人口普查数据库可能有一组人员的姓名的列 （首先，中间上, 一次），一个组以启用此人的地址和一个组有关用户的配置文件信息 (DOB，性别，等等)。 数据库可以然后存储每个列系列单独分区中保留所有人与相同的密钥相关的数据时。 然后，你可以无需通读的所有名称和地址信息以及读取所有配置文件信息。 [Cassandra](http://cassandra.apache.org/)是受欢迎的列系列数据库。
- [图形数据库](https://msdn.microsoft.com/library/dn313285.aspx#sec10)将信息存储为对象和关系的集合。 图形数据库的目的是使应用程序有效地执行的查询，它们之间遍历的网络中的对象和关系。 例如，这些对象可能是在人力资源数据库中，员工，你可能想以便于查询如"查找所有雇员的姓名直接或间接工作 Scott。" [Neo4j](http://www.neo4j.org/)是一个常用的关系图的数据库。

与关系数据库相比，NoSQL 选项提供更大的可伸缩性和用于存储和分析非结构化数据的成本效益。 代价是，它们不提供的丰富 queryability 和可靠的数据的关系数据库的完整性功能。 NoSQL 将适用于涉及大量联接查询不需要的 IIS 日志数据。 NoSQL 不适合非常好银行事务，这需要绝对数据完整性的并且涉及在与其他帐户相关的数据的多个关系。

此外还有一个较新类别的数据库平台调用[NewSQL](http://en.wikipedia.org/wiki/NewSQL) NoSQL 数据库的可伸缩性结合使用 queryability 和关系数据库的事务完整性。 NewSQL 数据库专门用于分布式的存储和查询处理时，这通常很难实现"OldSQL"数据库中。 [NuoDB](http://www.nuodb.com/)举例说明的 NewSQL 数据库，可以在 Azure 上使用。

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop 和 MapReduce

可以在 NoSQL 数据库中存储的数据的高卷可能很难及时有效地分析。 为此，你可以使用这样的框架[Hadoop](http://hadoop.apache.org/)实现[MapReduce](http://en.wikipedia.org/wiki/MapReduce)功能。 实质上是什么 MapReduce 过程将如下所示：

- 需要通过选择外数据处理的数据的大小只存储的数据你实际上需要分析的限制。 例如，你想要知道你的出生年份的用户的构成，因此从你的用户配置文件数据存储选择仅出生年。
- 分解分成几个部分的数据并将它们发送给不同的计算机进行处理。 计算机 A 上计算的人士提供 1950年 1959年日期数，则计算机 B 不 1960年 1969，等等。此组的计算机称为*Hadoop 群集*。
- 将每个部分结果部分处理完成后到一起。 你现在有一个相对较短的每个出生年份的多少人员列表和计算百分比此总体列表中的任务是易于管理。

在 Azure 上， [HDInsight](https://azure.microsoft.com/services/hdinsight/)使您能够处理和分析，并获取新的见解，从使用 Hadoop 的强大的大数据。 例如，你可以使用它来分析 web 服务器日志：

- 启用到你的存储帐户的 web 服务器日志记录。 这将设置 Azure 日志写入 Blob 服务为你的应用程序对每个 HTTP 请求。 Blob 服务基本上是云文件存储，以及它与 hdinsight 配合使用可以很好地集成。 

    ![日志到 Blob 存储](data-storage-options/_static/image2.png)
- 当应用程序变流量时，web 服务器 IIS 日志要写入到 Blob 存储中。 

    ![Web 服务器日志](data-storage-options/_static/image3.png)
- 在门户中，单击**新建** - **Data Services** - **HDInsight** - **快速创建**，并指定 HDInsight 群集名称、 群集大小 （HDInsight 群集数据节点数），用户名称和 HDInsight 群集的密码。 

    ![HDInsight](data-storage-options/_static/image4.png)

现在，你可以设置 MapReduce 作业，以分析日志，并获取类似于问题的答案：

- 一天中什么时间我的应用程序获取的最高或最低的流量？
- 哪些国家/地区是来自我流量？
- 什么是我的流量来自的区域的平均邻居收入。 （没有公共的数据集，可通过 IP 地址，让你以邻居收入，并且你可以与 web 服务器日志中的 IP 地址匹配的。）
- 邻居收入如何关联到特定页或站点中的产品？

然后可以使用类似这样到目标广告基于客户将会对感兴趣的可能性越小或可能购买特定产品的问题的答案。

中所述[使一切自动化章](automate-everything.md)，你可以在门户中执行操作的大多数函数可以实现自动化，并包含设置和 HDInsight 分析作业执行。 典型的 HDInsight 脚本可能包含以下步骤：

- 设置 HDInsight 群集，并将其链接到 Blob 存储输入你存储帐户。
- 将 MapReduce 作业可执行文件 （.jar 或.exe 文件） 上载到 HDInsight 群集。
- 提交 MapReduce 存储到 Blob 存储的输出数据。
- 等待作业完成。
- 删除 HDInsight 群集。
- 访问 Blob 存储的输出。

通过运行所有这些脚本，最小化设置 HDInsight 群集，这样就会减少所需的成本的时间量。

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>平台即服务 (PaaS) 而不是基础结构即服务 (IaaS)

前面列出的数据存储选项包括平台作为-服务 (PaaS) 和基础结构作为-服务 (IaaS) 解决方案。 PaaS 意味着我们管理的硬件和软件基础结构，你只需使用服务。 SQL Database 是 Azure PaaS 功能。 对于数据库，提出并在幕后 Azure 将设置和配置 Vm 并将在其上的数据库设置。 不能直接访问 Vm，且没有对其进行管理。IaaS 表示设置、 配置和管理在我们的数据中心基础结构中运行的 Vm，你将置于想在其上包括的任何内容。 我们提供预配置的 VM 映像的库的常见 VM 配置。 例如，你可以针对 Windows Server 2008、 Windows Server 2012、 BizTalk Server、 Oracle WebLogic Server、 Oracle 数据库等安装预配置的 VM 映像。

Azure 提供的 PaaS 数据解决方案包括：

- Azure SQL 数据库 （以前称为 SQL Azure）。 基于 SQL Server 上的云关系数据库。
- Azure 表存储。 键/值 NoSQL 数据库。
- Azure Blob 存储。 文件存储在云中。

对于 IaaS，可以运行任何操作可以将加载到一个 VM，例如：

- 如 SQL Server、 Oracle、 MySQL、 SQL Compact、 SQLite 或 Postgres 的关系数据库。
- Memcached、 Redis、 Cassandra 和 Riak 之类的键/值数据存储区。
- 列数据存储，例如 HBase。
- 如 MongoDB、 RavenDB 和 CouchDB 的文档数据库。
- 如 Neo4j 图形数据库。

![在 Azure 上的数据存储选项](data-storage-options/_static/image5.png)

IaaS 选项为你提供几乎无限的数据存储选项，而且其中的许多是尤其是易于使用的因为您可以创建使用预配置的映像的 Vm。 例如，在管理门户，转到**虚拟机**，单击**映像**卡，然后单击**浏览 VM 仓库**。

![浏览 VM 仓库](data-storage-options/_static/image6.png)

然后，您看到一份[数百个预配置的 VM 映像](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)，并从已预装的数据库管理系统的映像，如 MongoDB、 Neo4J、 Redis、 Cassandra 或 CouchDB，可以创建 VM:

![在 VM Depot 的 MongoDB](data-storage-options/_static/image7.png)

Azure 使 IaaS 数据存储选项为易于使用，但 PaaS 产品具有使它们更经济高效且更适用于许多方案的很多优势：

- 无需创建 Vm，你只需使用门户或脚本来设置数据存储区。 如果你想 200 terabyte 数据存储区，你可以只需单击一个按钮或运行命令，并且以秒为单位可以供你使用。
- 无需管理或修补程序服务; 使用的 VmMicrosoft 会为你自动。-无需担心设置缩放或高可用性; 的基础结构Microsoft 为你处理所有这些。
- 无需购买许可证;许可费用都包括在服务费用。
- 你只为你的使用付费。

在 Azure 中的 PaaS 数据存储选项包括由第三方提供商的产品。 例如，你可以选择[MongoLab 外接程序](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/)从要设置 MongoDB 数据库作为一项服务的 Azure 存储区。

## <a name="choosing-a-data-storage-option"></a>选择一个数据存储选项

没有一种方法最适合所有方案。 如果任何人都指出此技术是个问题的回答，首先要询问是"是什么问题？"，因为针对不同的事物进行了优化的不同解决方案。 有很明确好处关系模型;这就是为什么它相继出现因此长时间。 但也有到可以使用 NoSQL 解决方案进行寻址的 SQL 的向下边。

通常我们看到的内容工作最佳是一种组合方法，您在其中使用 SQL 和 NoSQL 单个解决方案。 甚至当人员说它们要接纳 NoSQL，是否你钻取到自己在做什么你通常查找他们正在使用多个不同的 NoSQL 框架： 他们正在使用[CouchDB](http://wiki.apache.org/couchdb/Introduction)，和[Redis](http://redis.io/)，和[Riak](http://basho.com/riak/)进行不同的工作。 甚至 Facebook，广泛使用 NoSQL，服务的不同部分使用不同的 NoSQL 框架。 可以灵活地混合和匹配数据的存储方法是因为很容易地使用多个数据解决方案并将它们集成在单个应用程序中的有关云，很好的事情之一。

下面是一些问题，可考虑时选择一种方法：

| 数据语义 | -什么是核心数据存储和数据访问语义 （将您存储关系或非结构化数据）？ 非结构化的数据，例如媒体文件最适合在 blob 存储;集合相关的数据，如产品、 清单、 供应商、 客户订单、 等，最适合在关系数据库中。 |
| --- | --- |
| 查询支持 | -如何轻松是它来查询的数据？ -什么类型的问题可能会有效地要求？ 键/值数据存储区是很好地获取给定键的值的单个行，但不是因此适合复杂的查询。 用户配置文件数据存储中都能得到一个特定用户的数据，键/值数据存储可能工作良好;产品目录想要获得不同的分组基于各种产品属性关系数据库可能更好地工作。 NoSQL 数据库可以存储大量数据有效，但可以构建围绕如何应用查询的数据，数据库以及这会使即席查询很难执行。 使用关系数据库中，你可以构建几乎任何类型的查询。 |
| 功能投影 | -可以问题，聚合，等等，在执行的服务器端？ 如果我运行选择的计数 (\*) 从 SQL 中的表，它将非常高效地执行服务器上的所有工作，并返回数值我正在寻找。 如果我想同一计算来自不支持聚合的 NoSQL 数据存储，这是一个效率低下的"不受限制的查询"，并可能会超时。即使成功执行查询我必须从服务器中的所有数据检索到客户端和客户端上的行进行计数。 -可以使用何种语言或表达式的类型？ 与关系数据库中，我可以使用 SQL。 对于某些如 Azure 表存储的 NoSQL 数据库，我将使用[OData](http://www.odata.org/)，而我可以做的就是对主键进行筛选和获取 （选择可用的字段的子集） 的投影。 |
| 容易伸缩 | -如何通常和如何多将数据需要扩展？ -平台以本机方式实现横向扩展？ -如何轻松是它以添加/删除容量 （大小和吞吐量）？ 关系数据库和表不自动分区以使它们的可伸缩性，这样便难以扩展超过一定的限制。 Azure 表存储空间等 NoSQL 数据存储区本质上是分区的所有内容，并且没有添加分区几乎没有限制。 你随时可以通过以下方式扩展表存储最多 200 兆兆字节，但 Azure SQL 数据库的最大数据库大小 500 千兆字节。 你可以通过分区为多个数据库，来缩放关系数据，但设置应用程序以支持该模型涉及大量编程工作。 |
| 检测和可管理性 | -如何轻松是能够检测、 监视和管理平台？ 你将需要及时了解运行状况和性能数据存储区，因此你需要提前知道一个平台，可免费，哪些度量值和你需要开发自己。 |
| 操作 | -如何轻松是平台来部署和在 Azure 上运行？ PaaS？ IaaS？ Linux？ 表存储和 SQL 数据库可轻松地在 Azure 上设置。 不是内置的 Azure PaaS 解决方案的平台需要更多的工作。 |
| API 支持 | -是一个可以轻松地使用平台的 API 可用？ 对于 Azure 表服务没有使用.NET API 支持.NET 4.5 的异步编程模型的 SDK。 如果你编写的.NET 应用程序，它将是可以更轻松地编写和 Azure 表服务相比到另一个键/值列数据存储平台具有任何 API 或一个不太全面测试代码。 |
| 事务的完整性和数据一致性 | -是关键平台才能保证数据一致性支持事务？ 用于跟踪发送，性能和低的数据存储成本可能比自动支持事务或数据平台中的引用完整性更重要的批量电子邮件进行 Azure 表服务一个不错的选择。 用于跟踪银行帐户余额或采购订单提供强事务保证将更好的选择的关系数据库平台。 |
| 业务连续性 | -如何轻松是否备份、 还原和灾难恢复？ 将获取生产数据损坏更快或更高版本，你将需要撤消的功能。 关系数据库通常具有更多的细化还原功能，如能够及时还原到某个点。 了解哪些还原功能可用于你正在考虑每个平台中是要考虑的重要因素。 |
| 成本 | -如果多个平台可支持您数据的工作负荷，它们的权限相比如何成本？ 例如，如果你使用 ASP.NET 标识，你可以在 Azure 表服务或 Azure SQL 数据库中存储用户配置文件数据。 如果你不需要丰富的查询的 SQL 数据库功能，你可以选择 Azure 表部分因为它的成本很多更低的给定的存储。 |

我们通常的建议是知道中每个类别的问题的答案，然后选择你的数据存储解决方案。

此外，你的工作负荷可能存在某些平台可以比其他更好地支持的特定要求。 例如:

- 不应用程序需要审核功能？
- 你的数据使用寿命要求是什么-你需要存档或清除的自动的功能吗？
- 你是否需要专用的安全需求？ 例如，数据包括 PII （个人身份信息），但是你必须能够确保 PII 会从查询结果中排除。
- 如果你有一些数据，无法存储在云中出于法规或技术原因，你可能需要云数据存储平台，它方便了与你在本地存储集成。

## <a name="demo--using-sql-database-in-azure"></a>演示 – 在 Azure 中使用 SQL 数据库

修复该应用程序使用关系数据库来存储任务。 环境创建 Windows PowerShell 脚本中所示[使一切自动化章](automate-everything.md)创建两个 SQL 数据库实例。 您可以看到这些在门户中通过单击**SQL 数据库**选项卡。

![在门户中的 SQL 数据库](data-storage-options/_static/image8.png)

也很容易通过使用门户来创建数据库。

单击**新建-数据服务** -- **SQL 数据库** -- **快速创建**，输入数据库名称，选择你已在你的帐户中的服务器或者，创建一个新的活动，然后单击**创建 SQL 数据库**。

![新建 SQL 数据库](data-storage-options/_static/image9.png)

请等待几秒钟，并随时供你使用 Azure 中有一个数据库。

![创建的新 SQL 数据库](data-storage-options/_static/image10.png)

因此，Azure 未几个秒什么可能需要你一天或每周或更长时间才能在本地环境中完成。 因为你可以轻松地创建数据库自动在脚本中或通过使用管理 API，你可以动态向外扩展通过将数据分散到多个 < o:p > 数据库，来处理程序，但前提是你的应用程序具有已设计用来实现的和。 < /o: p >

这是我们的平台即服务模型的一个示例。 无需管理的服务器，我们执行此操作。 无需担心备份，我们执行此操作。 在高可用性 – 运行自动在三个服务器之间复制数据库中的数据。 如果一台计算机出现故障，我们自动故障转移，并会丢失任何数据。 服务器进行定期修补，不需要去担心这些事情。

单击一个按钮并获取确切的连接字符串需要可以立即开始使用新的数据库。

![连接字符串](data-storage-options/_static/image11.png)

仪表板显示你的连接历史记录和使用的存储量。

![SQL Database 仪表板](data-storage-options/_static/image12.png)

你可以管理门户中的数据库或通过使用 SQL Server 工具你已熟悉，包括 SQL Server Management Studio (SSMS) 和 SQL Server 对象资源管理器 (SSOX) 和服务器资源管理器的 Visual Studio 工具。

![SSOX](data-storage-options/_static/image13.png)

另一个好处是定价模型。 你可以使用免费的 20 MB 数据库，启动开发和生产数据库开始大约为每月 5 美元。 您支付仅你实际上存储在数据库中，不的最大容量的数据量。 你无需购买相应许可证。

SQL Database 是易于扩展。 为 Fix It 应用，我们在我们的自动化脚本中创建的数据库的上限为 1 千兆。 如果你想要将其增加到 150 千兆，你可以只需转到门户和更改该设置，或执行 REST API 命令，并且以秒为单位你 150 千兆数据库可部署到的数据。

![SQL 数据库版本和大小](data-storage-options/_static/image14.png)

这就是云以快速轻松地建立起基础结构和开始立即使用它。

修复它应用使用两个 SQL 数据库，一个用于成员资格 （身份验证和授权），一个用于数据，并且这是你所要做，以对它进行设置和扩展它。 你此前看到如何设置数据库通过 Windows PowerShell 脚本，现在你已了解如何在门户中是多么容易。

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>与直接数据库访问使用 ADO.NET 实体框架

修复它应用通过使用实体框架来访问这些数据库，但 Microsoft 的.NET 应用程序的建议 ORM （对象关系映射器）。 ORM 是一个很好的工具，便于开发人员工作效率，但工作效率的代价是在某些情况下的性能下降。 在实际云应用程序中你不会进行一个选择是使用 EF 还是直接使用 ADO.NET--你将同时使用。 大多数情况下当你在编写适用于数据库中，代码获取最高的性能并不重要，并且你可以充分利用简化的编码和测试你获取与实体框架。 在 EF 开销导致不可接受的性能的情况下，可以写入和执行您自己使用 ADO.NET，理想情况下通过调用存储的过程的查询。

你想要最小化"频率"尽可能多地使用以访问数据库时的任何方法。 换而言之，如果你可以设置而不是几十或数百个较小的一个更大的查询结果中获取所需的所有数据，这是通常更可取的方法。 例如，如果你需要列表学员和注册中的课程，则通常最好获取所有一个联接查询而不是在一个查询中获取学生和执行单独查询每个学生的课程中的数据。

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL 数据库和实体框架中修复它应用

在修复它应用`FixItContext`类，该类派生自实体框架`DbContext`类，请标识的数据库并指定数据库中的表。 上下文指定实体集 （表） 的任务，并且该代码将传入到上下文的连接字符串名称。 该名称是指在 Web.config 文件中定义的连接字符串。

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

中的连接字符串*Web.config*文件被命名为 appdb （此处指向本地开发数据库）：

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

实体框架创建*FixItTasks*表基于中包含的属性`FixItTask`实体类。 这是一个简单的 POCO （普通旧 CLR 对象） 类，这意味着它不继承自或对实体框架的任何依赖关系。 但是，实体框架知道如何创建基于它的表并执行与其 CRUD （创建-读取-更新-删除） 操作。

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks 表](data-storage-options/_static/image15.png)

修复它应用程序包括一个存储库接口，它使用的 CRUD 操作如何使用数据存储区。

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

请注意，存储库方法都是所有异步，因此所有数据访问，可以都进行完全异步的方式。

存储库实现调用实体框架异步方法以处理的数据，包括 LINQ 查询以及与插入、 更新和删除操作。 下面是代码的查找修复它任务示例。

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

你会注意到还有一些计时和错误日志记录代码，我们将更高版本中查看该[监视和遥测章节](monitoring-and-telemetry.md)。

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>选择与 Azure 中的 VM (IaaS) 中的 SQL Server 的 SQL 数据库 (PaaS)

有关 SQL Server 和 Azure SQL 数据库很棒是这两个的核心编程模型，并完全相同。 您可以在这两个环境中使用的相同的技能的大多数。 你甚至可以将开发中的 SQL Server 数据库和 SQL 数据库实例在云中，即修复它应用的设置方式。

作为替代方法，你可以运行相同的 SQL Server 在云中通过安装在 IaaS Vm 上运行在本地。 对于某些旧的应用程序，在 VM 中运行 SQL Server 可能是更好的解决方案。 在专用 VM 上运行的 SQL Server 数据库，因为它具有比共享服务器运行的 SQL 数据库数据库的更多可用资源。 这意味着 SQL Server 数据库可以更大，仍很好地运行。 一般情况下，越小的数据库大小和表大小，更好地用例适用于 SQL 数据库 (PaaS)。

以下是有关如何选择两个模型之间的一些准则。

| Azure SQL 数据库 (PaaS) | 虚拟机 (IaaS) 中的 SQL Server |
| --- | --- |
| **专业人员**-无需创建或管理 Vm、 更新或修补程序 OS 或 SQL;Azure 为你执行该操作。 -内置的高可用性，与数据库级别 SLA。 -低总拥有成本 (TCO)，因为你只需支付使用 （不需要许可证）。 -适用于处理大量的小型数据库 (&lt;= 500 GB)。 -轻松地动态创建新的数据库启用横向扩展。 | ***专业人员***-与本地 SQL Server 的功能兼容。 -可以实现 SQL Server[通过 AlwaysOn 高可用性](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx)2 + Vm，并确保 SLA 来 VM 级别中。 -你可以完全控制如何管理 SQL。 -可以重复使用你已拥有，或按某个小时付费的 SQL 许可证。 -适用于处理更少但更大 (1 TB +) 数据库。 |
| **Cons**的某些功能相比于在本地 SQL Server 的缺口 (缺少[CLR 集成](https://technet.microsoft.com/library/ms131102.aspx)， [TDE](https://technet.microsoft.com/library/bb934049.aspx)，[压缩支持](https://technet.microsoft.com/library/cc280449.aspx)， [SQLServer Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)等) 的数据库大小限制为 500 GB。 | ***Cons*** -更新/修补程序 （OS 和 SQL） 是您有责任-创建和管理的数据库对于您有责任的限制为大约 8000 （通过 16 个数据驱动器） 的磁盘 IOPS （每秒输入/输出操作）。 |

如果你想要在 VM 中使用 SQL Server，你可以使用你自己的 SQL Server 许可证，或可以为一个按小时付费。 例如，在门户中或通过 REST API 可以创建使用 SQL Server 映像的新 VM。

![创建使用 SQL Server VM](data-storage-options/_static/image16.png)

![SQL Server VM 映像的列表](data-storage-options/_static/image17.png)

在创建 VM 具有 SQL Server 映像，我们创建速率的 SQL Server 许可成本按小时基于你的 VM 的使用情况。 如果你有项目，这只会运行几个月，是按小时付费成本更低。 如果你认为你的项目转到最后一年，是经济购买许可证通常所做的方式。

## <a name="summary"></a>摘要

云计算变得切实可行来混合和匹配数据的存储方法，以便最好地满足你的应用程序的需求。 如果你正在生成新的应用程序，请仔细考虑有关才能选取将继续在你的应用程序的增长，十分适用的方法在此处列出的问题。 [下一章](data-partitioning-strategies.md)将介绍一些可用于组合多个数据存储方法的分区策略。

## <a name="resources"></a>资源

有关更多信息，请参见以下资源。

选择的数据库平台：

- [高度可缩放解决方案的数据访问： 使用 SQL、 NoSQL 和 Polyglot 持久性](http://aka.ms/dag-doc)。 电子书 Microsoft 模式与实践中的深度放入不同种类的数据存储可用于云应用程序。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/ff898430.aspx)。 请参阅数据一致性入门、 数据复制和同步指南，索引表模式、 具体化视图模式。
- [基本： Acid 备用](http://queue.acm.org/detail.cfm?id=1394128)。 有关数据一致性和可伸缩性之间权衡的文章。
- [七个星期中的七个数据库： 现代数据库和 NoSQL 移动的指南](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)。 由 Eric Redmond 和 Jim。 wilson 制作的书籍。 强烈建议为自己引入数据存储平台目前可用的范围。

SQL Server 和 SQL 数据库之间进行选择：

- [高级 SQL Database 预览版指导](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx)。 SQL 数据库高级版，以及有关何时选择而不是 SQL 数据库 Web 和企业版本的指导简介。
- [指导原则和限制 (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)。 链接到有关的 SQL 数据库限制的文档的门户页，其中一个重点介绍该 SQL 数据库的 SQL Server 功能不支持。
- [Azure 虚拟机中的 SQL Server](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx)。 有关在 Azure 中运行 SQL Server 的文档链接的门户页面。
- [Scott Guthrie 介绍 Azure 中的 SQL 数据库](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/)。 SQL 数据库由 Scott Guthrie 6 分钟视频介绍。
- [应用程序模式和 Azure 虚拟机中 SQL Server 的开发策略](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx)。

在 ASP.NET Web 应用程序中使用实体框架和 SQL 数据库

- [EF 6 使用 MVC 5 入门](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 包含 9 个部分组成的系列教程将指导你完成生成，该服务的 MVC 应用程序使用 EF，并将数据库部署到 Azure 和 SQL 数据库。
- [使用 Visual Studio 的 ASP.NET Web 部署](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)。 十二个一部分进入详细深入探讨了有关如何将数据库部署通过使用 EF Code First 的教程系列。
- [将包含成员资格、 OAuth 和 SQL 数据库的安全 ASP.NET MVC 5 应用程序部署到 Azure 网站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)。 分步教程将指导你完成创建 web 应用程序使用身份验证、 成员资格数据库中存储应用程序表、 修改数据库架构，和将应用程序部署到 Azure。
- [ASP.NET 数据访问内容映射](https://go.microsoft.com/fwlink/p/?LinkId=282414)。 用于使用 EF 和 SQL 数据库的资源的链接。

在 Azure 上使用 MongoDB:

- [MongoLab-在 Azure 上的 MongoDB](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/)。 有关在 Azure 上运行 MongoDB 的文档的门户页。
- [创建 Azure 网站连接到 Azure 中的虚拟机上运行的 MongoDB](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)。 分步教程演示如何在 ASP.NET web 应用程序中使用 MongoDB 数据库。

HDInsight (Hadoop 在 Azure 上):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)。 门户上的 HDInsight 文档[Azure](https://azure.microsoft.com/)网站。
- [Hadoop 和 HDInsight： 在 Azure 中的大数据](https://msdn.microsoft.com/magazine/dn385705.aspx)。 MSDN 杂志文章 Bruno Terkaly 和 Ricardo Villalobos，引入 Azure 上的 Hadoop。
- [Microsoft 模式和实践-Azure 指南](https://msdn.microsoft.com/library/dn568099.aspx)。 请参阅 MapReduce 模式。

>[!div class="step-by-step"]
[上一页](single-sign-on.md)
[下一页](data-partitioning-strategies.md)
