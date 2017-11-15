# Migrate an ASP.NET Web Application to Azure App Service

[App Service](https://docs.microsoft.com/azure/app-service/app-service-web-overview#why-use-web-apps) is a fully managed compute platform service that is optimized for hosting scalable websites and web applications. This document provides information on how to lift-and-shift an existing application to Azure App Service, modifications to consider, and additional resources for moving to the cloud.

Ready to get started? [Publish your ASP.NET + SQL application to Azure App Service](https://go.microsoft.com/fwlink/?linkid=863214)

# Preparation   
* [How to determine if your app qualifies for App Service](https://azure.microsoft.com/downloads/migration-assistant/)
* [Moving your database to the cloud](https://go.microsoft.com/fwlink/?linkid=863217)

# Considerations
There are several factors you must consider before migrating your application. Below is a list of potential modifications you might have to make to your application, and how to go about doing them.

## SQL Database configuration
If your application is using an on-premises database, you have several options for your web app. You can read more about [migrating SQL databases to Azure](https://go.microsoft.com/fwlink/?linkid=863217).

## IIS
Everything traditionally configured via applicationHost.config in your application can now be configured through the portal. This applies to AppPool bitness, Enable/Disable Websockets, Managed pipeline version, .NET Framework version (2.0/4.0), etc. To modify your [Application Settings](https://docs.microsoft.com/en-us/azure/app-service/web-sites-configure) navigate to the [Azure Portal](https://portal.azure.com), open the blade for your web app and select the "Application Settings" tab.

## Authentication
If your application authenticates users at any point, you will have to modify this functionality to work the application deployed on Azure Web Apps. One possibility is to use Azure AD connect to integrate your on-premises directories with Azure Active Directory. Learn more about how to [Integrate your on-premises directories with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## Virtual Network Modification
If you use more than one Azure service you may consider using a Virtual Network to securely communicate between these services. You can configure a connection from your on-premises network to the [Azure Virtual Network](https://docs.microsoft.com/en-us/azure/app-service/web-sites-integrate-with-vnet) using VPN or ExpressRoute.

## Monitoring and Diagnostics
Your current on-premises solutions for monitoring and diagnostics are unlikely to work in the cloud. However, Azure provides tools for logging, monitoring, and diagnostics so that you can identify and debug issues with Web Apps. You can easily enable diagnostics for your web app in its configuration, and you can view the logs recorded in Azure Application Insights. Learn more about [Enabling diagnostics logging for web apps](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log).

## Connection Strings and application settings
One option for keeping information safe is to use [Azure KeyVault](https://docs.microsoft.com/azure/key-vault/), a service that secure stores sensitive information used in your application. Alternatively, you can store this data as an App Service setting.

## DNS
You may need to update DNS configurations based on the requirements of your application. These DNS settings can be configured in the [DNS for App Service settings](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain). Another factor to consider is [Binding an existing custom SSL Certificate](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl).

## File system and Storage
In the case that your application persists data, you will need to update it to use Azure Storage instead. Azure Storage is a service that provides file shares for sharing with SMB protocol, blob storage, simple queues, and non-relational tables. Learn more about [Azure Storage file shares](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).
