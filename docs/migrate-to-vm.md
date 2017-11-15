# Migrate an ASP.NET Web application to an Azure Virtual Machine

This document provides an overview of how to migrate an ASP.NET Web application from on-premises to an Azure Virtual Machine.

## Get Started

- Create a virtual machine for your ASP.NET application
    - [Create a new virtual machine for ASP.NET Applications](https://go.microsoft.com/fwlink/?linkid=863237)
    - [Migrate an existing on-premises virtual machine](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-migrate-on-premises-to-azure)
- [Publish your app using Visual Studio](https://go.microsoft.com/fwlink/?linkid=863240)
- [Create a secure virtual network for your VMs](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-get-started-vnet-subnet)
- [Create a CI/CD pipeline for your application](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups)
- [Move to a VM scale set for high availability and scalability](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app)

## Considerations

### Benefits
Virtual machines offer the easiest path to migrate an application from on-premises to the cloud.  They enable you to replicate the same environment your application uses on-premises, while removing the need to maintain your own data centers.  Virtual Machine Scale Sets provide high availability and scalability for applications running in Virtual Machines.

### Virtual Machine Size
Choose the virtual machine size and type that is best optimized for your workload.  See [sizes for Windows virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes) for more information.

### Maintenance
Just like an on-premises machine, with a virtual machine you are currently responsible for maintaining and updating the machine*.  If your application can run in a Platform as a Service (PaaS) environment such as App Service or in a Container, it will remove this need.
<br /><br />
*[Automatic OS upgrades for virtual machine scale sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) is currently available as a Preview service

### Virtual Networks
Azure Virtual networks enable you to
- Build a hybrid infrastructure that you control
- Bring your own IP addresses and DNS servers
- Get an isolated and highly-secure environment for your applications
- Connect your to your on-premises network using one of the [connectivity options](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways#s2smulti)
- Integrate your virtual machine into your on-premises network using [ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/)

To get started, see the [Virtual Network documentation](https://docs.microsoft.com/en-us/azure/virtual-network/)

### Active Directory
Many applications use Active Directory for authentication and identity management.  
- Azure AD Connect enables you to integrate your on-premises directories with Azure Active Directory.  To get started, see [Integrate your on-premises directories with Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect).  
- Alternately, [ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) enables your application to access your on-premises Active Directory.

### SQL Databases
If your application is using an on-premises database, your app will not be able to talk to it by default, you have two options. You can either:
- Configure a hybrid network that enables your application to access your database running on premises.  
- Migrate your database to the Azure.  See Migrate your SQL Server database to Azure for more information.

### High Availability and Scalability

#### Virtual Machine Scale Sets
You want to make sure that your application is highly available and can scale, migrate your VM image to an Azure Virtual Machine Scale Set to improve the availability and scalability of your application.  VM Scale Sets provide the ability to use an existing VM you’ve already configured or set up a build pipeline to build the image with your application.  
<br />
To get started, see Deploy your application on virtual machine scale sets.

#### Centralized Logging
When running your application across multiple instances, consider storing your logs in a centralized location such as [Azure Storage](https://docs.microsoft.com/en-us/azure/storage/).


