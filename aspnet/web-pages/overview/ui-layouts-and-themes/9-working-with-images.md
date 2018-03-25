---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: "使用 ASP.NET Web 页 (Razor) 站点中的映像 |Microsoft 文档"
author: tfitzmac
description: "本章展示如何添加、 显示和操作图像 （调整大小、 翻转，并添加水印） 在你的网站。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web 页 (Razor) 站点中的图像
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 这篇文章演示如何添加、 显示和操作图像 （调整大小、 翻转，并添加水印） 中的 ASP.NET Web 页 (Razor) 网站。
> 
> 你将学习：
> 
> - 如何动态添加到页面的图像。
> - 如何让的用户上载的映像。
> - 如何调整图像大小。
> - 如何翻转或旋转图像。
> - 如何向映像添加水印。
> - 如何将图像用作水印。
> 
> 这些是 ASP.NET 编程文章中引入的功能：
> 
> - `WebImage`帮助器。
> - `Path`对象，它提供可用于操作路径和文件名的方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 2
> - WebMatrix 2
>   
> 
> 本教程还适用于 WebMatrix 3。


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>动态添加到 Web 页的图像

在开发该网站时，可以向你的网站和各个页添加图像。 你还可以让用户上载可能适用于任务，例如允许他们添加个人资料照片的映像。

如果映像已在你的站点上可用，并且只是想要将其显示在页面上，则使用 HTML`<img>`元素如下：

[!code-html[Main](9-working-with-images/samples/sample1.html)]

有时，不过，你需要能够动态显示图像 &#8212;也就是说，你不知道运行哪些图像之前页面显示。

此部分中的过程演示如何在运行过程中用户在其中指定图像文件名称从映像名称的列表中显示图像。 他们选择的映像名称从下拉列表中，并且当它们提交页时，将显示它们所选择的映像。

![[image]] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. 在 WebMatrix 中，创建新网站。
2. 添加一个名为的新页*DynamicImage.cshtml*。
3. 在网站的根文件夹中，添加一个新文件夹并将其命名*映像*。
4. 四个将映像添加到*映像*刚创建的文件夹。 （任何映像你安装方便，但它们应适合拖到页面。）重命名图像*Photo1.jpg*， *Photo2.jpg*， *Photo3.jpg*，和*Photo4.jpg*。 (不会使用*Photo4.jpg*在此过程中，但你将使用它在文章的后面。)
5. 验证为只读未标记的四个图像。
6. 在页中的现有内容替换为以下：

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    页面的正文都具有一个下拉列表 (`<select>`元素)，名为`photoChoice`。 列表具有三个选项和`value`的每个列表选项的属性具有的一个放在映像名称*映像*文件夹。 实质上，则列表让用户选择友好名称，例如&quot;照片 1&quot;，然后传递*.jpg*时提交页面的文件名称。

    在代码中，你可以获取用户的选择 （换而言之，图像文件名称） 从列表通过阅读`Request["photoChoice"]`。 你首先查看是否存在所选内容根本。 如果没有，你构造组成为映像的文件夹名称和用户的图像文件名称的映像的路径。 (如果你尝试以构造的路径，但没有在中为 nothing `Request["photoChoice"]`，你会收到一个错误。)这会导致的相对路径如下：

    *图像/Photo1.jpg*

    路径存储在变量名为`imagePath`，你将需要更高版本在页中。

    在正文中，此外还有`<img>`元素，用于显示图像中选取用户。 `src`属性未设置为文件的文件名或 URL，就像将以显示静态元素。 相反，它设置为`@imagePath`，这意味着它在代码中设置的路径中获取其值。

    第一次运行页面，不过，没有图像若要显示，因为用户尚未选择任何内容。 这通常意味着，`src`属性将为空，并且该映像将显示一个红色&quot;x&quot; （或如果它找不到映像浏览器呈现的任何内容）。 若要防止此情况，你将`<img>`中的元素`if`块，可测试以查看是否`imagePath`变量中有任何内容。 如果用户所做的选择而`imagePath`包含的路径。 如果用户未选择映像，或者如果这是首次将显示的页，`<img>`元素即使不呈现。
7. 保存文件并在浏览器中运行页面。 (请确保页中选择**文件**工作区之前运行它。)
8. 从下拉列表中选择映像，然后单击**示例映像**。 请确保你看到不同的选择为不同的映像。

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>上载映像

前面的示例演示如何动态，显示图像，但它仅使用已在网站的映像的工作。 此过程演示如何让用户上载的映像，然后显示在页上。 在 ASP.NET 中，你能够在运行过程中使用的映像`WebImage`帮助器，有可让你创建、 处理和保存映像的方法。 `WebImage`帮助器支持所有常见 web 图像文件类型，包括*.jpg*， *.png*，和*.bmp*。 在整篇文章中，你将使用*.jpg*映像，但你可以使用任何图像类型。

