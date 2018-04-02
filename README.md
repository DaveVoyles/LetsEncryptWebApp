Azure Web App with Let's Encrypt Certificate
--------------------------------------------
**This is valid for 90 days.** You must run this command every 90 days to renew your SSL cert. It takes me ~1 minute to do. Not bad for free. You could likely even create an automated powershell script to run in Azure for you. 

This repository contains example code for creating an [Azure Web App](https://azure.microsoft.com/en-us/services/app-service/web/) with a [Let's Encrypt](https://letsencrypt.org/) SSL Certificate. It uses the [ACMESharp](https://github.com/ebekker/ACMESharp) Powershell module. You can install the ACMESharp Module in Powershell with:

```
Install-Module ACMESharp -AllowClobber
```

All the code needed to set up a Web App, generate the certificate, and bind the certificate is contained in the [CreateLetsEncryptWebApp.ps1](CreateLetsEncryptWebApp.ps1) script. The script does the following:

1. Creates a Web App with an App Service plan, if it doesn't exist already.
2. Pauses to allow the user to set a CNAME to point to the Web App. It is important to complete this step before continuing or the Web App will not allow the custom DNS name.
3. Creates an ACME Vault and registration (if it doesn't exist).
4. Generates a new ACME identifier for the DNS name.
5. Starts an HTTP challenge.
6. Uploads appropriate challenge reponse to the Web App.
7. Submits the challenge. 
8. Waits for challenge validation.
9. Generates certificate.
10. Binds certificate to the Web App

If the Web App already exists, it will simple generate a new cert and bind it, effectively renewing the certificate. 

### Instructions

1. From PowerShell, as Administrator, clone this repository 
2. Log into Azure via the CLI, with Login-AzureRmAccount
3. Set the correct account ID with Set-AzureRmContext <Account #>
4. Install-Module -Name AzureRM -AllowClobber
5. Install-Module ACMESharp -AllowClobber
6. Import-Module -Name AzureRM
7. Call the script:

```
.\CreateLetsEncryptWebApp.ps1 -ResourceGroupName "RESOURCE-GROUP-NAME" `
-WebAppName "WEB-APP-NAME" -Fqdn "DOMAIN NAME" -Location "LOCATION" `
-ContactEmail "EMAIL ADDRESS FOR REGISTRATION"
```

You'll be prompted to enter a temporary password. You'll also be prompted for the domain aName. 
