# Known issues for ASP.NET 5 support in Visual Studio 2015

#### applicationHost.config file appears as an untracked file in Solution

The applicationHost.config file will sometimes appear as an untracked file in Solution Explorer. This file should not be checked in as it has user specific paths which will not work in team environments.

#### Removing dnx451 from project.json frameworks may lead to erroneous build errors for ASP.NET 5 Web Sites projects

If you remove the dnx451 value from the frameworks section in project.json for ASP.NET 5 Web Site projects you may receive an error similar to "The type or namespace name 'AspNet' does not exist in the namespace 'Microsoft' (are you missing an assembly reference?)". To work around this you can remove the file at Compiler\Preprocess\RazorPreCompilation.cs.

#### Unable to add a reference to a standard C# project (.csproj) from an ASP.NE T 5 project which is missing global.json

If you are working in a solution with ASP.NET 5 projects that does not have a global.json file you may not be able to add a reference to a standard C# project (.csproj) from an ASP.NET 5 project.

#### Installing dnvm x64 CoreClr without installing the corresponding x86 version may cause errors

If you install an x64 CoreClr version of dnvm and use that as the default then you may get error message like "Cannot find the DNX runtime '<version>' … ". To work around this install the corresponding x86 version as well.

#### Publishing using Web Express may incorrectly report a failed status

When publishing using Visual Studio 2015 Web Express you may receive an error like "The InstallDir property does not exist in the path HKEY_LOCAL_MACHINE\SOFTWARE\ Wow6432Node\Microsoft\VisualStudio\14.0." Even though this error is reported in most cases the site is actually published.

#### IntelliSense for version in bower.json may not work on localized builds

If you are using a localized version of Visual Studio 2015 IntelliSense for version of bower packages may not work in bower.json.
