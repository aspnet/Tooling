# Known issues for .NET Core 2.0, ASP.NET Core 2.0, and ASP.NET and Web Tools in Visual Studio 2017 "Preview"

This page is specifically for issues with the .NET Core and ASP.NET Core 2.0 Previews, and the Visual Studio 2017 Preview.  For issues with Visual Studio 2017, or .NET Core/ASP.NET Core 1.x see our [Visual Studio 2017 Known Issues page](known-issues-vs2017.md). If you encounter any issues not already captured, you can report them using the Report a Problem tool on our [Developer Community](https://developercommunity.visualstudio.com/) site, where you can track the status and see issues reported by others as well. 

## .NET Core Known Issues

### Dependencies node in solution explorer shows with a warning icon until restore is complete

* #### Issue: 
When you create or open an existing .NET Core or ASP.NET Core project using Visual Studio 2017, the dependencies node shows warnings until all packages are restored and intellisense is ready. See [this issue](https://github.com/dotnet/project-system/issues/2079) for details.

![image](https://cloud.githubusercontent.com/assets/8246794/25904231/78210626-3553-11e7-8259-02d4239fb79c.png)

* #### Workaround:
Simply wait for package restore to complete and the warnings will go away

## ASP.NET and Web Tools Known Issues

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
