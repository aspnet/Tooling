# Known issues for .NET Core, ASP.NET Core, and ASP.NET and Web Tools in Visual Studio 2017 

If you encounter any issues not already listed below, you can report them using the Report a Problem tool on our [Developer Community](https://developercommunity.visualstudio.com/) site, where you can track the status and see issues reported by others as well. 

If you encounter a problem with Visual Studio, we want to know about it so that we can diagnose and fix it. By using the Report a Problem tool, you can collect detailed information about the problem, and send it to Microsoft with just a few button clicks. See [here](https://docs.microsoft.com/en-us/visualstudio/ide/talk-to-us) for more details. The GitHub Issue Tracker for the aspnet/Tooling repo is now deprecated in favor of the Report a Problem tool. 

## .NET Core Known Issues
For known issues with .NET Core, use the following links to see the issues in the .NET Core GitHub repo the team is tracking including comments and status.

* [Migration from project.json/xproj to csproj](https://github.com/dotnet/cli/issues?q=is%3Aissue+label%3Amigration+is%3Aopen)
* [.NET Core SDK](https://github.com/dotnet/sdk/issues?q=is%3Aissue+label%3ABug+is%3Aopen) & [CLI](https://github.com/dotnet/cli/issues?q=is%3Aissue+label%3Abug+is%3Aopen)
* [MSBuild](https://github.com/Microsoft/msbuild/issues?q=is%3Aissue+label%3Abug+is%3Aopen)
* [IDE/Project System](https://github.com/dotnet/roslyn-project-system/issues?q=is%3Aissue+label%3ABug+is%3Aopen)
* [NuGet](https://github.com/NuGet/Home/issues?q=is%3Aissue+label%3AType%3ABug+is%3Aopen)

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

## ASP.NET Core Known Issues

### Tag Helpers do not work

* #### Issue: 
Razor Tag Helpers do not get colorization or special IntelliSense at design time.  They work normally at runtime. 

* #### Workaround: 
Install the [Razor Language Service extension](https://aka.ms/razorlangsvc).

### No suggestions to install missing packages

* #### Issue: 
Pressing Ctrl+. on unresolved references to extension methods does not provide a lightbulb with helpful shortcuts to install the required package 

* #### Workaround: 
Manually find and install the package using Manage NuGet packages UI

### NuGet Recommends Upgrading Packages in 1.0 app to 1.1 versions

* #### Issue:
If your application is targeting ASP.NET Core 1.0 and you try to add NuGet packages using Manage NuGet Packages dialog or the Package Manager Console tool window, it defaults to installing the 1.1 version of the packages causing undesirable behavior in some cases.

* #### Workaround:
Update the version of the packages to 1.0.x

### .NET Core project containing node_modules cause VS to hang

* #### Issue: Visual Studio becomes unresponsive when working on .NET Core projects that contain node_modules.

* #### Workaround: Do npm restore while the project or VS is closed, and then reopen the solution/project.


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
