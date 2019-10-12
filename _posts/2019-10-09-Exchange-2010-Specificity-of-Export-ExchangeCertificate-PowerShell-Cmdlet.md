# Another note from my lab...

## Exporting an Exchange 2010 certificate with Export-ExchangeCertificate

ISSUE: there is no ```-Path``` or ```-FileName``` parameter with the ```Export-ExchangeCertificate``` Exchange cmdlet.


Just storing my lab's blurb below, I'll format it better later on...

```PowerShell
# Storing the New-ExchangeCertificate parameters value as well as the path we want the file request stored in
# into variables - we'll do the same for the Thumbprint -> that way we just have to update the values at one spot
# for multiple commands instead of having to update the values for each command lines...

$CertRequestPath = ".\request3.txt"
$SubjectName = "c=CA, s=ONTARIO, l=OTTAWA, o=PCO, ou=PCO, cn=mail.pco-bcp.gc.ca"
$DomainNames = "mail.pco-bcp.gc.ca", "autodiscover.pco-bcp.gc.ca"

# As we don't have a -FileName or a -Parameter attribute
Set-Content -path $CertRequestPath -Value (New-ExchangeCertificate -GenerateRequest -KeySize 2048 -SubjectName $SubjectName -DomainName $DomainNames -PrivateKeyExportable $True)

notepad $CertRequestPath


get-exchangecertificate

explorer .


$CertPath = "C:\scripts\cert4.cer"


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


# Setting the file path and name in a variable for easier manipulation:
$ExportCert = ".\ExportedCert.pfx"
#Exchange 2013/2016:
#Export-ExchangeCertificate -Thumbprint $Thumbprint -FileName $ExportCert -BinaryEncoded -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force) -Server O-EX-MAIL-01

#Exchange 2010 - there is no -FileName or -Path parameter for the older Export-ExchangeCertificate, despite what the documentation
# is showing... instead, store the Export-ExchangeCertificate result in a variable, and write it in a file
# using Set-Content, and -Encoding Byte parameter.
$file = Export-ExchangeCertificate -Thumbprint $Thumbprint -BinaryEncoded:$true -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force)
Set-Content -Path $ExportCert -Value $file.FileData -Encoding Byte

#Optional: open an Explorer window in the current directory
# To check that the cert has been exported:
explorer .
```

Then to import the certificate on another machine:

```PowerShell
$ExportedCertPath = "c:\scripts\$ExportCertTo" 

Import-ExchangeCertificate -FileData ([Byte[]]$(Get-Content -Path $ExportedCertPath -Encoding byte -ReadCount 0)) -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force)
```