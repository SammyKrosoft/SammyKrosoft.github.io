---
layout: post
title: How to check AD Schema, Forest and Domain versions with PowerShell using [ADSI] type accelerator
date: 2020-01-23 13:13
tags: [Exchange,AD,PowerShell,Active Directory]
---


The below is a script to demonstrate the use of the [ADSI] type accelerator to get some properties of AD components (Schema version, Exchange Forest version, Exchange domain version) to verify an Exchange deployment for example.


```powershell
# First we get the root domain of the local AD we run the script from:
$RootDSE= ([ADSI]"").distinguishedName

#Then we get the Exchange Schema version from that AD:
$ForestRangeUpper = ([ADSI]"LDAP://CN=ms-Exch-Schema-Version-Pt,CN=Schema,CN=Configuration,$RootDSE").rangeUpper
Write-Host "Forest rangeUpper value = AD’s Exchange Schema Version:" -BackgroundColor Yellow -ForeGroundColor Blue
$ForestRangeUpper

#Now getting the Exchange org name by querying the children of CN=Microsoft Exchange that's under 
# Configuration partition, under CN=Services object
$MSEConfigurationContainer = [ADSI]"LDAP://cn=Microsoft Exchange,cn=Services,cn=Configuration,$RootDSE"
$LDAPPathToExchangeORGObject = $MSEConfigurationContainer.Children.path
Write-Host "Here's the AD path to our Exchange Org:" -BackgroundColor Yellow -ForeGroundColor Blue
$LDAPPathToExchangeORGObject

#Getting the Forest Exchange Org version
$ForestObjectVersion = ([ADSI]"$LDAPPathToExchangeORGObject").objectVersion
Write-Host "... and here's the ObjectVersion of the Exchange Org object:" -BackgroundColor Yellow -ForeGroundColor Blue
$ForestObjectVersion

#Getting the Exchange Product ID registered in AD
$MSExchangeProductID = ([ADSI]"$LDAPPathToExchangeORGObject").msExchProductId
Write-Host "... and here's the ObjectVersion of the Exchange Org object info showing which Exchange version that was used to update the schema:" -BackgroundColor Yellow -ForeGroundColor Blue
$MSExchangeProductID

#Getting the Domain version (set with DomainPrep)
$DomainObjectVersion = ([ADSI]"LDAP://CN=Microsoft Exchange System Objects,$RootDSE").objectVersion
Write-Host "And finally the Domain objectVersion showing the DomainPrep status:" -BackgroundColor Yellow -ForeGroundColor Blue
$DomainObjectVersion

Write-Host "`n`nSSummary of all the above:" -BackgroundColor DarkGray

$ADStatusTable = [PSCustomObject]@{
        ExchangeProductID = $MSExchangeProductID
        ForestRAngeUpper = $ForestRangeUpper
        ForestObjectVersion = $ForestObjectVersion
        DomainObjectVersion = $DomainObjectVersion
}

$ADStatusTable
```