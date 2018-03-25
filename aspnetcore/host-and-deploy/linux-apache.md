---
title: "使用 Apache 在 Linux 上托管 ASP.NET Core"
description: "了解如何设置 Apache 为反向代理服务器在 CentOS 中将 HTTP 流量重定向到 ASP.NET 核心 web 应用程序在 Kestrel 上运行。"
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 033adddc586b60c9f7453df5434617aa838737f8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>使用 Apache 在 Linux 上托管 ASP.NET Core

作者：[Shayne Boyer](https://github.com/spboyer)

使用本指南中，了解如何设置[Apache](https://httpd.apache.org/)为反向代理服务器上[CentOS 7](https://www.centos.org/)将 HTTP 流量重定向到 ASP.NET 核心 web 应用程序上运行[Kestrel](xref:fundamentals/servers/kestrel)。 [Mod_proxy 扩展](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)和相关的模块创建服务器的反向代理。

## <a name="prerequisites"></a>系统必备

1. 具有 sudo 特权的标准用户帐户运行 CentOS 7 服务器
2. ASP.NET Core 应用

## <a name="publish-the-app"></a>发布应用

发布应用程序作为[独立的部署](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 运行时的版本配置中 (`centos.7-x64`)。 内容复制*bin/Release/netcoreapp2.0/centos.7-x64/publish*使用 SCP、 FTP 或其他文件传输方法向服务器的文件夹。

> [!NOTE]
> 在生产部署场景，持续集成工作流执行的工作的发布应用程序和复制到服务器的资产。 

## <a name="configure-a-proxy-server"></a>配置代理服务器

反向代理是为动态 web 应用程序提供服务的常见设置。 反向代理终止 HTTP 请求，并将其转发到 ASP.NET 应用程序。

代理服务器，则其中一个客户端将请求转发到另一个服务器而不是本身满足请求。 反向代理转发到固定的目标，通常代表任意客户端。 在本指南中，Apache 正在配置为在同一服务器上运行 Kestrel 为提供 ASP.NET Core 应用程序服务的反向代理。

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

### <a name="install-apache"></a>安装 Apache

更新 CentOS 程序包添加到其最新稳定版本：

```bash
sudo yum update -y
```

使用单个在 CentOS 上安装 Apache web 服务器`yum`命令：

```bash
sudo yum -y install httpd mod_ssl
```

运行该命令后输出的示例：

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> 在此示例中，输出将反映 httpd.86_64，由于 CentOS 7 版本是 64 位。 若要验证 Apache 的安装位置，请从命令提示符运行 `whereis httpd`。

### <a name="configure-apache-for-reverse-proxy"></a>配置 Apache 用于反向代理

Apache 的配置文件位于 `/etc/httpd/conf.d/` 目录内。 具有的所有文件*.conf*扩展处理除了中的模块配置文件按字母顺序`/etc/httpd/conf.modules.d/`，其中包含的任何配置所需加载模块文件。

创建一个配置文件，名为*hellomvc.conf*，应用程序：

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

`VirtualHost`块可以出现多次，在服务器上的一个或多个文件。 在前面的配置文件中，Apache 接受公共端口 80 上的流量。 域`www.example.com`正在提供服务，与`*.example.com`别名解析为同一网站。 请参阅[基于名称的虚拟主机支持](https://httpd.apache.org/docs/current/vhosts/name-based.html)有关详细信息。 请求是服务器的代理针对端口 5000 上 127.0.0.1 的根目录。 对于双向通信，`ProxyPass`和`ProxyPassReverse`所需。

> [!WARNING]
> 如果未能指定合适[ServerName 指令](https://httpd.apache.org/docs/current/mod/core.html#servername)中**VirtualHost**块公开您的应用程序安全漏洞。 子域通配符绑定 (例如， `*.example.com`) 不会带来安全风险，若要控制整个父域 (相对于`*.com`，这是易受攻击)。 请参阅[rfc7230 部分 5.4](https://tools.ietf.org/html/rfc7230#section-5.4)有关详细信息。

可以每个配置日志记录`VirtualHost`使用`ErrorLog`和`CustomLog`指令。 `ErrorLog` 是服务器用来记录错误的位置和`CustomLog`设置的文件名和日志文件格式。 在这种情况下，这是记录请求信息的位置。 没有为每个请求的一行。

将文件保存与测试配置。 如果一切正常，响应应为 `Syntax [OK]`。

```bash
sudo service httpd configtest
```

重新启动 Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>监视应用程序

Apache 现在已设置为转发到发出的请求`http://localhost:80`Kestrel 在上运行 ASP.NET Core 应用到`http://127.0.0.1:5000`。  但是，Apache 未设置来管理 Kestrel 过程。 使用*systemd*和创建服务文件以启动和监视基础的 web 应用。 systemd 是一个 init 系统，可以提供用于启动、停止和管理进程的许多强大的功能。 


### <a name="create-the-service-file"></a>创建服务文件

创建服务定义文件：

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

应用程序示例服务文件：

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **用户**&mdash;如果用户*apache*未使用的配置，用户必须创建第一次并且为文件提供的适当的所有权。

保存该文件并启用该服务：

```bash
systemctl enable kestrel-hellomvc.service
```

启动服务，然后验证它正在运行：

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

使用反向代理配置和通过管理的 Kestrel *systemd*，web 应用进行了完全配置，并且可以从处的本地计算机上的浏览器访问`http://localhost`。 检查响应标头，**服务器**标头指示 ASP.NET Core 应用由 Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>查看日志

因为 web 应用使用 Kestrel 管理使用*systemd*，到集中式日志记录事件和进程。 但是，此日志包含的服务和由托管的进程的所有条目*systemd*。 若要查看特定于 `kestrel-hellomvc.service` 的项，请使用以下命令：

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

时间筛选，使用命令中指定时间选项。 例如，使用`--since today`来筛选出存在当天或`--until 1 hour ago`来查看前一小时的条目。 有关详细信息，请参阅[journalctl 手册页](https://www.unix.com/man-page/centos/1/journalctl/)。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>保护应用程序

### <a name="configure-firewall"></a>配置防火墙

*Firewalld*是动态的守护程序，来管理具有对网络区域支持的防火墙。 端口和数据包筛选仍可通过 iptables 管理。 *Firewalld*默认情况下应安装。 `yum` 可以用于安装包或验证已安装。

```bash
sudo yum install firewalld -y
```

使用`firewalld`以打开仅为应用程序所需的端口。 在此示例中，使用的是端口 80 和 443。 以下命令永久设置端口 80 和 443 以打开：

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

重新加载防火墙设置。 检查可用的服务和默认区域中的端口。 选项均可通过检查`firewall-cmd -h`。

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a>SSL 配置

若要配置 ssl，Apache*如果*使用模块。 当*httpd*模块已安装，*如果*同时安装模块。 如果它未安装，请使用`yum`以将其添加到配置。

```bash
sudo yum install mod_ssl
```
若要强制实施 SSL，请安装`mod_rewrite`模块以启用 URL 重写：

```bash
sudo yum install mod_rewrite
```

修改*hellomvc.conf*文件启用 URL 重写且安全的端口 443 上的通信：

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> 此示例中使用的本地生成的证书。 **SSLCertificateFile**应为域名称的主证书文件。 **SSLCertificateKeyFile**密钥文件时应生成创建 CSR。 **SSLCertificateChainFile**应中间证书文件 （如果有），由证书颁发机构提供的。

保存该文件并测试配置：

```bash
sudo service httpd configtest
```

重新启动 Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>其他 Apache 建议

### <a name="additional-headers"></a>其他标头

为了保护免受恶意攻击，有几个标头应被修改或添加。 确保`mod_headers`安装模块：

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>从 clickjacking 攻击的安全 Apache

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)，也称为*UI 改变现有攻击*，是一种恶意攻击其中诱骗网站访问者不是它们当前正在访问单击的链接或另一页上的按钮。 使用`X-FRAME-OPTIONS`以确保网站的安全。

编辑*httpd.conf*文件：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

将行添加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`。 保存该文件。 重启 Apache。

#### <a name="mime-type-sniffing"></a>MIME 类型探查

`X-Content-Type-Options`标头会阻止从 Internet Explorer *MIME 探查*(确定文件的`Content-Type`从该文件的内容)。 如果服务器设置`Content-Type`标头到`text/html`与`nosniff`选项集，Internet Explorer 呈现作为内容`text/html`不考虑文件的内容。

编辑*httpd.conf*文件：

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

将行添加`Header set X-Content-Type-Options "nosniff"`。 保存该文件。 重启 Apache。

### <a name="load-balancing"></a>负载平衡 

此示例演示如何在同一实例计算机上的 CentOS 7 和 Kestrel 上设置和配置 Apache。 为了不具有单点故障;使用*mod_proxy_balancer*和修改**VirtualHost**将允许用于管理 Apache 代理服务器后面的 web 应用的多个实例。

```bash
sudo yum install mod_proxy_balancer
```

在配置文件中下面所示的其他实例`hellomvc`应用已设置为在端口 5001 上运行。 *代理*部分设置与平衡器配置具有两个成员进行负载平衡*byrequests*。

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a>速率限制
使用*mod_ratelimit*中, 附带*httpd*模块，客户端的带宽可将限制：

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
示例文件为 600 KB/秒根位置下限制带宽：

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
