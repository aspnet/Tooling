# Known issues for .NET Core 2.0, ASP.NET Core 2.0, and ASP.NET and Web Tools in Visual Studio 2017 15.3 "Preview"

This page is specifically for issues with the .NET Core and ASP.NET Core 2.0 Previews, and the Visual Studio 2017 Preview.  For issues with Visual Studio 2017, or .NET Core/ASP.NET Core 1.x see our [Visual Studio 2017 Known Issues page](known-issues-vs2017.md). If you encounter any issues not already captured, you can report them using the Report a Problem tool on our [Developer Community](https://developercommunity.visualstudio.com/) site, where you can track the status and see issues reported by others as well. 

## .NET Core Known Issues

### Dependencies node in solution explorer shows with a warning icon until restore is complete

* #### Issue: 
When you create or open an existing .NET Core or ASP.NET Core project using Visual Studio 2017, the dependencies node shows warnings until all packages are restored and intellisense is ready. See [this issue](https://github.com/dotnet/project-system/issues/2079) for details.

![image](https://cloud.githubusercontent.com/assets/8246794/25904231/78210626-3553-11e7-8259-02d4239fb79c.png)

* #### Workaround:
Simply wait for package restore to complete and the warnings will go away

### Missing NuGet node and package icons

* #### Issue: 
Users with High DPI monitors may observe missing NuGet icons

![image](https://cloud.githubusercontent.com/assets/8246794/26131110/daa49c9c-3a4c-11e7-956d-499a2bc22c38.png)

* #### Workaround:
You can resolve this by installing the [15.3.0 Preview 3 update to VS 2017](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-preview-relnotes). 

## ASP.NET Core 2.0 and Web Tools Known Issues

### Warning in the error list about Microsoft.Composition 1.0.27 getting restored in a way that may cause compatibility problems

* #### Issue: 
When you create a new ASP.NET Core 2.0 project using the Web Application template with 'Individual User Accounts' authentication, or when you use scaffolding to add Minimal Dependencies to an ASP.NET Core 2.0 project created using the Empty template, you get a warning in the error list about Microsoft.Composition 1.0.27 getting restored in a way that may cause compatibility problems. 
	
The package Microsoft.VisualStudio.Web.CodeGeneration.Design depends on Microsoft.CodeAnalysis.Workspaces, which in turn 
depends on the Microsoft.Composition v 1.0.27 which causes this incompatibility warning. 

![image](https://user-images.githubusercontent.com/8246794/27614916-6842fe7c-5b59-11e7-9e6c-3e3fed1a5874.png)

* #### Workaround:
None. The warning is benign and can be ignored. We are working on updating Microsoft.VisualStudio.Web.CodeGeneration.Design to depend on a version of Microsoft.CodeAnalysis.Workspaces that will not cause incompatibility warnings.

### When you drag a file out of wwwroot to the project root, you get error 'Failed to update the project tree'

* #### Issue: 
In Solution Explorer of an ASP.NET Core project, when you drag an html file (or other file) out of wwwroot to the project root, you get the following error

	---------------------------
	Microsoft Visual Studio
	---------------------------
	The project system has encountered an error.
	 
	Failed to update the project tree.
	A diagnostic log has been written to the following location: "C:\Users\<user-name>\AppData\Local\Temp\VsProjectFault_b80d13d6-ca81-4c51-b349-5b1a2af1639f.failure.txt".
	---------------------------
	OK   
	---------------------------


* #### Workaround:
Close and re-open the solution in VS should get you back to a good state. 
To avoid such errors, you can use File Explorer in Windows to perform this operation instead of Solution Explorer. 



### When you create a new ASP.NET Core 2.0 project using the Angular template, there is a warning in Dependencies node

* #### Issue: 
When you create a new ASP.NET Core 2.0 project using the Angular template, there is a warning in Dependencies node that does not go away for a long time, until the npm packages are fully installed

![image](https://user-images.githubusercontent.com/8246794/27614995-f6e2d238-5b59-11e7-8d7a-30e53ce43ce5.png)

* #### Workaround:
Wait until npm packages are fully installed. You can view progress in the 'Bower/npm' pane of the Output Window


### Certificate error when trying to apply EF migrations or using code generation

* #### Issue: 
When trying to apply EF migrations or when using code generation to scaffold code in an ASP.NET Core 2.0 application, you get error:
'No certificate named 'HTTPS' found in configuration for the current environment (Production).

![image](https://cloud.githubusercontent.com/assets/8246794/25904334/ce13d6e4-3553-11e7-86ad-445eace7a5a2.png)

* #### Workaround:
Start a Developer Command Prompt, set the environment variable ASPNETCORE_ENVIRONMENT=Development and then start VS with this environment variable set

![image](https://cloud.githubusercontent.com/assets/8246794/25904375/ec263bc2-3553-11e7-8ebb-f67f85da62d7.png)

### Cannot resolve scoped service error when trying to apply EF migrations or during code generation using existing DbContext

* #### Issue: 
When trying to apply EF migrations at design-time or during a publish, or during code generation, you might run into an error such as: Cannot resolve scoped service 'WebApplication2.Identity.Data.IdentityServiceDbContext' from root provider – even when ASPNETCORE_ENVIRONMENT is set to Development.

![image](https://cloud.githubusercontent.com/assets/8246794/25904483/322f06d0-3554-11e7-980e-2217c8339a03.png)

OR

![image](https://cloud.githubusercontent.com/assets/8246794/25904493/3967f4a2-3554-11e7-938d-00a9545238ff.png)

* #### Workaround:
Modify the code in Program.cs and try again


_Change_: 
```
     public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
```

_To_: 
```
     public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .UseDefaultServiceProvider(options =>
                    options.ValidateScopes=false)
                .Build();
```


### Add-Migration command fails with Exception calling "Substring"

* #### Issue: 
In an ASP.NET Core 2.0 project, when trying to use the NuGet Package Manager Console to add migrations using Add-Migration cmdlet or Update-Database, you get the following error : 
```
Add-Migration : Exception calling "Substring" with "1" argument(s): "StartIndex cannot be less than zero.
Parameter name: startIndex"
```


![image](https://cloud.githubusercontent.com/assets/8246794/25904679/c8bfd458-3554-11e7-8ba3-76bc79b559db.png)

* #### Workaround:
This error is actually pretty safe to ignore - [see here](https://github.com/aspnet/EntityFramework/issues/8163) for more info. If you look at the Solution Explorer, you can see that the migration actually got added. 
 
Alternatively, you can use "dotnet ef migrations add" to add the migration instead of the PowerShell cmdlet

### After re-targeting ASP.NET Core 1.1 application to 2.0, you get package downgrade warnings related to BrowserLink

* #### Issue: 
When you re-target your existing ASP.NET Core 1.1 application to ASP.NET Core 2.0, you might run into the many package downgrade warnings of the form. When you F5, the page fails to load with a HTTP Error 502.3 - Bad Gateway. 
```
Detected package downgrade: System.Net.Sockets from 4.3.0 to 4.1.0 WebApp_Anc11_core_NoAuth_1 (>= 1.0.0) -> Microsoft.VisualStudio.Web.BrowserLink (>= 1.1.0) -> Microsoft.AspNetCore.Hosting.Abstractions (>= 1.1.0) -> NETStandard.Library (>= 1.6.1) -> System.Net.Sockets (>= 4.3.0) WebApp_Anc11_core_NoAuth_1 (>= 1.0.0) -> Microsoft.VisualStudio.Web.BrowserLink (>= 1.1.0) -> System.Net.Sockets (>= 4.1.0)
```

* #### Workaround:
Comment out or remove references to BrowserLink from your csproj file and from Startup.cs. BrowserLink is not supported in ASP.NET Core 2.0 yet. 

## ASP.NET Core 1 and Web Tools Known Issues

### After installing update to Visual Studio and publishing a ASP.NET Core 1.1 application, you get a HTTP Error 502.5 on the published application

* #### Issue: 
Visual Studio 2017 (version 15.3) Preview installs the .NET Core 1.1.2 runtime. When you build and publish ASP.NET Core 1.1 application using this version of Visual Studio, the application is built against the 1.1.2 runtime. If the web server does not have the 1.1.2 runtime, then you get HTTP Error 502.5

* #### Workaround:
Install the 1.1.2 runtime on the web server

### After opening ASP.NET Core 1.1 solution, you see warnings in Dependencies node in Solution Explorer

* #### Issue: 
When you open a pre-built ASP.NET Core 1.1 application after installing the latest update to Visual Studio, you might run into the following issue where you see warnings in Dependencies node in Solution Explorer. The warnings do not go away even after restore completes.

* #### Workaround:
Close and re-open the solution. The warnings should go away.

### Bower packages are not automatically restored after adding full dependencies to an ASP.NET Core 1.1 application created using Empty template

* #### Issue: 
If you try to add full dependencies to an ASP.NET Core 1.1 application created using Empty template, the warning sign doesn't go away from dependencies node for Bower packages

![image](https://cloud.githubusercontent.com/assets/8246794/25910188/6d4339d4-3564-11e7-901d-f85ee0117467.png)

* #### Workaround:
Right-click Bower node and restore packages


