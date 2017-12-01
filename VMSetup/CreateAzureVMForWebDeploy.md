# Prepare an Azure VM to enable deployment via WebDeploy
 
This document outlines the steps required to setup an Azure virtual machine that will be able to host ASP.NET (Framework) web applications, and also allow websites to be published using WebDeploy.
To publish to a VM from Visual Studio see Publishing to an Azure VM from Visual Studio.

For a quick and easy way to get up and running with a new Azure VM, use one of the Quick Setup Scripts provided.
- [Setup a new VM](#ARMTemplate)
- [Patch an existing VM](#PowerShellScript)

Otherwise, follow the step-by-step instructions below.
- [Provision a new virtual machine using Azure Portal](#ProvisionNewVM)
- [Connect to the new VM (Remote Desktop Connection)](#ConnectToTheVM)
- [Install IIS (web server) plus Web Management Service and ASP.NET 4.6](#InstallVMFeatures)
- [Install Web Deploy](#InstallWebDeploy)
- [Configure inbound firewall rules in Azure](#ConfigureFirewallRules)
- [Setup DNS name for the VM](#SetupDNSName)

# Summary of Requirements

The minimum requirements for a virtual machine that hosts an ASP.NET web application and allows publishing via Web Deploy are as follows:

Server Components:  
* IIS  
* ASP.NET 4.6  
* Web Management Service  
* Web Deploy  3.6 

Open firewall ports:  
* Port 80 (http)  
* Port 8172 (Web Deploy)  
	
DNS:  
* A DNS name assigned to the VM   

# Quick Setup Scripts

[ASP.NET ARM Template]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjustcla%2FASPNet-VM%2Fmaster%2FASPNet-ARMTemplate.json
[VM Setup Script]: https://github.com/justcla/ASPNet-VM/blob/master/setup.ps1

<a name="ARMTemplate"></a>
## Setup a new VM

Use [this Custom ARM Template][ASP.NET ARM Template] to provision a new VM in Azure that's setup for hosting ASP.NET web apps and publishing from Visual Studio.<br>
[![Create ASP.NET VM in Azure](https://camo.githubusercontent.com/9285dd3998997a0835869065bb15e5d500475034/687474703a2f2f617a7572656465706c6f792e6e65742f6465706c6f79627574746f6e2e706e67)][ASP.NET ARM Template]

The template will perform the following actions:

* Provision a new Azure Virtual Machine (Windows Server 2016 Datacenter)
* Configure components and features on the VM (available as [PowerShell script][VM Setup Script])
* Configure Azure Firewall rules (Ports 80 and 8172)

You just need to provide the following values, and the rest will be generated for you.<br>

Custom template field | How to populate the field
|
Subscription | Choose your Azure subscription |
Location | Accept the default or choose a desired region from the list |
Resource group	| This is the name of the "virtual folder" that will contain all resources created for this VM.<br>You can delete all the resources created during this process by deleting the resource group.<br><br>Consider a name that is similar to the name of the VM you are creating.<br>
**Virtual Machine Name** | **This is the name of the virtual machine you are creating.**
Admin Username | Create an administrator user.<br>*Note: You can use this username when publishing to the VM from Visual Studio. (See article here)*
Admin Password | Create a password for the new Admin account.<br>**Remember this password. You will need it for publishing to the VM.**

***Note:** You will need to manually [configure a DNS name for the VM](http://example.com) in order to use the Microsoft Azure Virtual Machine publishing wizard in Visual Studio.*

<a name="PowerShellScript"></a>
## Patch an existing VM

If you already have an Azure VM, you can download and run [this PowerShell script][VM Setup Script] on the VM to setup the required components.

The script will perform the following tasks:  
* Install IIS (with Management Console)
* Install ASP.NET 4.6
* Install Web Management Service
	- Enable service
	- Configure automatic startup
* Install WebDeploy 3.6

```powershell
# Install IIS (with Management Console)
Install-WindowsFeature -name Web-Server -IncludeManagementTools

# Install ASP.NET 4.6
Install-WindowsFeature Web-Asp-Net45

# Install Web Management Service (enable and start service)
Install-WindowsFeature -Name Web-Mgmt-Service
Set-ItemProperty -Path  HKLM:\SOFTWARE\Microsoft\WebManagement\Server -Name EnableRemoteManagement -Value 1
Set-Service -name WMSVC -StartupType Automatic
if ((Get-Service WMSVC).Status -ne "Running") {
    net start wmsvc
}

# Install Web Deploy 3.6
# Download file from Microsoft Downloads and save to local temp file (%LocalAppData%/Temp/2)
$msiFile = [System.IO.Path]::GetTempFileName() | Rename-Item -NewName { $_ -replace 'tmp$', 'msi' } -PassThru
Invoke-WebRequest -Uri http://download.microsoft.com/download/0/1/D/01DC28EA-638C-4A22-A57B-4CEF97755C6C/WebDeploy_amd64_en-US.msi -OutFile $msiFile
# Prepare a log file name
$logFile = [System.IO.Path]::GetTempFileName()
# Prepare the arguments to execute the MSI
$arguments= '/i ' + $msiFile + ' ADDLOCAL=ALL /qn /norestart LicenseAccepted="0" /lv ' + $logFile
# Sample = msiexec /i C:\Users\{user}\AppData\Local\Temp\2\tmp9267.msi ADDLOCAL=ALL /qn /norestart LicenseAccepted="0" /lv $logFile
# Execute the MSI and wait for it to complete
$proc = (Start-Process -file msiexec -arg $arguments -Passthru)
$proc | Wait-Process
Get-Content $logFile
```

***Note:** To complete the setup for publishing from Visual Studio, you will need to [setup Azure firewall rules](http://example.com), and [configure a DNS name for the VM](http://example.com). See documentation further below.*

# Walk-through: Create a new Azure VM for hosting your web app 
 
Here are the steps that we will follow in this post. 
- [Provision a new virtual machine using Azure Portal](#ProvisionNewVM)
- [Connect to the new VM (Remote Desktop Connection)](#ConnectToTheVM)
- [Install IIS (web server) plus Web Management Service and ASP.NET 4.6](#InstallVMFeatures)
- [Install Web Deploy](#InstallWebDeploy)
- [Configure inbound firewall rules in Azure ](#ConfigureFirewallRules)
- [Setup DNS name for the VM](#SetupDNSName)
 
<a name="ProvisionNewVM"></a>
## Provision a new virtual machine using Azure Portal 
1. Log in to the Azure portal at https://portal.azure.com 
2. Click the "+ New" in the top left corner. 
3. Look for Windows Server 2016 VM (Get Started category)  
   - or any Windows Server 2016 found in "Compute" category. Ie. "Windows Server 2016 Datacenter"
4. Fill out the required fields to configure the new Virtual Machine:
    * Name: eg. MyAzureVM
        * This is the name of VM as it appears in the portal.
        * IP Address will be generated
    * Username/Password
        * This is the username and password used when connecting via Remote Desktop Connection.
        * Also used when publishing websites from Visual Studio (shown in this tutorial).  
        * **Remember this password.** There is no way to retrieve it if you lose it.
    * Resource Group: eg. MyAzureVM-RG
        * Use a name similar to the VM name
        * Resource Group is a “virtual folder” of Azure resources. This will be the resource group that the new VM will be placed into. The other Azure services created as part of this process will also be included in this resource group. 
        * To clean up the VM you can delete the Resource Group, which will take care of deleting the other resources that were created, as well. 
5. Choose a size for the VM  
    * DS1_V2 will be sufficient for this tutorial
    * The size can be changed later
    * If you have Free Azure Credits (e.g. as a part of you MSDN Subscription), it might cover the costs.
      * Check your [Visual Studio subscription]. Check for [offers for monthly Azure credit][Check for Azure offers].  
    * Note: The costs shown in your Azure portal may differ from the values shown here. Cost is different based on Region and is subject to changes over time.
6. Accept the defaults in **Step 3: (Settings)**  
7. Confirm details in Summary and "Purchase".  
   Provisioning will begin, and an icon will appear on Azure Portal dashboard  
   You can watch the progress in the notifications area.  
   Provisioning completes (after about 5 minutes)  

[Visual Studio subscription]: https://my.visualstudio.com/subscriptions
[Check for Azure offers]: https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/

<a name="ConnectToTheVM"></a>
## Connect to new virtual machine (Remote Desktop Connection)

1. Open new VM in Azure Portal  
2. Click the "Connect" link in the toolbar  
   * Downloads RDP file in web browser. Run it to open RDP connection.  
   * Opens Remote Desktop Connection pointing to IP address of the new Azure VM  
3. Accept warning about unknown publisher  
4. Enter Credentials  
   * Don't log in with your work/home user account.  
   * Choose "More choices" -> "Use a different account"  
   * Enter the username and password you used to create the new Azure VM  
5. Accept warning about security certificate  
   Window opens with remote connection to new Azure VM.  
   * Server Manager - Dashboard opens on first startup  

<a name="InstallVMFeatures"></a>
## Install IIS (Web Server) plus Web Management Service and ASP.NET 4.6 
 
1. Open the Server Manager Dashboard
2. Choose "2 Add roles and features"
3. Accept the defaults and press "Next" three times to progress to the "Server Roles"section.
4. Select "Web Server (IIS)"
5. Confirm the installation of additional tools (including management tools)
6. Press "Next" and proceed to the Roles Services configuration
7. Select "Management Service".
   - This service is required to enable Web Deploy (through port 8172)
8. Add feature ASP.Net 4.6
9. Accept the inclusion of ASP.Net 4.6
10. Press "Next" to confirm configuration.
11. Press "Install" to complete IIS setup.  
    Wait for feature installation to complete. 
    IIS now appears in Server Manager

<a name="InstallWebDeploy"></a>
## Install Web Deploy 3.6 
	
### Configure IE Enhanced Security (Off) 

This allows you to download executables via Internet Explorer. 

1. In the **Server Manager** go to the **Local Server** section
2. Look for "IE Enhanced Security Configuration: On"
3. Click "On".  
  Launches dialog to configure IE security restrictions
4. Choose "Off" for Administrators
5. Press "OK" to accept the change

### Download Web Deploy 3.6

1. Launch Internet Explorer
2. Accept default security settings. Note that IE has Enhanced Security disabled.
3. Navigate to Microsoft Download site to download Web Deploy 3.6  
   https://www.microsoft.com/en-us/download/details.aspx?id=43717
4. Choose the AMD-64bit version
5. Allow pop-ups and Run the downloaded MSI
6. Follow installation steps for Web Deploy 3.6
7. Choose "Complete" option to install all components  
    Will install the following components

<a name="ConfirmSetup"></a>
## Confirm your setup (Services and Firewall rules)

**Check that services are installed and running in the Server Manager**
* Open the Server Manager 
* Go to the IIS section 
* You should see the following services running in the **Server Manager - IIS** section 
  - Click the refresh icon in the top toolbar as required

***Note:** Make sure service "MsDepSvc" is installed and running. This is the MSDeploy service (Web Deploy). It might not show up in the Server Manager, but you should see in the Task Manager.*

**Check the inbound firewall rules from within the VM**

* Open **Windows Firewall with Advanced Security** (found in Windows Administrative Tools) 
* Confirm two entries:  
  - Web Management Service (Port 8172)  
  - World Wide Web Service (Port 80)  

<a name="ConfigureFirewallRules"></a>
## Configure Inbound Firewall Rules in the Azure Portal

- Login to the [Azure Portal](http://portal.azure.com) 
- Open the VM (Virtual Machines->MyAzureVM) 
- Open the "Networking" section 
- Create two new firewall entries via "Add inbound port rule"
	- http - Port 80 (Priority 100) 
	- WebDeploy - Port 8172 (Priority 1010) 

<a name="SetupDNSName"></a>
## Configure a DNS name for the VM via the Azure Portal

To use the in-built web publishing wizard in Visual Studio, you’ll need to configure a DNS name for the virtual machine. This can be done from the Azure Portal.

- From the Azure Portal, navigate to the Overview page of your virtual machine.
- Under “DNS name”, click “Configure”
- Provide a DNS name label that is globally unique.
- When the name is validated, a green tick will appear.
- Click “Save” to save the configuration.

<a name="NextSteps"></a>
# Next Steps

<a name="PublishWebAppFromVisualStudio"></a>
## Publish a web application from Visual Studio

There’s a separate article that describes the process of publishing a web app from Visual Studio. It is designed to pick up from where this article leads off and includes instructions for configuring the DNS, and troubleshooting tips.
 
 
 
