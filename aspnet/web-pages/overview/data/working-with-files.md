---
uid: web-pages/overview/data/working-with-files
title: "使用 ASP.NET Web 页 (Razor) 站点中的文件 |Microsoft 文档"
author: tfitzmac
description: "本章介绍如何读取、 写入、 追加、 删除和上载文件。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web 页 (Razor) 站点中的文件
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 此文章介绍了如何读取、 写入、 追加、 删除和将 ASP.NET Web 页 (Razor) 站点中的文件上载。
> 
> > [!NOTE]
> > 如果你想要将图像上载并对其进行处理 （例如，翻转或调整其大小），请参阅[处理 ASP.NET Web Pages 站点中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。
> 
> 
> **你将学习：** 
> 
> - 如何创建一个文本文件，并向其中写入数据。
> - 如何将数据追加到现有文件。
> - 如何读取文件并从它将显示。
> - 如何从网站中删除文件。
> - 如何让用户上载一个文件或多个文件。
> 
> 这些是 ASP.NET 编程文章中引入的功能：
> 
> - `File`对象，它提供了如何管理文件。
> - `FileUpload`帮助器。
> - `Path`对象，它提供可用于操作路径和文件名的方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教程还适用于 WebMatrix 3。


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>创建文本文件并向其中写入数据

除了你的网站中使用的数据库，你可能会处理文件。 例如，你可以将文本文件用作一种简单的方式来存储站点数据。 (用于存储数据的文本文件有时称为*平面文件*。)文本文件可以采用不同格式，如*.txt*， *.xml*，或*.csv* （以逗号分隔值）。

如果你想要将数据存储在文本文件中，你可以使用`File.WriteAllText`方法来指定要创建的文件并对其进行写入的数据。 在此过程中，你将创建页面，其中包含有三个简单的表单`input`元素 （名字、 姓氏和电子邮件地址） 和一个**提交**按钮。 当用户提交表单时，你将在文本文件中存储用户的输入。

1. 创建一个名为的新文件夹*应用\_数据*，如果它已不存在。
2. 在你的网站的根目录，创建一个名为的新文件*UserData.cshtml*。
3. 将现有内容替换为以下： 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    HTML 标记创建具有三个文本框的窗体。 在代码中，你使用`IsPost`属性来确定开始处理之前是否已提交页面。

    第一个任务是获取用户输入并将其分配给变量。 然后，代码会将单独的变量的值串联成一个逗号分隔的字符串，然后存储在一个不同的变量。 请注意，逗号分隔符是包含用引号引起来的字符串 （"，"），因为要按其原义嵌入到你要创建的大字符串的逗号。 在你将连接在一起的数据结束时，你将添加`Environment.NewLine`。 这将添加一个分行符 （换行符）。 你要创建的用于此串联是一个字符串，如下所示：

    [!code-css[Main](working-with-files/samples/sample2.css)]

    （通过结束时不可见的行中断。）

    然后创建一个变量 (`dataFile`)，其中包含的位置和要存储中的数据的文件的名称。 设置位置需要某些特殊处理。 在网站中，它是不好的做法，在代码中引用为绝对路径，如*C:\Folder\File.txt* web 服务器上的文件。 如果移动网站，绝对路径将会不正确。 此外，为托管站点 （如而非你自己的计算机上） 您通常不知道正确的路径是在你编写代码时。

    但有时 （例如 now，进行写入的文件)，你需要的完整路径。 解决方案是使用`MapPath`方法`Server`对象。 这将返回到你的网站的完整路径。 若要获取的网站根目录中，你的用户的路径`~`运算符 (站点来表示您的虚拟根) 到`MapPath`。 (你还可以传递子文件夹名称给它，如*~/App\_数据 /*，以获取该子文件夹的路径。)然后可以连接到任何该方法返回若要创建的完整路径的其他信息。 在此示例中，你可以添加文件名称。 (你可以阅读更多有关如何使用中的文件和文件夹路径[ASP.NET 网页编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    该文件保存在*应用\_数据*文件夹。 此文件夹是用于存储数据文件中所述的 ASP.NET 中的特殊文件夹[使用 ASP.NET Web Pages 站点中的数据库的简介](https://go.microsoft.com/fwlink/?LinkId=195209)。

    `WriteAllText`方法`File`对象将数据写入该文件。 此方法采用两个参数： 写入的文件及要写入的实际数据 （包括路径） 的名称。 请注意，第一个参数的名称具有`@`字符作为前缀。 这是告诉的 ASP.NET 你提供的文本，原义字符串且字符，例如"/"不应以特殊方式解释。 (有关详细信息，请参阅[ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    > [!NOTE]
    > 为了使你的代码以将文件保存于*应用\_数据*文件夹中，应用程序需要读写权限为该文件夹。 你的开发计算机上这是不通常问题。 但是，当你的站点发布到宿主提供程序的 web 服务器时，你可能需要显式设置这些权限。 如果你在托管提供商的服务器上运行此代码，并获取错误，请与宿主提供程序中，若要了解如何设置这些权限。

- 在浏览器中运行页面。 

    ![](working-with-files/_static/image1.jpg)
- 到的字段中输入值，然后单击**提交**。
- 关闭浏览器。
- 返回到该项目并刷新视图。
- 打开*data.txt*文件。 在窗体中提交的数据位于文件中。 

    ![[image]](working-with-files/_static/image2.jpg)
- 关闭*data.txt*文件。

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>将数据追加到现有文件

在前面的示例中，你使用`WriteAllText`创建一个文本文件，在其中有一条数据。 如果你再次调用该方法，并将其传递的相同的文件名称，完全覆盖现有文件。 但是，创建一个文件后你通常想要将新数据添加到文件末尾。 你可以执行，使用`AppendAllText`方法`File`对象。

1. 在网站中，请一份*UserData.cshtml*文件并将副本命名*UserDataMultiple.cshtml*。
2. 将代码块在打开之前`<!DOCTYPE html>`标记中使用下面的代码块： 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    此代码从前面的示例中有一项更改。 而不是使用`WriteAllText`，它使用`the AppendAllText`方法。 方法非常相似，只不过`AppendAllText`将数据添加到文件末尾。 与`WriteAllText`，`AppendAllText`创建文件，如果它尚不存在。
3. 在浏览器中运行页面。
4. 用于字段输入值，然后单击**提交**。
5. 添加更多的数据，并再次提交窗体。
6. 返回到你的项目，右键单击项目文件夹，然后单击**刷新**。
7. 打开*data.txt*文件。 它现在包含您刚才输入的新数据。 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>读取和显示文件中的数据

即使你不需要将数据写入到文本文件，有时可能需要从一个读取数据。 若要执行此操作，可以再次使用`File`对象。 你可以使用`File`对象来单独读取每个行 （换行分隔） 或读取无论分隔方式的单个项。

此过程演示了如何读取，并显示你在前面的示例中创建的数据。

1. 在你的网站的根目录，创建一个名为的新文件*DisplayData.cshtml*。
2. 将现有内容替换为以下： 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    通过读取上一示例中创建到一个名为变量的文件来启动代码`userData`，使用此方法调用：

    [!code-css[Main](working-with-files/samples/sample5.css)]

    要执行此操作的代码位于`if`语句。 如果你想要读取的文件，它是一个好办法使用`File.Exists`方法，以首先确定文件是否可用。 代码还将检查文件是否为空。

    页面的正文包含两个`foreach`循环，嵌套在另一个。 外部`foreach`循环从数据文件一次获取一行。 在这种情况下，行定义中的文件 &#8212; 换行也就是说，每个数据项是在其对应行。 外部循环创建新项 (`<li>`元素) 的有序列表内 (`<ol>`元素)。

    内部循环将每个数据行拆分为使用逗号作为分隔符的项 （字段）。 （根据前面的示例，这意味着每个行包含三个字段和 #8212; 名字、 姓氏和电子邮件地址，每个以逗号分隔）。内部循环还会创建`<ul>`列表和显示一个列表项为每个数据行中的字段。

    代码演示了如何使用两种数据类型、 数组和`char`数据类型。 该数组是必需的因为`File.ReadAllLines`方法返回的数组形式的数据。 `char`数据类型是必需的因为`Split`方法返回`array`其中每个元素的类型均`char`。 (有关数组的信息，请参阅[ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)。)
3. 在浏览器中运行页面。 将显示为前面的示例输入的数据。 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **显示 Microsoft Excel 以逗号分隔文件中的数据**
> 
> 您可以使用 Microsoft Excel 保存电子表格作为以逗号分隔文件中包含的数据 (*.csv*文件)。 执行操作时，该文件将保存在不采用 Excel 格式的纯文本。 在电子表格中的每个行分隔的文本文件中以换行符结尾的用逗号分隔每个数据项。 在前面的示例所示的代码可用于读取 Excel 以逗号分隔文件，只需通过更改你的代码中的数据文件的名称。


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>删除文件

若要从你的网站中删除文件，可以使用`File.Delete`方法。 此过程演示如何使用户可以删除映像 (*.jpg*文件) 从*映像*文件夹，如果他们知道文件的名称。

> [!NOTE] 
> 
> **重要**在生产网站中，你通常限制允许哪些人具有对数据进行更改。 有关如何将成员资格设置以及如何授权用户的站点上执行任务的信息，请参阅[添加安全和 ASP.NET Web Pages 站点的成员资格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在网站中，创建名为的子*映像*。
2. 复制一个或多个*.jpg*文件到*映像*文件夹。
3. 在网站的根目录，创建一个名为的新文件*FileDelete.cshtml*。
4. 将现有内容替换为以下： 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    此页包含窗体用户可以在其中输入图像文件的名称。 他们不输入*.jpg*文件扩展名; 通过限制如下的文件名称，则帮助阻止用户删除你的站点上的任意文件。

    该代码读取用户已输入，然后构造的完整路径的文件名称。 若要创建路径，该代码使用当前的网站路径 (如返回`Server.MapPath`方法)，则*映像*文件夹名称、 用户已提供的名称和作为文字字符串的".jpg"。

    若要删除的文件，该代码调用`File.Delete`方法，将其传递你刚构建的完整路径。 在结束标记时，代码将显示确认消息的文件被删除。
5. 在浏览器中运行页面。 

    ![[image]](working-with-files/_static/image5.jpg)
6. 输入要删除，然后单击的文件的名称**提交**。 如果该文件已删除，文件的名称被显示在页面底部。

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>让用户上载一个文件

`FileUpload`帮助器允许用户将文件上载到你的网站。 下面的过程演示了如何让用户上载单个文件。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你以前未将其添加。
2. 在*应用\_数据*文件夹中，创建一个新文件夹并将其命名*UploadedFiles*。
3. 在根中，创建一个名为的新文件*FileUpload.cshtml*。
4. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    此页面的正文部分使用`FileUpload`帮助器创建上载框和你可能熟悉的按钮：

    ![[image]](working-with-files/_static/image6.jpg)

    有关设置的属性的`FileUpload`帮助器指定你希望提交按钮以读取和你想要上载的文件的单个框**上载**。 （你将添加多个框本文后面的部分。）

    当用户单击**上载**，在页面顶部的代码获取文件，并将其保存。 `Request`你通常使用从窗体字段中获取值的对象还具有`Files`数组，其中包含的文件 （或文件） 的已上载。 你可以超出数组 #8212; 中的特定位置的单个文件例如，若要获取第一个上载的文件，你获得`Request.Files[0]`，若要获取第二个文件，你获得`Request.Files[1]`，依次类推。 （记住，在编程中，计数通常从零开始）。

    当提取已上载的文件时，你将其放入变量 (在这里， `uploadedFile`)，以便可以对其进行操作。 若要确定已上载的文件的名称，您只会收到其`FileName`属性。 但是，当用户上载一个文件时，才`FileName`包含用户的原始名称，包括完整路径。 它可能如下所示：

    *C:\Users\Public\Sample.txt*

    不过，因为这是用户的计算机，不是针对你的服务器上的路径，你不希望所有该路径信息。 你只是想实际文件名 (*Sample.txt*)。 可以通过使用剥离出只需从路径文件`Path.GetFileName`方法，如下：

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path`对象是一个实用工具，具有大量如下可以使用条带路径、 合并路径，等的方法。

    一旦你已收到已上载的文件的名称，你可以构建你想要将上载的文件存储在你的网站的新路径。 在这种情况下，你将结合`Server.MapPath`，文件夹名称 (*应用\_数据/UploadedFiles*)，以及要创建一个新路径的新提取的文件名称。 然后，你可以调用已上载的文件的`SaveAs`方法以实际将保存该文件。
5. 在浏览器中运行页面。 

    ![[image]](working-with-files/_static/image7.jpg)
6. 单击**浏览**，然后选择要上载的文件。 

    ![[image]](working-with-files/_static/image8.jpg)

    下一步的文本框**浏览**按钮将包含的路径和文件位置。

    ![[image]](working-with-files/_static/image9.jpg)
7. 单击“上载” 。
8. 在网站中，右击项目文件夹，然后单击**刷新**。
9. 打开*UploadedFiles*文件夹。 你上载的文件位于文件夹中。 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>让用户上载多个文件

在前面的示例中，你可以让用户上载一个文件。 但你可以使用`FileUpload`帮助器一次上载多个文件。 此方法非常方便和上载的照片，其中一次上载一个文件单调乏味一样的方案。 (你可以阅读上载照片[处理 ASP.NET Web Pages 站点中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。)此示例演示如何以使用户可以上载两个一次，不过可以使用相同的技术来上载多个程序。

1. 将 ASP.NET Web 帮助程序库添加到你的网站中所述[安装 ASP.NET 网页站点中的帮助器](https://go.microsoft.com/fwlink/?LinkId=252372)，如果你尚未。
2. 创建一个名为的新页*FileUploadMultiple.cshtml*。
3. 在页中的现有内容替换为以下：  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    在此示例中，`FileUpload`页的正文中的帮助器以使用户可以将两个文件上载默认配置。 因为`allowMoreFilesToBeAdded`设置为`true`，帮助器呈现一个链接，允许用户添加更多上载框：

    ![[image]](working-with-files/_static/image11.jpg)

    若要处理用户上载的文件，该代码使用在前面的示例和 #8212; 中使用的相同基本技术获取从文件`Request.Files`然后将其保存。 （包括各种内容需要操作，以正确的文件名和路径。）这一次的创新是用户可能会上载多个文件，并且你不知道许多。 若要了解，你可以获取`Request.Files.Count`。

    与现有此数字，则可以遍历`Request.Files`，反过来，提取每个文件并将其保存。 如果你想要循环访问集合的已知的次数，你可以使用`for`循环，如下：

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    变量`i`是只会从零到您设置任何上限的临时计数器。 在这种情况下，上限为的文件数。 但由于计数器开始从零开始，适用于计数在 ASP.NET 中的方案的典型的上限值实际是一个小于该文件数。 （如果三个文件上载，计数为零为 2。）

    `uploadedCount`变量进行合计已成功上载并保存的所有文件。 此代码中占了预期的文件可能无法要上载的可能性。
4. 在浏览器中运行页面。 浏览器显示页和其两个上载框。
5. 选择要上载的两个文件。
6. 单击**添加另一个文件**。 该页显示一个新的上载框。 

    ![[image]](working-with-files/_static/image12.jpg)
7. 单击“上载” 。
8. 在网站中，右击项目文件夹，然后单击**刷新**。
9. 打开*UploadedFiles*文件夹以查看已成功上载的文件。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[使用 ASP.NET Web 页站点中的图像](https://go.microsoft.com/fwlink/?LinkId=202897)

[将导出到 CSV 文件](https://msdn.microsoft.com/library/ms155919.aspx)
