
## Release notes for .NET Core 1.0.1 – VS 2015 Tooling Preview 2

This release contains the following.

### RTM tooling for ASP.NET (ASP.NET, not including ASP.NET Core)
This release contains RTM quality support for ASP.NET.

### Preview 2 tooling for .NET Core and ASP.NET Core

This release contains Preview 2 tooling for .NET Core and ASP.NET Core.

### Runtime packages updated to 1.0.1

In the previous release the .NET Core packages used version `1.0.0`. To update your existing projects you can update package references from `1.0.0` to `1.0.1`.

### Updated ASP.NET Core templates to ensure Views in Areas are published

In the previous release if you added an Area to an ASP.NET Core project when the project is published the views under the areas may not be published. This is caused by an incomplete entry in the `project.json` file. The fix for this is to update the `project.json` file. See https://github.com/aspnet/Templates/issues/654 for more details. For new projects created with Preview 2, `project.json` contains the complete entry.

## Release notes for .NET Core 1.0.0 - VS 2015 Tooling Preview 2

You can read more about this release at https://blogs.msdn.microsoft.com/webdev/2016/06/27/announcing-asp-net-core-1-0/

This release contains the following

### Web tooling updates for ASP.NET as well as Preview 2 tooling for Microsoft .NET Core 1.0.0

This release contains the latest tools to build great ASP.NET applications. It also contains Preview 2 tooling for Microsoft .NET Core 1.0.0.

### Template updates

 - **Default auth switched to No Auth**: In previous versions Individual Auth was the default authentication option. We have changed this to No Auth to simplify the default project contents.
 - **BundlerMinifier included**: In previous versions we were relying on gulp to bundle and minify client side content. We have changed this to use BundlerMinifier to simplify the default project contents and decrease the dependencies.

### FTP Publish support included

In this release we have added support to publish to FTP endpoints using the Visual Studio Publish Web dialog.

### Cumulative updates

Since our initial release on 6/27/2016, we have released cumulative updates as listed below.

#### [7/20/2016] Cumulative servicing update for Visual Studio 2015 Update 3

If you obtain [cumulative servicing update](https://msdn.microsoft.com/en-us/library/mt752379.aspx) that provides fixes to Visual Studio 2015 Update 3, you will benefit from the resolution of the following two issues:
* In ASP.NET cshtml files, committing code completion after you type the full snippet may cause incorrect snippets to be added to the source file.
* An issue that affects inspecting local variables for code that's embedded in cshtml files occurs in ASP.NET Core applications. 

#### [8/11/2016] Tooling update for .NET Core 1.0.0 - VS 2015 Tooling Preview 2

We fixed a few key customer issues as listed below to improve the development experience of .NET Core. This update can be downloaded and installed from [here](https://go.microsoft.com/fwlink/?LinkId=817245) and is a cumulative update that contains Preview 2 tooling plus targeted bug fixes listed below. 
 
Please note that this is only an update to the Visual Studio tooling - runtime pieces of .NET Core as well as components of .NET Core 1.0.0 SDK are the same as what they were before.

Fixes in this update include the following:
* Some customers were blocked from installing the Preview 2 installer with a message that Visual Studio 2015 Update 3 may not be completely installed. To work-around it, one had to run the installer from the command-line with the SKIP_VSU_CHECK=1 argument. This workaround is not needed anymore, and this updated Preview 2 installer will now install correctly on top of Visual Studio 2015 Update 3. 
* Fix for a bug that sometimes caused crashes in Visual Studio 2015 Update 3 (`CLR_EXCEPTION_System.NullReferenceException_80004003_Microsoft.VisualStudio.Composition.dll!Microsoft.VisualStudio.Composition.ExportProvider+PartLifecycleTracker.ReportPartiallyInitializedImport`)
* Fix for a [bug](https://github.com/aspnet/Tooling/issues/647) that sometimes caused .xproj to be repeatedly built with the message `(project) will be compiled because inputs were modified`.

## Release notes for .NET Core 1.0.0 RC2 – VS 2015 Tooling Preview 1

You can read more about this release at https://blogs.msdn.microsoft.com/webdev/2016/05/16/announcing-asp-net-core-rc2

This release contains the following.

### RTM tooling for ASP.NET (ASP.NET, not including ASP.NET Core)

This release contains RTM quality support for ASP.NET.

### Preview 1 tooling for .NET Core and ASP.NET Core

This release contains Preview 1 tooling for .NET Core and ASP.NET Core. We have also fixed a number of issues from previous releases.

### Support for running EF Migrations on Publish

We have added support on the Web Publish dialog to execute Entity Framework Migrations during a publish using Web Deploy (aka MSDeploy).

### New Project updates

The templates provided in the File->New Project dialog have been updated. We now have three template entries in the New Project Dialog related to ASP.NET.

 - **ASP.NET Web Application (.NET Framework)**: This opens the One ASP.NET dialog with templates for ASP.NET. Including Web Forms, MVC, Web API, etc. These templates use `System.Web` and only run on Windows.
 - **ASP.NET Core Web Application (.NET Core)**: This opens the One ASP.NET dialog with templates for ASP.NET Core running on .NET Core. Since these are targetting .NET Core they can be used on all platforms (Windows, Linux and OS X). 
 - **ASP.NET Core Web Application (.NET Framework)**: This opens the One ASP.NET dialog with templates for ASP.NET Core running on .NET Framework. Since they are running on the .NET Framework, these templates will only run on Windows. You should use this if you have investments in libraries, or code, which are not compatible with .NET Core.

### Web linters

With this release we are including web linters including: ESLint, CSSLint, TSLint and CoffeeLint.

### Dependency updates

We have refreshed many of the packages used in the default templates provided by File -> New Project.

In addition we have updated to node version 5.4.1 and npm version 3.3.4.

### Updated support for xproj projects referencing csproj projects

We have updated the support for .xproj projects referencing .csproj projects. In the past we relied on a command line tool `dnu wrap`. We ran into many issues with that support and decided to re-implement it without using that command line tool.

### Links

For more info see https://blogs.msdn.microsoft.com/webdev/2016/05/16/announcing-asp-net-core-rc2/. If you have any problems using the release please file an issue at https://github.com/aspnet/tooling/issues.

For known issues see https://github.com/aspnet/Tooling/blob/master/known-issues.md.
