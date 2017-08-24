
When creating projects from the command line using `dotnet new`, you may run into the following error.

```
dotnet new web -f netcoreapp1.1
Error: Invalid parameter(s):
-f netcoreapp1.1
    'netcoreapp1.1' is not a valid value for -f (Framework).
Run dotnet new web --help for usage information.
See https://aka.ms/dotnet-install-templates to learn how to install additional template packs.
```

This occurs when you attempt to create a template for a framework, and the templates for that framework are not installed. For example if you install .NET Core 2.0, and try to create a `1.1` project. To resolve this you can install
the 1.1 templates with one, or more, of the commands below.

### To install 1.1 web templates

```
dotnet new -i Microsoft.DotNet.Web.ProjectTemplates.1.x::1.0.0-*
```

### To install 1.1 class library and console templates

```
dotnet new -i Microsoft.DotNet.Common.ProjectTemplates.1.x::1.0.0-*
```

### To install 1.1 unit test templates

```
dotnet new -i Microsoft.DotNet.Test.ProjectTemplates.1.x::1.0.0-*
```

After installing the desired templates, you should be able to re-run the command to create the project.

For general help see the [dotnet new docs](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-new)