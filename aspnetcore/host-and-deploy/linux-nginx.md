---
title: "使用 Nginx 在 Linux 上托管 ASP.NET Core"
author: rick-anderson
description: "描述如何在 Ubuntu 16.04 转发到 ASP.NET 核心 web 应用程序在 Kestrel 上运行的 HTTP 流量的反向代理设置 Nginx。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: a1de177fcd41c925a85e5aab9a0d236249b7da0b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>使用 Nginx 在 Linux 上托管 ASP.NET Core

作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)

本指南介绍如何在 Ubuntu 16.04 服务器上设置生产就绪 ASP.NET Core 环境。

> [!NOTE]
> 对于 Ubuntu 14.04 *supervisord*建议为用于监视 Kestrel 进程的解决方案。 *systemd*在 Ubuntu 14.04 上不可用。 [请参阅本文档的以前版本](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)。

本指南：

* 将现有 ASP.NET Core 应用程序反向代理服务器后面。
* 将设置反向代理服务器将请求转发到 Kestrel web 服务器。
* 可确保在作为后台进程启动时运行的 web 应用。
* 配置有助于重新启动 web 应用的进程管理工具。

## <a name="prerequisites"></a>系统必备

1. 使用具有 sudo 特权的标准用户帐户访问 Ubuntu 16.04 服务器
1. 现有的 ASP.NET Core 应用程序

## <a name="copy-over-the-app"></a>通过应用程序复制

运行[dotnet 发布](/dotnet/core/tools/dotnet-publish)从要打包到一个自包含的目录，可以在服务器上运行的应用程序的开发环境。

将 ASP.NET Core 应用程序复制到使用任何工具集成到组织的工作流 （例如，SCP，FTP） 服务器。 测试应用，例如：

* 从命令行中，运行`dotnet <app_assembly>.dll`。
* 在浏览器中，导航到 `http://<serveraddress>:<port>` 以确认应用在 Linux 上正常运行。 
 
## <a name="configure-a-reverse-proxy-server"></a>配置反向代理服务器

反向代理是为动态 web 应用程序提供服务的常见设置。 反向代理终止 HTTP 请求，并将其转发到 ASP.NET 核心应用程序。

### <a name="why-use-a-reverse-proxy-server"></a>为何使用反向代理服务器？

Kestrel 非常适合从 ASP.NET Core 提供动态内容。 但是，web 服务功能不是为 IIS、 Apache 或 Nginx 例如与服务器的功能丰富。 反向代理服务器可以卸载例如提供静态内容、 缓存请求、 压缩请求和从 HTTP 服务器的 SSL 终止的工作。 反向代理服务器可能驻留在专用计算机上，也可能与 HTTP 服务器一起部署。

鉴于此指南的目的，使用 Nginx 的单个实例。 它与 HTTP 服务器一起运行在同一服务器上。 根据要求，可以选择不同的安装。

因为通过反向代理转发请求，使用从转发标头 Middleware [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)包。 中间件更新`Request.Scheme`，使用`X-Forwarded-Proto`标头，因此该重定向 Uri 和其他安全策略正常工作。

当使用任何类型的身份验证中间件，转发标头中间件必须运行第一个。 此顺序可确保身份验证中间件可使用的标头值并生成正确的重定向 Uri。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)或类似的身份验证方案中间件：

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

调用[UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)中的方法`Startup.Configure`之前调用[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)和[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)或类似的身份验证方案中间件：

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

如果没有[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)指定到中间件，要转发的默认标头是`None`。

### <a name="install-nginx"></a>安装 Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> 如果将安装可选 Nginx 模块，生成从源 Nginx 可能需要。

使用 `apt-get` 安装 Nginx。 安装程序创建一个 System V init 脚本，该脚本运行 Nginx 作为系统启动时的守护程序。 因为是首次安装 Nginx，通过运行以下命令显式启动：

```bash
sudo service nginx start
```

确认浏览器显示 Nginx 的默认登陆页。

### <a name="configure-nginx"></a>配置 Nginx

若要将 Nginx 配置为转发请求向 ASP.NET Core 应用程序的反向代理，修改*/etc/nginx/sites-available/default*。 在文本编辑器中打开它，并将内容替换为以下内容：

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

如果没有`server_name`Nginx 的匹配项，使用默认服务器。 如果定义了默认的服务器，配置文件中的第一个服务器将是默认服务器。 最佳做法，将添加在配置文件中返回 444 状态代码的特定的默认服务器。 默认服务器配置示例是：

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

