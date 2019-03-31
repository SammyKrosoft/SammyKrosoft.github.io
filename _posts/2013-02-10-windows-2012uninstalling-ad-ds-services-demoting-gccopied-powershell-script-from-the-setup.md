---
layout: post
title: Windows 2012–Uninstalling AD DS services (demoting GC)–copied Powershell script from the setup
date: 2013-02-10 19:02
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>#   <br># Windows PowerShell script for AD DS Deployment    <br>#</p><p>Import-Module ADDSDeployment   <br>Uninstall-ADDSDomainController `    <br>-DemoteOperationMasterRole:$true `    <br>-IgnoreLastDnsServerForZone:$true `    <br>-LastDomainControllerInDomain:$true `    <br>-RemoveApplicationPartitions:$true `    <br>-Force:$true</p></p>

