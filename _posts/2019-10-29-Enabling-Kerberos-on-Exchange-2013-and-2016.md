---
layout: post
author: sammykrosoft
comments: true
categories: [PowerShell,Exchange 2010, Certificate]
permalink: /Enabling-Kerberos-on-Exchange-2013-and-2016.html
---

# Microsoft Documentation - Set Kerberos for Load Balanced Exchange 2016 Client Access Services

Here I just prepped a quick Excel spreadsheet that show the sequence of commands to use when you setup an Exchange Alternate Service Account on Exchange 2013 or 2016.

[See the Downloads section for links to the Kerberos instructions Excel spreadsheet as well as the link to the Github repository containing the file](#Download)



## Introduction
This is an Excel spreadsheet intended to help in setting Kerberos for Exchange CAS service.

The official Microsoft Documentation is here:
[Configure Kerberos for Exchange 2016 Client Access Service](https://docs.microsoft.com/en-us/exchange/architecture/client-access/kerberos-auth-for-load-balanced-client-access?view=exchserver-2016)

## Principle

### Replace the below value on the top table on the spreadsheet with your values:

|Sample domain hostname|CONTOSO|
|----------------------|-------|
|Sample Exchange Name 1|server1.contoso.ca|
|Sample Exchange Name 2|server2.contoso.ca|
|Sample URL|mail.contoso.ca|
|Sample ASA Computer name|Exchange2016ASA|
|Sample E2010 ASA Computer name|E2010ASA|

### The PowerShell commands will be updated with the values specified in the above table

|Order|Set/Check|Command to run|
|-----|---------|--------------|
|1|New-ADComputer|New-ADComputer -Name Exchange2016ASA -AccountPassword (Read-Host 'Enter password' -AsSecureString) -Description 'Alternate Service Account credentials for Exchange do not delete' -Enabled:$True -SamAccountName Exchange2016ASA|
|2|Set-ADComputer|Set-ADComputer Exchange2016ASA -add @{"msDS-SupportedEncryptionTypes"="28"}|
|3|cd|cd $exscripts|
|4|Script (Set)|.\RollAlternateServiceAccountPassword.ps1 -ToSpecificServer server1.contoso.ca -GenerateNewPasswordFor ESDC\Exchange2016ASA$|
|5|Script (Set)|.\RollAlternateServiceAccountPassword.ps1 -ToSpecificServer server2.contoso.ca -copyfrom server1.contoso.ca|
|6|Script (Set)|"#Optional, to configure Kerberos on all servers at once <br>$E2016 = Get-ExchangeServer \| ? {$_.AdminDisplayVersion -like ""*15.1*""}"|
|7|--- Script (Set) followup|"#And then: <br>$E2016 \| Foreach {.\RollAlternateServiceAccountPassword.ps1 -ToSpecificServer ""$($_.Name).$($_.Domain)"" -copyfrom server1.contoso.ca}"|
|8|Script (Check)|Get-ClientAccessServer server1.contoso.ca -IncludeAlternateServiceAccountCredentialStatus \| Format-List Name, AlternateServiceAccountConfiguration|
|9|Script (Check)|get-ExchangeServer \| ? {$_.AdminDisplayVersion -like "*15.1*"} \| Get-ClientAccessService -IncludeAlternateServiceAccountCredentialStatus \| Format-List Name, AlternateServiceAccountConfiguration|
|10|setspn (Check)|setspn -F -Q http/mail.contoso.ca|
|11|Setspn (Remove)|setspn –D http/mail.contoso.ca CONTOSO\E2010ASA$|
|12|setspn (Set)|setspn -S http/mail.contoso.ca CONTOSO\Exchange2016ASA$|
|13|setspn (Check)|Setspn -L CONTOSO\Exchange2016ASA$|
|14|setspn (Check)|setspn -F -Q http/mail.contoso.ca|

## Download

[Download the Kerberos enablement instructions Excel file](https://github.com/SammyKrosoft/MSDoc-Set-Kerberos-Exchange2016/raw/master/Configure-ExchangeKerberos.xlsx)

or

[Go to the corresponding GitHub repository](https://github.com/SammyKrosoft/MSDoc-Set-Kerberos-Exchange2016)