与上述配置文件和默认服务器，Nginx 接受主机标头使用的端口 80 上的公共流量`example.com`或`*.example.com`。 不会获取与这些主机不匹配的请求转发到 Kestrel。 Nginx 将匹配的请求转发到在 Kestrel `http://localhost:5000`。 请参阅[nginx 如何处理请求](https://nginx.org/docs/http/request_processing.html)有关详细信息。

> [!WARNING]
> 如果未能指定合适[server_name 指令](https://nginx.org/docs/http/server_names.html)公开您的应用程序安全漏洞。 子域通配符绑定 (例如， `*.example.com`) 不会带来安全风险，若要控制整个父域 (相对于`*.com`，这是易受攻击)。 请参阅[rfc7230 部分 5.4](https://tools.ietf.org/html/rfc7230#section-5.4)有关详细信息。

一旦建立 Nginx 配置，运行`sudo nginx -t`若要验证的配置文件的语法。 如果配置文件测试成功，强制 Nginx 以便通过运行选取更改`sudo nginx -s reload`。

## <a name="monitoring-the-app"></a>监视应用程序

服务器已设置为转发到发出的请求`http://<serveraddress>:80`Kestrel 在上运行 ASP.NET Core 应用到`http://127.0.0.1:5000`。 但是，Nginx 未设置来管理 Kestrel 过程。 *systemd*可以用于创建服务文件以启动和监视基础的 web 应用。 systemd 是一个 init 系统，可以提供用于启动、停止和管理进程的许多强大的功能。 

### <a name="create-the-service-file"></a>创建服务文件

创建服务定义文件：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

下面是应用程序的示例服务文件：

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

**注意：**如果用户*www 数据*未使用的配置，此处定义的用户必须创建第一次并且为文件提供的适当的所有权。
**注意：** Linux 具有区分大小写的文件系统。 搜索配置文件中的"生产"结果设置 ASPNETCORE_ENVIRONMENT *appsettings。Production.json*，而不*appsettings.production.json*。

保存该文件并启用该服务。

```bash
systemctl enable kestrel-hellomvc.service
```

启动服务并验证它正在运行。

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

配置反向代理和管理通过 systemd Kestrel，web 应用完全配置，并且可以从处的本地计算机上的浏览器访问`http://localhost`。 它也是可从远程计算机，禁止任何防火墙可能阻止访问。 检查响应标头，`Server`标头将显示 ASP.NET Core 应用程序提供的 Kestrel。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>查看日志

因为 web 应用使用 Kestrel 管理使用`systemd`，到集中式日志记录所有事件和进程。 但是，此日志包含由 `systemd` 管理的所有服务和进程的全部条目。 若要查看特定于 `kestrel-hellomvc.service` 的项，请使用以下命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

有关进一步筛选，使用时间选项（如 `--since today`、`--until 1 hour ago`）或这些选项的组合可以减少返回的条目数。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>保护应用程序

### <a name="enable-apparmor"></a>启用 AppArmor

Linux 安全模块 (LSM) 是一个框架，是从 Linux 2.6 Linux 内核的一部分。 LSM 支持安全模块的不同实现。 [AppArmor](https://wiki.ubuntu.com/AppArmor) 是实现强制访问控制系统的 LSM，它允许将程序限制在一组有限的资源内。 确保已启用并成功配置 AppArmor。

### <a name="configuring-the-firewall"></a>配置防火墙

关闭所有未使用的外部端口。 通过为配置防火墙提供命令行接口，不复杂的防火墙 (ufw) 为 `iptables` 提供了前端。 验证`ufw`配置为允许所需的任何端口上的流量。

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>保护 Nginx

Nginx 的默认分配不启用 SSL。 若要启用其他安全功能，请从源生成。

#### <a name="download-the-source-and-install-the-build-dependencies"></a>下载源并安装生成依赖项

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>更改 Nginx 响应名称

编辑 src/http/ngx_http_header_filter_module.c：

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>配置选项和生成

正则表达式需要 PCRE 库。 正则表达式用于 ngx_http_rewrite_module 的位置指令。 http_ssl_module adds HTTPS 协议支持。

请考虑使用类似的 web 应用程序防火墙*ModSecurity*来加强应用程序。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>配置 SSL

* 配置服务器以侦听端口上的 HTTPS 流量`443`通过指定由受信任证书颁发机构 (CA) 颁发的有效证书。

* 通过采用的一些做法在下面的示例所示加强安全*/etc/nginx/nginx.conf*文件。 示例包括选择更强的密码并将通过 HTTP 的所有流量重定向到 HTTPS。

* 添加 `HTTP Strict-Transport-Security` (HSTS) 标头可确保由客户端发起的所有后续请求都仅通过 HTTPS。

* 不添加 Strict 传输安全标头或选择适当`max-age`如果将在将来禁用 SSL。

添加 /etc/nginx/proxy.conf 配置文件：

[!code-nginx[](linux-nginx/proxy.conf)]

编辑 /etc/nginx/nginx.conf 配置文件。 示例包含一个配置文件中的 `http` 和 `server` 部分。

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>保护 Nginx 免受点击劫持的侵害
点击劫持是收集受感染用户的点击数的恶意技术。 点击劫持诱使受害者（访问者）点击受感染的网站。 使用 X-框架的选项以确保网站的安全。

编辑 nginx.conf 文件：

```bash
sudo nano /etc/nginx/nginx.conf
```

添加行 `add_header X-Frame-Options "SAMEORIGIN";` 并保存文件，然后重新启动 Nginx。

#### <a name="mime-type-sniffing"></a>MIME 类型探查

此标头可阻止大部分浏览器通过 MIME 方式探查来自已声明内容类型的响应，因为标头会指示浏览器不要替代响应内容类型。 使用 `nosniff` 选项后，如果服务器认为内容是“文本/html”，则浏览器将其显示为“文本/html”。

编辑 nginx.conf 文件：

```bash
sudo nano /etc/nginx/nginx.conf
```

添加行 `add_header X-Content-Type-Options "nosniff";` 并保存文件，然后重新启动 Nginx。
