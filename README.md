Visual Studio Tooling for ASP.NET
=================

Please use the [issue tracker](https://github.com/aspnet/Tooling/issues) to log bugs and participate in discussions related to the ASP.NET Core tooling in Visual Studio. Please be sure to include details on the specific version of Visual Studio that you are using.

The latest released tooling can be downloaded at https://dot.net.

This project is part of ASP.NET Core. You can find samples, documentation and getting started instructions for ASP.NET Core at the [Home](https://github.com/aspnet/home) repo.

# Pre-release builds

If you want to try the latest tooling builds available and provide feedback to the team then you can download them below.

These pre-release builds are internal nightly builds that have had some testing done, but the testing is not as extensive as a released version. There are likely to be bugs, sometimes serious ones, and due to the nature of tooling some of those bugs could force you to rebuild the impacted machine or re-install Visual Studio.


| Date       | Link        | Version | Discussion |
|------------|-------------|---------|------------|
| Nightly - 6/17/16 | [DotNetCore.1.0.0.RC3-34-VS2015Tools.Preview2.exe](https://download.microsoft.com/download/B/D/1/BD13D9A4-71FC-4C9B-B67B-38540773B978/DotNetCore.1.0.0.RC3-34-VS2015Tools.Preview2.exe) | 1.0.20617.34   | [Discussion for Nightly - 6/17/16] (https://github.com/aspnet/Tooling/issues/599) |
| Nightly - 6/7/16  | [DotNetCore.1.0.0.RC3-27-VS2015Tools.Preview2.exe](https://download.microsoft.com/download/A/B/1/AB1D84A0-3E98-43E9-A73E-E55D963162B3/DotNetCore.1.0.0.RC3-27-VS2015Tools.Preview2.exe) | 1.0.20607.27   | [Discussion for Nightly - 6/7/16] (https://github.com/aspnet/Tooling/issues/577) |

## Installation Instructions

  - Install [Visual Studio 2015 Update 3 RC](https://blogs.msdn.microsoft.com/visualstudio/2016/06/07/visual-studio-2015-update-3-rc/) (Current nightly builds require Update 3 RC to be installed)
  - [Apply workaround](https://go.microsoft.com/fwlink/?LinkId=808095) for setup issue in VS 2015 U3 RC. There is a known issue with Visual Studio 2015 Update 3 RC, as a result of which, .NET Core Tooling may fail to install with the following message after Visual Studio 2015 Update 3 RC is installed:
  
  `0x80070666 - Another version of this product is already installed. Installation of this version cannot continue. To configure or remove the existing version of the product, use Add/Remove Programs on the Control Panel.`
  - Install the build downloaded from this page

## Release Notes and Known Issues

- Nightly - 6/17/16
  - **Notes**
    - Includes a new pre-release .NET Core SDK (version 1.0.0-preview2-003041) and .NET Core (version 1.0.0-rc3-004459-00)
    - Default authentication for web templates has been changed to **No Authentication**.
    - The ASP.NET Core Web Application template (Individual Accounts) uses BundlerMinifier.Core instead of gulp to do bundling and minification. NPM and gulp tooling is still supported though, it is just not included in the template by default anymore. 
  - **Issues**
    - The exe name, title in installer, Add Remove Programs (ARP), and package versions in project.json all say RC3. It is labelled as RC3 because the package versions automatically rolled forward to RC3 when we shipped RC2. It is not meant to imply that there will be another RC release. 
    - Publishing new web application to Azure that targets the new runtime installed with the tooling bundle will fail by default. This is because the nightly runtime that comes with this build is not pre-installed on Azure.
    - Due to a known issue in the setup detection logic, even though you have Visual Studio 2015 Update 3 RC installed, you might see the following message when you run the installer. This will be fixed in the next build. For now, to work around this and bypass the check, you can run the EXE from a command prompt and then pass SKIP_VSU_CHECK=1 at the end.
  ![image](https://cloud.githubusercontent.com/assets/8246794/16207980/dd636272-36e4-11e6-9d5a-295027a5ac5c.png)

- Nightly - 6/7/16
  - **Notes**
    - Includes a new pre-release .NET Core SDK (version 1.0.0-preview2-002913) and .NET Core (version 1.0.0-rc3-004338)
    - Default authentication for web templates has been changed to **No Authentication**.
  - **Issues**
    - The exe name, title in installer, Add Remove Programs (ARP), and package versions in project.json all say RC3. It is labelled as RC3 because the package versions automatically rolled forward to RC3 when we shipped RC2. It is not meant to imply that there will be another RC release. 
    - The .NET Core SDK that is installed as part of this bundle is still named “.NET Core 1.0.0 RC2 – SDK Preview 1” in ARP, even though it is a pre-release of Preview2 tooling. Below is how ARP looks after installing the above build on top of RC2 Tooling. The SDK should be Preview2 but is still called “.NET Core 1.0.0 RC2 – SDK Preview 1 (x64)” – the one with version number “1.0.0.2913” is actually the Preview 2 SDK. It will appear with the correct versions on disk.
![image](https://cloud.githubusercontent.com/assets/8246794/15905301/3d81b7ce-2d69-11e6-8edd-0c3edb485492.png) 
    - Publishing new web application to Azure that targets the new runtime installed with the tooling bundle will fail by default. This is because the nightly runtime that comes with this build is not pre-installed on Azure.
    - After creating a new web application with 'Add Application Insights checkbox' checked during project creation, you will see the following error when running the page. We don't have compatible Application Insights packages in these tooling builds yet.
      - _Error Message:_
      ```
System.TypeLoadException
Could not load type 'Microsoft.Extensions.DependencyInjection.ServiceCollectionExtensions' from assembly 'Microsoft.Extensions.DependencyInjection.Abstractions, Version=1.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
```
      - _Workaround_: Uncheck 'Add Application Insights' checkbox during project creation.
