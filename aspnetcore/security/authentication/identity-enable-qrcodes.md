---
title: "启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成"
author: rick-anderson
description: "了解如何启用使用 ASP.NET Core 双因素身份验证的身份验证器应用的 QR 代码生成。"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: dd326bb32565b743d21e196bcb616a716d7994bf
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>启用 ASP.NET Core 中的身份验证器应用的 QR 代码生成

注意： 本主题适用于 ASP.NET Core 2.x

ASP.NET 核心附带的单个身份验证的身份验证器应用程序的支持。 两个因素身份验证 (2FA) 身份验证器应用，使用基于时间的一次性密码算法 (TOTP)，是推荐的方法为 2FA 行业。 2FA 使用 TOTP 优于 SMS 2FA。 验证器应用提供哪些用户确认其用户名和密码后，必须输入一个 6 到 8 位代码。 通常在智能手机上安装验证器应用。

ASP.NET 核心 web 应用程序模板支持身份验证器，但不提供对 QRCode 生成的支持。 QRCode 生成器轻松地 2FA 的安装程序。 本文档将指导你完成添加[QR 代码](https://wikipedia.org/wiki/QR_code)生成到 2FA 配置页。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>将 QR 代码添加到 2FA 配置页

这些说明使用*qrcode.js*从https://davidshimjs.github.io/qrcodejs/存储库。

* 下载[qrcode.js javascript 库](https://davidshimjs.github.io/qrcodejs/)到`wwwroot\lib`项目文件夹中的。

* 在*Pages\Account\Manage\EnableAuthenticator.cshtml* （Razor 页） 或*Views\Manage\EnableAuthenticator.cshtml* (MVC)、 找到`Scripts`文件末尾的部分：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新`Scripts`部分添加到引用`qrcodejs`你添加的库和生成的 QR 代码的调用。 其外观应，如下所示：

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* 删除的段落，您链接到这些说明。

运行你的应用，并确保你可以扫描 QR 代码和验证身份验证器证明的代码。

## <a name="change-the-site-name-in-the-qr-code"></a>更改 QR 代码中的站点名称

最初创建你的项目时选择的项目名称中获取 QR 代码中的站点名称。 你可以通过查找对其进行更改`GenerateQrCodeUri(string email, string unformattedKey)`中的方法*Pages\Account\Manage\EnableAuthenticator.cshtml.cs* （Razor 页） 文件或*Controllers\ManageController.cs* (MVC) 文件。 

从模板的默认代码将如下所示：

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

第二个参数的调用中`string.Format`是你的站点名称，取自解决方案名称。 它可以更改为任何值，但它必须始终为 URL 编码。

## <a name="using-a-different-qr-code-library"></a>使用不同的 QR 代码库

QR 代码库可以替换你首选的库。 HTML 包含`qrCode`元素在其中可以通过任何机制将 QR 代码你的库提供。

QR 代码的格式正确 URL 可用于:

* `AuthenticatorUri` 模型的属性。
* `data-url` 中的属性`qrCodeData`元素。 

## <a name="totp-client-and-server-time-skew"></a>TOTP 客户端和服务器时间偏差

TOTP 身份验证依赖于服务器和身份验证器的设备具有精确的时间。 仅在 30 秒内，最后一个令牌。 如果未 TOTP 2FA 登录名，请检查服务器的时间比准确，并且最好是同步到准确 NTP 服务。
