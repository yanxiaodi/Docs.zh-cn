<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>搭建“电影”模型的基架

* 从命令行（在包含 Program.cs、Startup.cs 和 .csproj 文件的项目目录中）中运行如下命令：

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

如果收到错误：
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

打开一个到项目目录（包含 Program.cs、Startup.cs 和 .csproj 文件的目录）的命令 shell。

如果收到错误：
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

退出 Visual Studio，然后重新运行命令。
