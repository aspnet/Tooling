# .NET Core 3.0 Preview 1

To create or open applications targeting .NET Core 3.0, Visual Studio 2019 or newer is recommended. Click the following link to download the latest version of Visual Studio 2019. If you are after an offline installation follow these instructions on [how to install offline](https://docs.microsoft.com/visualstudio/install/create-an-offline-installation-of-visual-studio).

* [Download Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/)

Visual Studio 2017 15.9 supports creating and opening applications targeting .NET Core 3.0, but it contains known issues so Visual Studio 2019 or newer is recommended.

## Known issues in Visual Studio 2017

When creating a new project Visual Studio may show you a yellow bar with the message 
"**ASP.NET Core 3.0 or newer projects are not supported by this version of Visual Studio**"

**Workaround:** You need to hit Cancel in OneAspNet dialog and then restart VS for the yellow-bar message to go away and ASP.NET Core 3.0 to show up in the dropdown. Simply cancel and re-open OneAspNet dialog without restarting VS does not fix the issue.
