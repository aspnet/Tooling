# ASPNet-VM
Scripts and Templates for setting up a Windows 2016 Server VM that hosts ASP.NET 4.6 web applications on IIS and has Web Deploy 3.6 installed and configured.

## Create new VM in Azure

Click the following **Create Azure VM** button to use <a href="https://github.com/aspnet/Tooling/blob/AspNetVMs/VMSetup/ASPNet-ARMTemplate.json" target="_blank">this custom Azure Resource Manager template</a> to provision a new VM in Azure that's set up for hosting ASP.NET web apps and publishing from Visual Studio.<br>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Faspnet%2FTooling%2FAspNetVMs%2FVMSetup%2FASPNet-ARMTemplate.json" target="_blank">![Create ASP.NET VM in Azure][Create Azure VM button]</a>

The template performs the following actions:

* Provision a new Azure VM (Windows Server 2016 Datacenter)
* Configure components and features on the VM (available as [PowerShell script][PowerShellScript])
* Configure Azure Firewall rules (Ports 80 and 8172)

> Important:
> You need to manually configure a DNS name for the VM in order to use the **Microsoft Azure Virtual Machines** publishing wizard in Visual Studio.


## Configure an existing VM

- Run [`setup.ps1`][PowerShellScript] in an elevated prompt 
 
[Create Azure VM button]: ../media/create-asp-net-vm-with-webdeploy/CreateAzureVM.png
[PowerShellScript]: https://github.com/aspnet/Tooling/blob/AspNetVMs/VMSetup/setup.ps1
