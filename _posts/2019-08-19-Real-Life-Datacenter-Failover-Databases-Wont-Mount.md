---
layout: post
author: sammykrosoft
comments: true
categories: [Exchange,Databases]
permalink: /Real-Life-Datacenter-Failover-Databases-Wont-Mount.html
---

# a note from the field...

## Activating an Exchange mailbox database on another Datacenter 
We had an issue today where the databases from the Disaster Recovery (DR) datacenter would not activate because the "Active MAnager" tried to join the server in the failed datacenter and couldn't.
Also, the last status of the database index viewed by the Active Manager was "Failed" as well.
The trick was to use the `Move-ActiveMailboxDatabase` with the `-SkipClientExperienceChecks` property to skip the Failed index preventing the database to activate, and the `-MountDialOverride GoodAvailability` property to skip the fact that some log files were not synchronized from the failed datacenter to the DR datacenter.

- Here's the full command line we used to activate the database `Database_Name` on server `Server_Name` :

```powershell

Move-ActiveMailboxDatabase Database_Name\Server_Name -SkipClientExperienceChecks -MountDialOverRide GoodAvailability 

```

## Checking the database status
We then ran the following command line to check that the database is indeed mounted on the server we want:

```powershell

Get-MailboxDatabase -Status | Ft Name,MountedOnServer, Mounted

```

Here we should see under the MountedOnServer one of the servers from the DR datacenter, and under the Mounted status the “$True” value.
