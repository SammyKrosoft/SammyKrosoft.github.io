---
layout: post
author: sammykrosoft
title: Exporting Exchange On-Premises EWS properties
comments: true
categories: [PowerShell, Exchange On-Premises]
permalink: /searching-AD-using-powershell-adsisearcher-type-accelerator.html
---

# Raw notes about how to search in AD without loading AD module

PowerShell has what we call "type accelerators", which are like aliases to access some type of .NET objects and/or functions.

To list the available accelerators on your system, you can use the below:
```powershell
[psobject].Assembly.GetType("System.Management.Automation.TypeAccelerators")::Get
```

In this quick and dirty post we'll use an example with the ```[adsisearcher]``` type accelerator. But using the above command line you'll see we have other accelerators like ```[adsi]``` to make AD operations without the AD PowerShell module, but you also have simpler ones to define objects types like ```[ipaddress]``` that can be used to force or validate an IP address notation without having to use complicated regular expressions comparisons, or another one commonly used is the ```[pscustomobject]``` type accelerator that we often use to create PowerShewll custom objects like :

```
[pscustomobject]@{
    Name = $mailbox.name
    MailboxType = $Mailbox.RecipientTypeDetails
    ADObjectOULocation = $ADUser.Object
    ...
}
```

Here is an example to use the ```[adsisearcher]``` type accelerator to directly search in the current AD using PowerShell without any modules loaded.

Note that 
```[int]``` or ```[string]``` are other type accelerators we often use to define variable types for example ... ```[boolean]``` is another example.

# Raw script with [adsisearcher] example

```powershell
#Define the search object directly with the search filter (LDAP filter)
$a=[adsisearcher]"(&(objectCategory=computer)(name=*E20*))"

#Dump some properties like the SearchRoot or the SearchScope
Write-Host "Current search root:" -BackgroundColor Yellow -ForegroundColor Blue
$a.SearchRoot
Write-Host "Current search scope:" -BackgroundColor Yellow -ForegroundColor Blue
$a.SearchScope

#For a list of properties, you can type $a | Get-Member
# or dump the first object with all its properties $a | select -first 1 | fl *

#Using a method specific to [adsisearcher] object called "FindAll()"
# to search all objects in AD using the filter defined with the $a variable above
Write-Host "Finding all objectsss" -BackgroundColor Yellow -ForegroundColor Blue
$Results = $a.FindAll()

#For a list of functions you can use with an object, use the already mentionned $a | Get-Member
#Note that in object oriented models, a function is called a "Method".
#Look for "Method" Member Type on the $a | Get-Member output to see the functions an object can call

#Dumping the results, here we have to separate the Path (LDAP Path of the object)
# as other AD properties of the object are defined in a separate hash table, each "Key" of that table is a property
# Using a custom PowerShell object for this, dumping the path property for each item, and calling the AD object property from Hash table using the $Item.properties['propertyname'] for each object of the result set

$col = @()
Foreach ($item in $Results) {
    $col += [pscustomobject]@{
        path = $Item.path
        'Object Category' = $Item.properties['objectcategory']
     }  

}

$col | ft
```

Sample output:

![](/assets/files/ADSISearcherSampleScriptOutput.png)