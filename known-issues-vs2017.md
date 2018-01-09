# Known issues for .NET Core, ASP.NET Core, and ASP.NET and Web Tools in Visual Studio 2017 

If you encounter any issues with VS 2017 not already listed below, you can report them using the Report a Problem tool on our [Developer Community](https://developercommunity.visualstudio.com/) site, where you can track the status and see issues reported by others as well. 

If you encounter a problem with Visual Studio, we want to know about it so that we can diagnose and fix it. By using the Report a Problem tool, you can collect detailed information about the problem, and send it to Microsoft with just a few button clicks. See [Visual Studio’s Talk to Us page](https://docs.microsoft.com/en-us/visualstudio/ide/talk-to-us) for more details. The GitHub Issue Tracker for the aspnet/Tooling repo is now deprecated in favor of the Report a Problem tool.

## .NET Core Known Issues
For known issues with .NET Core, use the following links to see the issues in the .NET Core GitHub repo the team is tracking including comments and status.

* [Migration from project.json/xproj to csproj](https://github.com/dotnet/cli/issues?q=is%3Aissue+label%3Amigration+is%3Aopen)
* [.NET Core SDK](https://github.com/dotnet/sdk/issues?q=is%3Aissue+label%3ABug+is%3Aopen) & [CLI](https://github.com/dotnet/cli/issues?q=is%3Aissue+label%3Abug+is%3Aopen)
* [MSBuild](https://github.com/Microsoft/msbuild/issues?q=is%3Aissue+label%3Abug+is%3Aopen)
* [IDE/Project System](https://github.com/dotnet/roslyn-project-system/issues?q=is%3Aissue+label%3ABug+is%3Aopen)
* [NuGet](https://github.com/NuGet/Home/issues?q=is%3Aissue+label%3AType%3ABug+is%3Aopen)

### Publishing a project with a reference can sometimes result in the referenced project getting published with the wrong configuration

* #### Issue: 
For a .NET Core Console project with a project reference to a Core / NET Standard Class Lib, if we publish the console app in release configuration, the class lib assembly will be published with Debug configuration instead. The console assembly will be in Release configuration as expected.

* #### Workaround:

### Publish .NET Core Console applications / class libraries

* #### Issue: 
If the TargetFramework is set to net45x and RuntimeIdentifiers is set to win7-x64, the published output produces an executable with the wrong bitness.

* #### Workaround:

### 'Target Runtime' dropdown shows "Portable"

* #### Issue: 
If the RuntimeIdentifier element is set in the project file, the "Target Runtime" dropdown in the window that appears when the "Settings" link under "Summary" will not pick up that RuntimeIdentifier and will instead show "Portable".

* #### Workaround:
Set the "RuntimeIdentifiers" element in the project file so the dropdown can pick up the Runtime Identifiers.

### Core Console/Class library  publish dialog not showing the right RID's

* #### Issue: 
Core Console/Class lib Publish Profile Settings Dialog shows only Portable RID when the user wants to add a RID by adding a RuntimeIdentifier element in the project file 

* #### Workaround:
Only add one runtime identifier to the project, by changig the RuntimeIdentifier element, to a RuntimeIdentifiers element, for example:

```XML
<RuntimeIdentifiers>win7-x86</RuntimeIdentifiers>
```

## ASP.NET and Web Tools Known Issues

### When trying to publish to Azure, unable to create App Service for a project with _ in the project name

* #### Issue:
When trying to publish a project with _ in the project name to Azure, the App Name text box of the Create App Service dialog can give a validation error: "Could not verify name availability, please try again"

![image](https://user-images.githubusercontent.com/8246794/29236912-f6e4a39c-7ec7-11e7-9321-6a124fd6dcdd.png)

* #### Workaround:
Remove _ from the App Name text box

### MVC4 projects do not connect to SQL Server LocalDB at runtime

* #### Issue:
When running an MVC4 project in Visual Studio, database access by the application may fail if it is using SQL Server Express LocalDB 2012. This is caused because MVC4 projects by default depend on SQL Server Express LocalDB 2012 which is not installed with Visual Studio 2017.

* #### Workaround:
Upgrade the project to use SQL Server Express LocalDB 2016, or manually download and install [SQL Server Express LocalDB 2012](https://go.microsoft.com/fwlink/?LinkID=627336) on the machine.

### Remote profiling to Azure App Service does not work

* #### Issue:
Remote profiling to Azure App Service from Cloud Explorer or Server Explorer displays the error "Cannot access a disposed error".

* #### Workaround:
Use Visual Studio 2015 to profile, Azure App Services does not yet support profiling from Visual Studio 2017.

### To create ASP.NET Core 1.0 / 1.1 projects install Visual Studio's ".NET Core 1.0 - 1.1 developer tools for Web" component

* #### Issue:
Creating a new ASP.NET Core Web Application based on ASP.NET Core 1.0 or 1.1 is blocked (the OK button is disabled) and displays the message "To create ASP.NET Core 1.0/1.1 projects you need to install Visual Studio's '.NET Core 1.0 - 1.1 developer tools for Web' compomnent."  

* #### Workaround:
Use Visual Studio Installer to install the missing Visual Studio component ".NET Core 1.0 - 1.1 developer tools for Web".

## ASP.NET Core Known Issues

### Re-targeting ASP.NET Core 1.0 or 1.1 application to ASP.NET Core 2.0 will require additional steps by the user

* #### Issue: 
After you change target of an existing ASP.NET Core 1.0 or 1.1 application to 2.0, you might get incompatibility errors. Here are some symptoms of issues you might see:

Build warning:
```
Package 'Microsoft.Composition 1.0.27' was restored using '.NETFramework,Version=v4.6.1' instead of the project target framework '.NETCoreApp,Version=v2.0'. This package may not be fully compatible with your project.
```

Error when you run the project:
```
An unhandled exception occurred while processing the request.
InvalidOperationException: Can not find compilation library location for package 'Microsoft.NETCore.App'
	Microsoft.Extensions.DependencyModel.CompilationLibrary.ResolveReferencePaths()
```

Build Errors
```
error NU1605: Detected package downgrade: System.Diagnostics.Tools from 4.3.0 to 4.0.1. Reference the package directly from the project to select a different version. 
error NU1605: WebApplication1 (>= 1.0.0) -> Microsoft.VisualStudio.Web.BrowserLink (>= 1.1.2) -> Microsoft.AspNetCore.Hosting.Abstractions (>= 1.1.2) -> NETStandard.Library (>= 1.6.1) -> System.Diagnostics.Tools (>= 4.3.0) 
```

* #### Workaround:
You will need to do additional steps to fully migrate your project to 2.0. See following documentation for additional details:

[Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0](https://docs.microsoft.com/en-us/aspnet/core/migration/1x-to-2x/)

[Migrating Authentication and Identity to ASP.NET Core 2.0](https://docs.microsoft.com/en-us/aspnet/core/migration/1x-to-2x/identity-2x)


### PackageManagerConsole EF Core commands like Add-Migration commands fail when a restore is in progress

* #### Issue: 
When a package restore is in progress, if you try to run one of the EntityFrameworkCore commands like Add-Migration and Update-Database using the Package Manager Console, it may fail with the following error

```
Update-Database : The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was 
included, verify that the path is correct and try again.
```

* #### Workaround:
Wait until package restore completes and try again

### Edit and continue dialog pops up twice when debugging a Razor view

* #### Issue: 
When debugging a Razor view and / or hitting an exception in a Razor view the edit/continue dialog pops up in VisualStudio. If you click edit => f5, you will get the same prompt again.

* #### Workaround:
Hit edit => close the Razor editor => f5.


### When running dotnet aspnet-codegenerator from a command prompt, you run into an unhandled InvalidOperationException

* #### Issue: 
When you try to invoke dotnet aspnet-codegenerator after creating a new ASP.NET Core application using the command-line, you get an unhandled InvalidOperationException that mentions that Microsoft.VisualStudio.Web.CodeGeneration.Design package needs to be added as a NuGet package reference

Example:
```
dotnet new mvc
dotnet aspnet-codegenerator
```	
![image](https://user-images.githubusercontent.com/8246794/29236917-1136782e-7ec8-11e7-9846-de28eba73253.png)

* #### Workaround:
Add the missing package reference in your csproj file, and then run 'dotnet restore'

```
<PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.0.0" />
```

### EntityFramework migrations not available to be applied during publish

* #### Issue: 
Publish settings dialog for ASP.NET core web applications on .NET framework sometimes shows the EF migration section as empty when IIS express is running. This is because the files are locked and EF is not able to detect the dbcontexts for the project.

* #### Workaround:
Exit the Publish dialog, do a VS build and re-open the publish settings dialog. 

### Creating ASP.NET Core Web Application on .NET Framework may not work offline

* #### Issue: 
When creating a ASP.NET Core Web Application on .NET Framework without an internet connection, project creation may fail with the following package restore error due to missing ManagedEsent package

![image](https://user-images.githubusercontent.com/8246794/29236922-2b94bda2-7ec8-11e7-80fc-3db977d1dcb8.png)

* #### Workaround:
Connect to the internet and run Restore Packages on the solution


### After installing update to Visual Studio and publishing a ASP.NET Core 1.1 application, you get a HTTP Error 502.5 on the published application

* #### Issue: 
Visual Studio 2017 (version 15.2) installs the .NET Core 1.1.2 runtime. When you build and publish ASP.NET Core 1.1 application using this version of Visual Studio, the application is built against the 1.1.2 runtime. If the web server does not have the 1.1.2 runtime, then you get HTTP Error 502.5

* #### Workaround:
Install the 1.1.2 runtime on the web server

### Tag Helpers do not work

* #### Issue: 
Razor Tag Helpers do not get colorization or special IntelliSense at design time.  They work normally at runtime. 

* #### Workaround: 
Update VS 2017 to version 15.3

### No suggestions to install missing packages

* #### Issue: 
Pressing Ctrl+. on unresolved references to extension methods does not provide a lightbulb with helpful shortcuts to install the required package 

* #### Workaround: 
Manually find and install the package using Manage NuGet packages UI

### NuGet Recommends Upgrading Packages in 1.0 / 1.1 app to 2.0 versions

* #### Issue:
If your application is targeting ASP.NET Core 1.0 or 1.1 and you try to add NuGet packages using Manage NuGet Packages dialog or the Package Manager Console tool window, it defaults to installing the 2.0 version of the packages causing undesirable behavior in some cases.

* #### Workaround:
Update the version of the packages to 1.0.x / 1.1.x

### .NET Core project containing node_modules cause VS to hang

* #### Issue: 
Visual Studio becomes unresponsive when working on .NET Core projects that contain node_modules.

* #### Workaround: 
Do npm restore while the project or VS is closed, and then reopen the solution/project.


### Publishing project with Entity Framework migration fails

* #### Issue:
Publishing a project with Entity Framework migrations fails if the DB context is not in the project that is getting published.

* #### Workaround:
Add the following to the pubxml or csproj & fill in the DBContextName, ConnectionString, and PathToCsProjContainingDBContext
```xml
  <Target Name="PrePublishScript" BeforeTargets="PrepareForPublish">
    
   <PropertyGroup>
      <IsGenerateEFSQLScriptsDisabled>true</IsGenerateEFSQLScriptsDisabled>
      <SqlFilePath>$(PublishIntermediateTempPath)\GeneratedSqlFile.sql</SqlFilePath>
      <DBContextName></DBContextName>
      <ConnectionString></ConnectionString>
      <PathToCsProjContainingDBContext></PathToCsProjContainingDBContext>
    </PropertyGroup>
    
	<Exec Command="dotnet ef migrations script --idempotent --output &quot;$(SqlFilePath)&quot; --context  $(DBContextName) --project $(PathToCsProjContainingDBContext)” />
    <ItemGroup>
      <_EFSQLScripts Include="$(SqlFilePath)">
        <DBContext>$(DBContextName)</DBContext>
        <ConnectionString>$(ConnectionString)</ConnectionString>
        <EscapedPath>^$([System.Text.RegularExpressions.Regex]::Escape($(SqlFilePath)))$</EscapedPath>
      </_EFSQLScripts>
    </ItemGroup>
  </Target>
```

### Publish settings dialog does not show RuntimeIdentifier dropdown

* #### Issue: 
Publish settings dialog for non-portable ASP.NET Core web apps does not show the RuntimeIdentifier dropdown.

* #### Workaround:
To work around this, set the RuntimeIdentifier property in the csproj.

```xml
   <PropertyGroup>
	     <RuntimeIdentifier></RuntimeIdentifier>
   </PropertyGroup
```

### Package restore sometimes fails when trying to Add Minimal Dependencies

* #### Issue: 
On an empty ASP.NET Core project, MVC Dependency scaffolder sometimes fails with error 'Package restore failed.'

* #### Workaround:
To work around this, click OK on the error dialog and retry scaffolding.

### Scaffolding views cannot be created

* #### Issue: 
In ASP.NET Core projects, 'Delete', 'Details', and 'List' views cannot be scaffolded without a valid DbContext type.

* #### Workaround:
Specify a valid DbContext type.

### Scaffolded files not included in project

* #### Issue:  
If the project has a globbing pattern for ```<Compile>``` item group with a exclude pattern, and the files generated by scaffolding match the pattern, they are not included in the project. For example: If a project has ```<Compile Include = "**/*.cs" Exclude="ExcludedDir/*.cs" />``` and scaffolding generates a file in 'DefaultController.cs' ExcludedDir, it will not be included in the project for compilation. 

* #### Workaround: 
Manually adjust the globbing pattern to include the 'ExcludedDir/DefaultController'

### Ambiguity error during scaffolding controller with Entity Framework

* #### Issue:
If the DataContext class has a member (property, method, variable) defined with the same name as the Model class being used for scaffolding fails with an error as below:<br/>
![amiguity error dialog](./images/vs2017-ambiguity-error.png)

* #### Workaround:
Add a DbSet<> property to the DataContext class for the model which will be used for scaffolding manually and then retry scaffolding
`public DbSet<ClassName> MemberName { get; set; }`

### Project with Docker support does not run in IIS Express

* #### Issue: 
When creating a Docker project and running the project in the "IIS Express" configuration, the website cannot be opened and the status is always loading.

* #### Workaround:
To work around this, add  .UseIISIntegration()  to Program.cs:

```
            var host = new WebHostBuilder()
                .UseKestrel()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .UseApplicationInsights()
                .Build();
```

### Docker enabled project fails to build

* #### Issue:  
When creating an ASP.NET Core project, if you check the "Enable Container (Docker) Support" checkbox but do not have "Docker for Windows" installed, the created project will fail to build with the following error: 

  Microsoft.DotNet.Docker.CommandLineClientException: 'docker-compose' was not found. Please verify that it is available on %PATH%, or for troubleshooting, follow instructions from http://aka.ms/DockerToolsTroubleshooting 

* #### Workaround: 
Install [Docker for Windows](https://docs.docker.com/docker-for-windows/) to resolve the issue

### TypeScript files do not compile

* #### Issue: 
TypeScript files are not automatically compiled on save in .NET Core projects 

* #### Workaround: 
Add a tsconfig.json file to the root of the project, containing at least {}.

### Sequence numbers added to wrong item templates

* #### Issue: 
In Core projects, item templates such as "npm Configuration File", "Bower Configuration File", "Grunt/Gulp Configuration File", etc., which work only with specific names, are created with an extra "1" inserted before the end of the file name. 

* #### Workaround: 
Edit the supplied file name before or after creating the file. 
