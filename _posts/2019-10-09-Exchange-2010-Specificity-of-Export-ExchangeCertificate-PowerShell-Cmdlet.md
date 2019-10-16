---
layout: post
author: sammykrosoft
comments: true
categories: [PowerShell,Exchange 2010, Certificate]
permalink: /Exchange-2010-Specificity-of-Export-ExchangeCertificate-PowerShell-Cmdlet.html
---

# Create E2010 certificate, Import, Assign, Export, Import on another machine

## Creating the request
ISSUE: there is no ```-Path``` or ```-FileName``` parameter with the ```Export-ExchangeCertificate``` Exchange cmdlet.


Just storing my lab's blurb below, I'll format it better later on...

```powershell
# Storing the New-ExchangeCertificate parameters value as well as the path we want the file request stored in
# into variables - we'll do the same for the Thumbprint -> that way we just have to update the values at one spot
# for multiple commands instead of having to update the values for each command lines...

$CertRequestPath = ".\request3.txt"
$SubjectName = "c=CA, s=ONTARIO, l=OTTAWA, o=PCO, ou=PCO, cn=mail.pco-bcp.gc.ca"
$DomainNames = "mail.pco-bcp.gc.ca", "autodiscover.pco-bcp.gc.ca"

# As we don't have a -FileName or a -Parameter attribute
Set-Content -path $CertRequestPath -Value (New-ExchangeCertificate -GenerateRequest -KeySize 2048 -SubjectName $SubjectName -DomainName $DomainNames -PrivateKeyExportable $True)

# Just by curiosity if we want to see the actual certificate
#request blurb (usually BASE64 encoded), or if we want to copy/paste
#the cert request blurb directly after having it generated,
#we just open the file with norepad directly from PowerShell:
notepad $CertRequestPath

# Then double-checking the Exchange certificates, including
#the request we just did:
get-exchangecertificate

# Just opening an Explorer window to view the current directory we're working from:
explorer .
```
## Import (or Install) the new certificate on the Same machine

```powershell
# Storing the certificate path (the one received from the request we made earlier) in a variable
$CertPath = "C:\scripts\cert4.cer"

# Importing the certificate
# IMPORTANT NOTE: The cert has to be imported on the same machine
# where we made the request:
Import-ExchangeCertificate -FileData ([Byte[]]$(Get-Content -Path $CertPath -Encoding byte -ReadCount 0))


#Showing the Exchange Certificates before enabling the 
# Services on the new cert:
write-host "Before:"
Get-ExchangeCertificate -Server O-EX-MAIL-01

write-host "Assigning services..." -BackgroundColor Red -ForegroundColor Blue

#NOTE: change the thumbprint to your cert's thumbprint
# that you identified on the previous Get-ExchangeCertificate
# above
$Thumbprint = "3E840940A0413E75DBAA1A14FE495086B97C2642"

#We enable the IIS services (for OA, OAB, EWS, MAPI/HTTP, ...) on the new certificate
# identified by its thumbprint that we stored on a variable just above
Enable-ExchangeCertificate -Thumbprint $Thumbprint -Services POP,IMAP,SMTP,IIS

# and we dump the new view of Exchange certs after enabling
# services on the new certificate (note that you'll have IMAP, POP and SMTP on all your certs...)
write-host "After:"
Get-ExchangeCertificate -Server O-EX-MAIL-01
```

## Export the certificate with the Private Key

```powershell
# Setting the file path and name where to export the certificate to
#in a variable for easier manipulation:
$ExportCertTo = ".\ExportedCert.pfx"
#Exchange 2013/2016:
#Export-ExchangeCertificate -Thumbprint $Thumbprint -FileName $ExportCert -BinaryEncoded -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force) -Server O-EX-MAIL-01

#Exchange 2010 - there is no -FileName or -Path parameter for the older Export-ExchangeCertificate, despite what the documentation
# is showing... instead, store the Export-ExchangeCertificate result in a variable, and write it in a file
# using Set-Content, and -Encoding Byte parameter.
$file = Export-ExchangeCertificate -Thumbprint $Thumbprint -BinaryEncoded:$true -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force)
Set-Content -Path $ExportCertTo -Value $file.FileData -Encoding Byte

#Optional: open an Explorer window in the current directory
# To check that the cert has been exported:
explorer .
```

## Then to import the certificate on ANOTHER machine:


```powershell
$ExportedCertPath = "c:\scripts\$ExportCertTo" 

Import-ExchangeCertificate -FileData ([Byte[]]$(Get-Content -Path $ExportedCertPath -Encoding byte -ReadCount 0)) -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force)
```