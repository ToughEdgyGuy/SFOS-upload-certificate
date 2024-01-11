Forked from https://github.com/mwcs4/SFOS-upload-certificate
##### additional Ressources
 - https://community.sophos.com/sophos-xg-firewall/f/discussions/138668/upload-certificates-using-powershell-to-automate-let-s-encrypt
 - https://github.com/win-acme/win-acme/releases
 - https://www.alitajran.com/install-free-lets-encrypt-certificate-in-exchange-server/

### Usecase
Microsoft Exchange Server behind a Sophos XG Firewall with the WAF Feature enabeld and configured. Since Exchange Server uses multiple virtual directories, our Certificate usualy has multiple Hostnames in the Lets Encrypt Certificate (atleast autodiscover.[domain]). 
The Problem with the original Script is that if there are multiple Domains Specified in the WAF Rule, the Script would not match the Rule and therefore not Change the Certificat.

## How to Use the Workflow
1. Install Powershell v7 (https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4)
2. Download and Extract the Win-Acme Script (https://github.com/win-acme/win-acme/releases)
3. run the Win-Acme Script and configure like it is specified here (https://www.alitajran.com/install-free-lets-encrypt-certificate-in-exchange-server/).
   It is important to change the configfile so the Private Key is Exportable
4. download the Upload-Cert.ps1 Script
5. Create User on the Sophos Firewall and enable the API + the IP of the Source Server that runs this Script
6. create an automatic task that runs the Upload-Cert.ps1 Script daily
   
``` powershell 7
.\Upload-Cert.ps1 -Uri "https://_SOPHOS_IP_/webconsole/APIController" -Credential (New-Object System.Management.Automation.PSCredential("_SOPHOS_USER_", (ConvertTo-SecureString "_SOPHOS_USER_PASSWORD_" -AsPlainText -Force))) -CertificateFriendlyName "_CERTNAME_SPECIFIED_IN_THE_WIN_ACME_SCRIPT_" -verbose
```