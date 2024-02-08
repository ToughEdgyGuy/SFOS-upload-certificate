Forked from https://github.com/mwcs4/SFOS-upload-certificate
##### additional Ressources
 - https://community.sophos.com/sophos-xg-firewall/f/discussions/138668/upload-certificates-using-powershell-to-automate-let-s-encrypt
 - https://github.com/win-acme/win-acme/releases
 - https://www.alitajran.com/install-free-lets-encrypt-certificate-in-exchange-server/

### Usecase
Microsoft Exchange Server behind a Sophos XG Firewall with the WAF Feature enabeld and configured. Since Exchange Server uses multiple virtual directories, our Certificate usualy has multiple Hostnames in the Lets Encrypt Certificate (atleast autodiscover._domain_). 
The Problem with the original Script is that if there are multiple Domains Specified in the WAF Rule, the Script would not match the Rule and therefore not Change the Certificate.

## How to Use the Workflow
1. Install Powershell v7 (https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.4) download the .msi
2. Download and Extract the Win-Acme Script (https://github.com/win-acme/win-acme/releases) [show_more -> win-acme.vX.X.X.x64.pluggable.zip]
3. Create 2 WAF Rules
   - HTTPS: (you cannot use HTTP redirection) [maybe if the http rule is ontop and has pathspecific routing enabled]
   - HTTP: create SitePathRoute for /.well-known/acme-challenge/
4. configure the Win-Acme Script like specified here (https://www.alitajran.com/install-free-lets-encrypt-certificate-in-exchange-server/).
   It is important to change the configfile so the Private Key is Exportable
5. download the Upload-Cert.ps1 Script
6. Create an Administrator User on the Sophos Firewall and enable the API + the IP of the Source Server that runs this Script (beware of powershell escape Characters inside of the password)
7. create an automatic task with powershell 7 that runs the Upload-Cert.ps1 Script daily. (as System)
   
``` powershell 7
.\Upload-Cert.ps1 -Uri "https://_SOPHOS_IP_:4444/webconsole/APIController" -User "_SOPHOS_USER_" -Pw "_SOPHOS_USER_PASSWORD_" -CertificateFriendlyName "_CERTNAME_SPECIFIED_IN_THE_WIN_ACME_SCRIPT_" -verbose
```
