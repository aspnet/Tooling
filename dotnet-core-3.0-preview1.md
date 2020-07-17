# .NET Core 3.0 Preview 1

Visual Studio 2019 is required to create or open apps targeting .NET Core 3.0. [Download Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/).

For offline installation, see [how to install offline](https://docs.microsoft.com/visualstudio/install/create-an-offline-installation-of-visual-studio).

### Known issues in Visual Studio 2019

When creating a new project Visual Studio may display a yellow bar with the following message:

**ASP.NET Core 3.0 or newer projects are not supported by this version of Visual Studio**

**Workaround:**

* Select **Cancel**.
* Restart Visual Studio.

On restart:

* The preceding error message will not be displayed.
* **ASP.NET Core 3.0** is available in the dropdown.

Selecting **Cancel** and reopening the dialog without restarting Visual Studio will not fix the issue.