![[image]] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. 添加新的页并将其命名*UploadImage.cshtml*。
2. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    文本的正文具有`<input type="file">`元素，它使用户可以选择要上载的文件。 在单击时**提交**，以及窗体提交他们选择的文件。

    若要获取已上载的图像，你可以使用`WebImage`帮助器，具有各种类型的有用的方法，用于处理图像。 具体而言，使用`WebImage.GetImageFromRequest`（如果有） 获取已上载的图像并将其存储在名为`photo`。

    在此示例中的工作很多涉及获取和设置文件和路径名称。 问题在于你想要获得用户上载的映像名称 （而不仅仅是名称），然后创建要在其中存储映像的新路径。 因为用户可能无法上载具有相同名称的多个映像，你使用的额外的代码以创建唯一的名称，并确保用户不会覆盖现有的图片。

    如果实际已上载图像 (测试`if (photo != null)`)，从图像的获取映像名称`FileName`属性。 当用户上载的图像，`FileName`包含用户的原始名称，其中包括从用户的计算机的路径。 它可能如下所示：

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    你不希望所有这些路径信息，但 &#8212;你只是想实际文件名 (*SamplePhoto1.jpg*)。 可以通过使用剥离出只需从路径文件`Path.GetFileName`方法，如下：

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    您然后通过将 GUID 添加到原始名称创建新的唯一文件名。 (有关详细信息的 Guid，请参阅[有关 Guid](#SB_AboutGUIDs)这篇文章中更高版本。)然后，你构建可用于保存图像的完整路径。 在保存路径组成新的文件名称、 文件夹 （映像） 和当前网站位置。

    > [!NOTE]
    > 为了使你的代码以将文件保存于*映像*文件夹中，应用程序需要读写权限为该文件夹。 你的开发计算机上这是不通常问题。 但是，当你的站点发布到宿主提供程序的 web 服务器时，你可能需要显式设置这些权限。 如果你在托管提供商的服务器上运行此代码，并获取错误，请与宿主提供程序中，若要了解如何设置这些权限。

    最后，你将传递保存路径`Save`方法`WebImage`帮助器。 这将存储已上载的图像采用新名称。 保存方法如下所示： `photo.Save(@"~\" + imagePath)`。 完整路径追加到`@"~\"`，这是当前的网站位置。 (有关信息`~`运算符，请参阅[ASP.NET Web 编程使用 Razor 语法的简介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)。)

    与前面的示例中，页的正文包含`<img>`元素将显示图像。 如果`imagePath`已设置`<img>`呈现元素及其`src`属性设置为`imagePath`值。
3. 在浏览器中运行页面。
4. 上载的图像，并确保在页中显示。
5. 在你的站点，打开*映像*文件夹。 你看到已其文件名看上去如下所示，添加一个新文件:: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    这是使用名称的前缀的 GUID 上载的映像。 (你自己的文件不会有不同的 GUID 和可能名为不同于*MyPhoto.png*。)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>有关 Guid
> 
> GUID (全局唯一 ID) 是通常以如下格式呈现的标识符： `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`。 数字和字母 （从 A 到 F) 与不同，对于每个 GUID，但它们都遵循使用组 8-4-4-4-12 个字符的模式。 （从技术上讲，GUID 是一个 16 字节 128 位数字。）当你需要一个 GUID 时，你可以调用专门为你生成一个 GUID 的代码。 Guid 思想是之间的数很大的规模 (3.4 x 10<sup>38</sup>) 和用于生成它的算法，得到数几乎保证是一种类型之一。 Guid 因此是一种好的方法时必须保证不会使用两次相同的名称生成操作的名称。 其缺点，当然，在于，Guid 并不特别是在用户友好，因此，他们往往会用于仅在代码中使用的名称。


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>调整图像的大小

如果你的网站接受来自用户的映像，你可能想要调整图像的大小之前显示或将其保存。 你可以再次使用`WebImage`此帮助器。

此过程演示如何调整已上载的图像创建缩略图，然后保存的网站中的缩略图和原始图像大小。 可以显示缩略图在页面上，并使用超链接将用户重定向到的全尺寸的图像。

![[image]] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. 添加一个名为的新页*Thumbnail.cshtml*。
2. 在*映像*文件夹中，创建名为的子*拇指*。
3. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    此代码是从前面的示例类似于代码。 区别是，此代码将图像保存两次，一次通常一次后创建缩略图图像的副本。 首先获取已上载的图像，并将其保存在*映像*文件夹。 然后构造一个用于缩略图新路径。 若要实际创建缩略图，请调用`WebImage`帮助器的`Resize`方法来创建一个 60 像素通过 60 像素的图像。 该示例演示如何保留纵横比以及如何可以防止图像正在扩大了 （以防新的大小实际上会使图像更大）。 调整大小的图像则保存在*拇指*子文件夹。

    在结束标记时，你使用相同`<img>`具有动态元素`src`你已了解在前面的示例，则有条件地显示图像的属性。 在这种情况下，你会显示缩略图。 你还使用`<a>`元素以创建大版本的映像的超链接。 与`src`属性`<img>`元素，你设置`href`属性`<a>`动态地向任何处于元素`imagePath`。 若要确保该路径可以充当一个 URL，将传递`imagePath`到`Html.AttributeEncode`方法，后者将在路径中的保留的字符转换为确定在 URL 中的字符。
4. 在浏览器中运行页面。
5. 上载照片，并验证已显示缩略图。
6. 单击以查看实际尺寸的图像的缩略图。
7. 在*映像*和*映像/拇指*，请注意，已添加新文件。

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>旋转和翻转图像

`WebImage`帮助器还可以翻转和旋转图像。 此过程演示如何从服务器获取映像、 （垂直） 倒置翻转图像、 将其保存，然后显示在页面上的翻转的图像。 在此示例中，你仅用在服务器已有的文件 (*Photo2.jpg*)。 在实际应用中，你可能将翻转图像动态，如你在前面的示例获取其名称。

![[image]] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. 添加一个名为的新页*FlipImage.cshtml*。
2. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    该代码使用`WebImage`帮助器要从服务器获取其图像。 创建使用相同的技术来保存图像，在前面的示例中使用的映像的路径并将该路径传递时映像使用创建`WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    如果找不到映像，你构造的新路径和文件名称，如你在前面的示例。 若要翻转图像，请调用`FlipVertical`方法，然后再次保存该图像。

    映像试页面上显示通过使用`<img>`具有元素`src`属性设置为`imagePath`。
3. 在浏览器中运行页面。 此图像可查看*Photo2.jpg*正面朝下显示。
4. 刷新页面或请求页上再次请参阅映像向上是翻转的右侧试。

若要将图像的旋转，你可以使用相同的代码，而不是调用只不过`FlipVertical`或`FlipHorizontal`，你调用`RotateLeft`或`RotateRight`。

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>添加到图像的水印

当将映像添加到你的网站时，你可能想要添加到映像水印，然后将其保存或将其显示在页面上。 人们通常使用水印，将版权信息添加到映像，或要播发其公司名称。

![[image]] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. 添加一个名为的新页*Watermark.cshtml*。
2. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    此代码就像中的代码是*FlipImage.cshtml*从前面的页 (尽管这次它使用*Photo3.jpg*文件)。 若要添加水印，请调用`WebImage`帮助器的`AddTextWatermark`方法之前保存的图像。 在调用`AddTextWatermark`，传递文本&quot;我水印&quot;、 变为黄色，设置字体颜色和字体系列设置为 Arial。 (尽管不会在这里，显示`WebImage`帮助器，您还可以指定不透明度、 字体系列和字体大小和水印文本的位置。)在保存映像时不得为只读的。

    如你所见之前，该图像显示在页上通过使用`<img>`元素的 src 属性设置为`@imagePath`。
3. 在浏览器中运行页面。 请注意在图像的右下角的"我水印"的文本。

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>作为水印使用的映像

而不是使用水印文本，你可以使用另一个映像。 人们有时作为水印，使用图像，例如公司徽标或它们而不是文本中使用水印图像的版权信息。

![[image]] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. 添加一个名为的新页*ImageWatermark.cshtml*。
2. 将映像添加到*映像*文件夹，你可以用作徽标，并且重命名图像*MyCompanyLogo.jpg*。 此映像应设置为 80 像素宽和 20 像素高时你可以清楚地查看的映像。
3. 在页中的现有内容替换为以下： 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    这是对从前面的示例代码的另一个变体。 在这种情况下，调用`AddImageWatermark`将的水印图像添加到目标映像 (*Photo3.jpg*) 保存图像之前。 当调用`AddImageWatermark`，将其宽度设置为 80 像素和为 20 个像素的高度。 *MyCompanyLogo.jpg*映像是水平对齐在中心和底部目标图像的垂直对齐。 不透明度设置为 100%，并填充设置为 10 个像素。 如果的水印图像大于目标图像，不会发生。 如果的水印图像大于目标图像并设置为零的映像水印的填充，则忽略水印。

    由于在这之前，显示映像使用`<img>`元素和动态`src`属性。
4. 在浏览器中运行页面。 请注意，在主映像的底部显示的水印图像。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他资源


[使用 ASP.NET Web 页站点中的文件](https://go.microsoft.com/fwlink/?LinkId=202896)

[编程使用 Razor 语法的 ASP.NET Web Pages 简介](https://go.microsoft.com/fwlink/?LinkID=251587)
