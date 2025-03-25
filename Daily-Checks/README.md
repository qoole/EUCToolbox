# EUCToolbox
# Instructions for Use

NOTE: The web service is for one-off alerting, if you want regular alerts, simply use the Jobs functionality within the runbook and populate the fields as required.  



# Scripted Deployment

First you will need an app-reg with the appropriate permissions which can be created by running "create-app-reg.ps1"
[create-app-reg.ps1](https://raw.githubusercontent.com/qoole/EUCToolbox/main/Daily-Checks/Install%20Scripts/create-app-reg.ps1)
  Make a note of the client ID and secret, you will need these later

Second you want to deploy the resources to Azure by clicking this link:
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fqoole%2FEUCToolbox%2Fmain%2FDaily-Checks%2FInstall%2520Scripts%2Farm-template.json)  

In the created Azure Storage container, upload the email template and make a note of the URL (customize as needed)
[email-template.html](https://raw.githubusercontent.com/qoole/EUCToolbox/main/Daily-Checks/Email%20Template/email-template.html)

Fill in the required details and it will create runbooks and an app service with the web page content.  
The runbook script content will be automatically populated, but if you want to start with a forked version, update accordingly.  
When it is complete, click on Outputs and make a note of the details.  
![alt text](https://euctoolbox.com/images/outputs-image.jpg)  

Finally, navigate to your new app service website and you will be prompted to enter the details recorded earlier.  


Add these and you are now up and running

# Manual Deployment

App Reg:
1) Create a new App Registration (multi-tenant if required) and make a note of the Application (client) ID
2) Add the redirect URIs to Microsoft default values:
- https://login.microsoftonline.com/common/oauth2/nativeclient
- https://login.live.com/oauth20_desktop.srf
- msal2afd3959-6ff7-400b-8cb3-4c4828166bf1://auth
3) Add these API permissions (all Application):
- AppCatalog.Read.All
- Application.Read.All
- AuditLog.Read.All
- CloudPC.Read.All
- Device.Read.All
- DeviceManagementApps.Read.All
- DeviceManagementConfiguration.Read.All
- DeviceManagementManagedDevices.Read.All
- DeviceManagementRBAC.Read.All
- DeviceManagementServiceConfig.Read.All
- Directory.Read.All
- Domain.Read.All
- Group.Read.All
- GroupMember.Read.All
- Organization.Read.All
- Policy.Read.All
- Policy.Read.ConditionalAccess
- Policy.Read.PermissionGrant
- Policy.ReadWrite.SecurityDefaults
- Reports.Read.All
- RoleManagement.Read.Directory
- SecurityEvents.Read.All
- ServiceHealth.Read.All
- ServiceMessage.Read.All
4) Click on Certificates and secrets and create a new secret.  Make a note of the secret value

Runbooks:
1) Create a new automation account and add these modules to it:
 - Microsoft.Graph.Groups
 - Microsoft.Graph.DeviceManagement.Enrollment
 - Microsoft.graph.authentication
 - Microsoft.graph.devices.corporatemanagement
 - Microsoft.Graph.Identity.Governance
2) Create a runbook, run on Azure, PowerShell v5.1 or v7
3) Paste this script into the content:
[Daily-Checks-Runbook.ps1](https://raw.githubusercontent.com/qoole/EUCToolbox/main/Daily-Checks/Runbook%20Script/Daily-Checks-Runbook.ps1)
4) Publish it
5) Click Webhooks and create a new webhook.  Don't populate any of the fields, they are passed from the application itself.
6) Make a note of the URI, it will not display again after leaving the page.

Storage Account:
1) Create a new Azure storage account
2) Create a container to store your email template (blob storage, public access to blobs only)
3) Upload the email template and note the URL
[email-template.html](https://raw.githubusercontent.com/qoole/EUCToolbox/main/Daily-Checks/Email%20Template/email-template.html)


Web Service
1) Create an Azure App Service running PHP (latest version), or you can use any other web hosting facilities
2) Copy the contents of this directory into the root:
[Webpage Content](https://github.com/qoole/EUCToolbox/tree/main/Daily-Checks/Webpage%20Content)
3) Navigate to the new URL and populate the fields
