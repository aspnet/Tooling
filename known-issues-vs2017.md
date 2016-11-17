﻿# Known issues for ASP.NET Core support in Visual Studio 2017

## Visual Studio 2017 RC ASP.NET Core Known Issues

### Visual Studio 2015 fails to restore NuGet packages after install Visual Studio 2017
After installing Visual Studio 2017 RC on the same machine as Visual Studio 2015 with .NET Core tooling, restoring NuGet packages in an ASP.NET Core project using Visual Studio 2015 fails

* #### Issue: 
On a machine that already has VS 2015 with .NET Core Tooling, if you install VS 2017 RC and then try to restore packages for an ASP.NET project using VS 2015, it might fail with the following error message: 

  MSBUILD : error MSB4025: The project file could not be loaded. Data at the root level is invalid. Line 1, position 1 

* #### Workaround: 
Install the latest update for [.NET Core VS 2015 tooling](https://www.microsoft.com/net/core#windowsvs2015) to resolve the above error. After installing this update, you will be able to restore NuGet packages again using VS 2015.

### Bower packages fail to restore
Bower Restore fails to restore packages 
 
* #### Issue: 
Bower Restore does not function, and IntelliSense for version numbers in bower.json always returns "*".  Attempt to restore displays the following in the Output Window (Bower/npm pane): 

  ECMDERR Failed to execute "git ls-remote --tags --heads https://github.com/lodash/lodash.git", exit code of #128 

* #### Workaround: 
    1. Select Tools/Options 
    2. In the Options Dialog in the left-hand pane, select Projects and Solutions / External Web Tools 
    3. Add the following as new path to the "Locations of external tools" box: 
$(DevEnvDir)\CommonExtensions\Microsoft\TeamFoundation\Team Explorer\Git\mingw32\bin 
    4. Click “OK” 
    5. Restart Visual Studio 2017 RC

### Long time before all files are visible in Solution Explorer
After creating a .NET Core or ASP.NET Core project, you need to wait until package restore completes 

* #### Issue: 
As soon as creating a .NET Core or ASP.NET Core project, you will see a message in the status bar that it is restoring some packages. While this package restore is in progress, you will not be able to view all the files in Solution Explorer or build the project. If you tried to build a project while package restore is in progress, you might also see a dialog with the message "The build must be stopped before the project can be closed".  

* #### Workaround: 
The project is fully ready for interaction only after package restore is complete. We are working on improving this experience in future releases.

### Docker enabled project fails to build
Project fails to build if you enable Docker Support without 'Docker for Windows' installed 

* #### Issue:  
When creating an ASP.NET Core project, if you check the "Enable Container (Docker) Support" checkbox but do not have "Docker for Windows" installed, the created project will fail to build with the following error: 

  Microsoft.DotNet.Docker.CommandLineClientException: 'docker-compose' was not found. Please verify that it is available on %PATH%, or for troubleshooting, follow instructions from http://aka.ms/DockerToolsTroubleshooting 

* #### Workaround: 
Install [Docker for Windows](https://docs.docker.com/docker-for-windows/) to resolve the issue

### Error when opening ASP.NET Core project
Error dialog with message : The project system has encountered an error. Could not resolve mscorlib for target framework '.NETCoreApp,Version=v1.0' 

* #### Issue: 
When you create or re-open an ASP.NET Core project, you might sometimes see this error dialog: 

  The project system has encountered an error. Could not resolve mscorlib for target framework '.NETCoreApp,Version=v1.0' 

* #### Workaround: 
Restart Visual Studio and try again

### No suggestions to install missing packages
Ctrl+. Light Bulbs does not work with extension methods in .NET Core or ASP.NET Core projects 

* #### Issue: 
Pressing Ctrl+. on unresolved references to extension methods does not provide a lightbulb with helpful shortcuts to install the required package 

* #### Workaround: 
Manually find and install the package using Manage NuGet packages UI

### Tag Helpers do not work
Razor Tag Helpers do not work in .NET Core projects 

* #### Issue: 
Razor Tag Helpers do not get colorization or special IntelliSense at design time in RC.  They work normally at runtime. 

* #### Workaround: 
None available

### TypeScript files do not compile
TypeScript files not compiled in .NET Core projects 

* #### Issue: 
TypeScript files are not automatically compiled on save in .NET Core projects 

* #### Workaround: 
Add a tsconfig.json file to the root of the project, containing at least {}.

### Entity Framework commands to not work
Entity Framework Commands do not work in Package Manager Console 

* #### Issue: 
Entity Framework powershell command integration with msbuild is not yet complete, and Microsoft.EntityFrameworkCore.Design is omitted from ASP.NET Core Web Application with Individual Authentication project template. 

* #### Workaround: 
Add Microsoft.EntityFrameworkCore.Design NuGet package to the project (assuming a ASP.NET Core Web Application with Individual Authentication) 

  Open an ordinary command prompt at the project level and use dotnet commands for Entity Framework.

### Scaffolding broken
Scaffolding broken for ASP.NET Core (.NET Framework) templates 

* #### Issue: 
[CLI is not generating the assembly redirects for project dependency tools](https://github.com/dotnet/cli/issues/4666) 

* #### Workaround: 
None

### Sequence numbers added to wrong item templates
Sequence numbers are added to inappropriate Item Templates 

* #### Issue: 
In Core projects, item templates such as "npm Configuration File", "Bower Configuration File", "Grunt/Gulp Configuration File", etc., which work only with specific names, are created with an extra "1" inserted before the end of the file name. 

* #### Workaround: 
Edit the supplied file name before or after creating the file. 

### Error running app with Windows Authentication
Error running Core Apps using Windows Authentication 

* #### Issue: 
There is an error in the project template processing code, and $webserverport1$ is not replaced with the randomly generated port number. 

* #### Workaround: 
Replace $webserverport1$ in Properties\launchSettings.json with a port number, such as 8183.

### Bundle and Minify not working
Bundle and Minify is not operating in Core projects created from VS project templates

* #### Issue:
Bundle and Minify commands are commented out in csproj files created from project templates.

* #### Workaround:
  * Create a Core web application project
  * Right-click on the project node in Solution Explorer and select "Exid [projectname].csproj
  * Ignore the warning about line enndings.  (Known issue.)
  *  At the botom of the csproj file, find:
  ```xml
   <Target Name="BeforePublish">
    <Exec Command="bower install" />
    <Exec Command="dotnet bundle" />
  </Target>
```
Replace with:
```xml
   <Target Name="BeforePublish">
    <!--
    <Exec Command="bower install" />
    -->
    <Exec Command="dotnet bundle" />
  </Target>
  ```
(The bower install command is not ready for RC and will be fixed in an upcoming release.)
To invoke bundling and minification, publish the project.  For example, publish to file system.

### Migrations not applied during publish
Cannot apply migrations during publish of ASP.NET Core project

* #### Issue:
Ability to provide a destination connection string and apply migrations are not available in the Publish Settings for an ASP.NET Core project

* #### Workaround:
You need to manually apply migrations on the destination database server

### Unable to publish
Unable to publish ASP.NET Core Web Application (.NET Framework)

* #### Issue:
If you try to publish an ASP.NET Core Web Application (.NET Framework), you will run into the following error: "DestinationFiles" refers to 1 item(s), and "SourceFiles" refers to 2 item(s). They must have the same number of items

* #### Workaround:
None available

### Publish crashes
Publish crashes on locales that do not use '.' as a decimal separator

* #### Issue:
A bug in publishing fails to distinguish decimal separators in a language-neutral way.

* #### Workaround:
Before publishing, set VS locale to ENU.

### Cannot configure web server settings
Property pages of ASP.NET Core projects do not allow you to configure Web Server Settings

* #### Issue:
Web Server settings such as App URL, ability to enable SSL, Windows Authentication are not available in the property pages of an ASP.NET Core project

* #### Workaround:
You can edit the launchSettings.json file and apply settings similar to .NET Core tooling in Visual Studio 2015

### Razor IntelliSense not working
Razor IntelliSense Completion issues in .NET Core projects

* #### Issue:
Completion at the end of razor expressions doesn't work well in RC. eg, typing "@DateTi." or "@DateTime." (w/o the quotes) will not commit properly, and may mark the dot as markup.  Please report any additional IntelliSense Issues using "Send Feedback."

* #### Workaround:
Backspace one character and retry

### Scaffolded files not included in project
Scaffolded files may not be included in user's project in certain cases, where user's project defines exclusions.

* #### Issue:  
If user project has a globbing pattern for ```<Compile>``` item group with a exclude pattern. And if the files generated by scaffolding match the pattern, they are not included in the project. For example: If project has ```<Compile Include = "**/*.cs" Exclude="ExcludedDir/*.cs" />``` and scaffolding generates a file in 'DefaultController.cs' ExcludedDir, it will not be included in the project for compilation. 

* #### Workaround: 
Manually adjust the globbing pattern to include the 'ExcludedDir/DefaultController
