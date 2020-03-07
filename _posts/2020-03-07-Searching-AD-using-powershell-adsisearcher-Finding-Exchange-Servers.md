---
layout: post
author: sammykrosoft
title: List Exchange servers without Exchange Tools, just using [ADSI] and [ADSISEARCHER] PowerShell type accelerators
comments: true
categories: [PowerShell, Active Directory, Exchange]
permalink: /searching-Exchange-servers-using-ADSI-ADSISearcher-type-accelerators.html
---

# Finding Exchange servers and their properties in AD using [ADSI] and [ADSISearcher] type accelerators

## Overall Principle

The principle is the following:

- Using a variable to connect to the AD forest's Configuration partition ```$RootADConfigPartition = ([ADSI]'LDAP://RootDSE').ConfigurationNamingContext```
- Using another variable, create the **ADSISearcher** base object ```$ADConnection = [ADSISearcher]""``` which root is by default the current domain, will be the AD forest's Configuration partition, loaded on the first variable mentionned above ```$ADConnection.SearchRoot = "LDAP://$RootADConfigPartition"```
- Then use your filter, aka what do you want to search ... can be computers with certain names, or computers of certain type, like Exchange server in this example ```objectCategory=CN=MS-Exch-Exchange-Server,CN=Schema,$RootADConfigPartition```

## Repository Link

[Scripts to search for Exchange servers](https://github.com/SammyKrosoft/Search-AD-Using-Plain-PowerShell)

## Script 
Pasting the script from the above mentionned repository for quick reading convenience:

```powershell
$RootADConfigPartition = ([ADSI]'LDAP://RootDSE').configurationNamingContext

Write-host "Rood AD config partition" -BackgroundColor Yellow
$RootADConfigPartition

Write-Host "Creating ADSISearcher:" -BackgroundColor Yellow
Write-Host "LDAP://$RootADConfigPartition"

$ADConnection = [ADSISearcher]""
$ADConnection.SearchRoot = "LDAP://$RootADConfigPartition"

$ADConnection.Filter = "(objectCategory=CN=MS-Exch-Exchange-Server,CN=Schema,$RootADConfigPartition)"

write-host "Search root:" -BackgroundColor Yellow
$ADConnection | select -ExpandProperty SearchRoot


$allresults = $ADConnection.FindAll()

$Coll = @()
Foreach ($item in $allresults) {
    $coll+=[pscustomobject]@{
        Name = $Item.Properties['name']
        Version = $Item.Properties['serialNumber']
        Site = @("$($Item.Properties['msExchServerSite'] -Replace '^CN=|,.*$')")
        RolesNb = $Item.Properties['msExchCurrentServerRoles']
        RolesString = Switch ($Item.Properties['msExchCurrentServerRoles']){
                        2   {"MBX"}
                        4   {"CAS"}
                        16  {"UM"}
                        20  {"CAS, UM" -split ","}
                        32  {"HUB"}
                        36  {"CAS, HUB" -split ","}
                        38  {"CAS, HUB, MBX" -split ","}
                        54  {"MBX"}
                        64  {"Edge"}
                        16385   {"CAS"}
                        16439   {"CAS, HUB, MBX"  -split ","}
                            }
    }
}
$coll | ft


$Exchange2010Count = ($Coll | ? {$_.Version -match "14\."} | measure).count
$Exchange2013Count = ($Coll | ? {$_.Version -match "15\.0"}| measure).count
$Exchange2016Count = ($coll | ? {$_.Version -match "15\.1"}| measure).count

#NOTE: I am using (Object with filter | measure).count instead of (Object with filter).count directly
#because when there is only 1 object matching filter, for some reason the (object with filter).count
#return an empty object ... piping to " measure" which is an alias for Measure-Object, the count
#seems to be always right, even when there is only 1 object in the Object collection matching the filter.

Write-Host "Number of Exchange 2010 servers:    $Exchange2010Count"
Write-Host "Number of Exchange 2013 servers:    $Exchange2013Count"
Write-Host "Number of Exchange 2016 servers:    $Exchange2016Count"

<# msExchCurrentServerRoles values:
    The value here is issued from a bitwise value.
    Single role servers:
        MBX=2,
        CAS=4,
        UM=16,
        HT=32,
        Edge=64 
    multirole servers:
        CAS/HT=36,
        CAS/MBX/HT=38,
        CAS/UM=20,
        E2k13 MBX=54,
        E2K13 CAS=16385,
        E2k13 CAS/MBX=16439

#>
```