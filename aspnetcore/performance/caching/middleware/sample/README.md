# <a name="aspnet-core-response-caching-sample"></a>ASP.NET 核心响应缓存示例

本示例演示使用 ASP.NET Core[响应缓存中间件](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)。

应用程序通过其索引页上，包括`Cache-Control`标头来配置缓存行为。 应用程序还将设置`Vary`标头来配置缓存，以提供响应才`Accept-Encoding`的后续请求的标头中的匹配的原始请求。

运行示例时，索引页是从缓存而存储和缓存最多 10 秒时提供。
