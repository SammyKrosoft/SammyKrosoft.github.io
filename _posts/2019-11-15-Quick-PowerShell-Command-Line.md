---
layout: post
author: sammykrosoft
comments: true
categories: [PowerShell, Exchange On-Premises]
permalink: /Get-WebServicesVirtualDirectory-Exported-With-All-Parameters.html
---

```powershell
#Storing file path name in a variable to use it twice: one in the Export-CSV, one to open it after populating it
$OutFile = "c:\temp\WebServicesProps.txt"

#Get-WebServicesVirtualDirectory with all parameters named, as we wish to export the Multivalued AD, Strings and System parameters at once - Using $OutFile for Export-CSV
Get-WebServicesVirtualDirectory -ADPropertiesOnly | Select CertificateAuthentication, InternalNLBBypassUrl, GzipLevel, MRSProxyEnabled, Name, LiveIdNegotiateAuthentication, WSSecurityAuthentication, LiveIdBasicAuthentication, BasicAuthentication, DigestAuthentication, WindowsAuthentication, OAuthAuthentication, AdfsAuthentication, MetabasePath, Path, ExtendedProtectionTokenChecking, AdminDisplayVersion, Server, InternalUrl, ExternalUrl, AdminDisplayName, ExchangeVersion, DistinguishedName, Identity, Guid, ObjectCategory, WhenChanged, WhenCreated, WhenChangedUTC, WhenCreatedUTC, OrganizationId, Id, OriginatingServer, IsValid, ObjectState, @{Name="ExternalAuthenticationMethods";Expression={$_.ExternalAuthenticationMethods -join ";"}}, @{Name="InternalAuthenticationMethods";Expression={$_.InternalAuthenticationMethods -join ";"}}, @{Name="ExtendedProtectionFlags";Expression={$_.ExtendedProtectionFlags -join ";"}}, @{Name="ExtendedProtectionSPNList";Expression={$_.ExtendedProtectionSPNList -join ";"}}, @{Name="ObjectClass";Expression={$_.ObjectClass -join ";"}} | Export-CSV -NoTypeInformation $OutFile

#Opening the file immediately after the export to either save it under another name or copy/paste the results directly
Notepad $OutFile
```powershell