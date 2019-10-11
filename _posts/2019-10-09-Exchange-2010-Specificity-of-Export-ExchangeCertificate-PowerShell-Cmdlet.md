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


cls

write-host "Before:"
Get-ExchangeCertificate -Server O-EX-MAIL-01

write-host "Assigning services..." -BackgroundColor Red -ForegroundColor Blue

$Thumbprint = "3E840940A0413E75DBAA1A14FE495086B97C2642"

Enable-ExchangeCertificate -Thumbprint $Thumbprint -Services POP,IMAP,SMTP,IIS

write-host "After:"
Get-ExchangeCertificate -Server O-EX-MAIL-01


$ExportCert = ".\ExportedCert.pfx"
#Exchange 2013/2016:
#Export-ExchangeCertificate -Thumbprint $Thumbprint -FileName $ExportCert -BinaryEncoded -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force) -Server O-EX-MAIL-01

#Exchange 2010 - there is no -FileName or -Path parameter for the older Export-ExchangeCertificate, despite what the documentation
# is showing... instead, store the Export-ExchangeCertificate result in a variable, and write it in a file
# using Set-Content, and -Encoding Byte parameter.
$file = Export-ExchangeCertificate -Thumbprint $Thumbprint -BinaryEncoded:$true -Password (ConvertTo-SecureString -String 'password' -AsPlainText -Force)
Set-Content -Path $ExportCer -Value $file.FileData -Encoding Byte

```