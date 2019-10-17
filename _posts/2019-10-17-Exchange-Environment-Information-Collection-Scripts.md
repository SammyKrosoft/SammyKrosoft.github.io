---
layout: post
author: sammykrosoft
comments: true
categories: [PowerShell,Exchange 2010, Certificate]
permalink: /Exchange-Environment-Information-Collection-Scripts.html
---

# Exchange environment report script

[Exchange environment report script](https://gallery.technet.microsoft.com/exchange/Generate-Exchange-2388e7c9)

Run the following:
```powershell
.\Get-ExchangeEnvironmentReport.ps1 -HTMLReport .\YourOrganization-report.html
```
Which will generate the environment report on the local directory where you are running the script from
NOTE: don’t forget to run the Exchange Management Shell as an Admin (elevated prompt)

# Exchange Health Report

[Exchange Health Report Script from Paul Cunningham on GitHub](https://raw.githubusercontent.com/cunninghamp/Test-ExchangeServerHealth.ps1/master/Test-ExchangeServerHealth.ps1)

NOTE: Most practical way is to copy the content of the above page, paste it in a NOTEPAD window, and then save it as “Test-ExchangeServerHealth.ps1” file
IMPORTANT: Modify this inside the script before running (starting line 186 in the script – or just search for the string “Modify these EmailSettings” in the script):

```powershell
#...................................
# Modify these Email Settings
#...................................

$smtpsettings = @{
        To =  "MyTeam@contoso.ca"
        From = " Consultant@contoso.ca"
        Subject = "$reportemailsubject - $now"
        SmtpServer = "any_Exchange_server_that_has_the_HUB_role"
        }
```

And then run the following:
```powershell
.\Test-ExchangeServerHealth.ps1 -ReportMode -SendEmail
```

# Double-Check Exchange Certificate settings on your servers

[Exchange Certificate Report Script from Paul Cunningham on Github](https://raw.githubusercontent.com/cunninghamp/Powershell-Exchange/master/Get-ExchangeCertificateReport/Get-ExchangeCertificateReport.ps1)

NOTE: Most practical way is to copy the content of the above page, paste it in a NOTEPAD window, and then save it as “Get-ExchangeCertificateReport.ps1” file

Just run the following:
```powershell
.\CertificateReport.ps1
```

