---
layout: post
title: Exchange 2010/2013/2016 and Exchange Online – List which RBAC role group / role the current logged user has
date: 2017-11-01 13:07
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Hi all !

Another quick post – I post mostly to be able to refer it back when I’ll need it and also when I suspect I’ll need it quite often in the near future and also to let you guys know of course – now this post is about listing which RBAC Roles does the currently PowerShell logged user have.

There are 2 parts here:

- one with the command line to get the <strong>roles assigned to the current user that is local to the computer</strong>, and how it’s been assigned to him (whether it’s been assigned to the connected user using a Role Group aka Role Group assignment type, or using a RBAC Policy, or using direct role assignment, etc…)

- one with the cmdlet to get the <strong>roles assigned to the current user that opened a remote PowerShell session, like a remote session on Exchange Online</strong> (<a href="https://technet.microsoft.com/en-us/library/jj984289(v=exchg.160).aspx">see here about how to connect to your O365 tenant</a>)

&nbsp;
<h1><span style="font-size: xx-small"> </span>Part I – Getting the list of role assignments and the type of each of these role assigments for the current computer local user</h1>
NOTE: you must be in an Exchange Management Shell to be able to run the below command, or at least a <a href="https://technet.microsoft.com/en-us/library/dd297932(v=exchg.141).aspx">PowerShell session of an Exchange server that you imported to your local Powershell session</a>…
<p class="MsoNormal" style="background: #012456;margin: 0cm 0cm 0pt;line-height: normal"><span style="font-family: 'Lucida Console'"><span style="font-size: small"><span><span style="color: #e0ffff">Get-ManagementRoleAssignment –</span></span><span><span><span style="color: #ffe4b5">RoleAssignee </span></span><span style="color: #000000"></span><span><span style="color: #ff4500">$env:username </span></span><span style="color: #000000"></span><span><span style="color: #d3d3d3">| </span></span><span style="color: #000000"></span><span><span style="color: #e0ffff">Group </span></span><span><span style="color: #ffe4b5">-Property </span></span><span><span style="color: #ee82ee">RoleAssigneeName, </span></span><span><span style="color: #ee82ee">RoleAssigneeType </span></span><span style="color: #000000"></span><span><span style="color: #d3d3d3">| </span></span><span style="color: #000000"></span><span><span style="color: #e0ffff">Sort </span></span><span style="color: #000000"></span><span><span style="color: #ee82ee">Name </span></span><span style="color: #000000"></span><span><span style="color: #d3d3d3">| </span></span><span style="color: #000000"></span><span><span style="color: #e0ffff">ft </span></span><span style="color: #000000"></span><span><span style="color: #ee82ee">Name –</span></span><span><span style="color: #ffe4b5">a</span></span></span></span></span></p>
this is what you get for example:

<span style="font-family: 'Lucida Console'">Name
----
Default Role Assignment Policy, RoleAssignmentPolicy
Discovery Management, RoleGroup
Organization Management, RoleGroup</span>

The above mean that <span style="font-family: 'Courier New'">$env:username</span>, which is the locally logged on user (or the user which launched the PowerShell session using “Run as another user…”) has roles assigned by the “<strong>Default Role Assignment Policy</strong>” that is applied to him, which is a "Role Assignment Policy", as well as two "Role Groups" assigned to him: the <strong>Discovery Management</strong> role group and the<strong> Organization Management</strong> role group.

Then you can see which Management Roles are in each of the above, for example by running the following:
<p class="MsoNormal" style="background: #012456;margin: 0cm 0cm 0pt;line-height: normal"><span style="font-family: 'Lucida Console'"><span style="font-size: small"><span><span style="color: #e0ffff">get-rolegroup </span></span><span><span style="color: #000000"></span><span><span style="color: #db7093">"Organization Management" </span></span><span style="color: #000000"></span><span><span style="color: #d3d3d3">| </span></span><span style="color: #000000"></span><span><span style="color: #e0ffff">select</span></span><span style="color: #000000"></span><span><span style="color: #ffe4b5">-ExpandProperty </span></span><span style="color: #000000"></span><span><span style="color: #ee82ee">roles</span></span></span></span></span></p>
this is what you’ll get for example for  Organization Management:

<span style="font-family: 'Lucida Console'">Active Directory Permissions</span>

<span style="font-family: 'Lucida Console'">Address Lists</span>

<span style="font-family: 'Lucida Console'">ApplicationImpersonation</span>

<span style="font-family: 'Lucida Console'">Audit Logs</span>

<span style="font-family: 'Lucida Console'">Cmdlet Extension Agents</span>

<span style="font-family: 'Lucida Console'">Database Availability Groups</span>

<span style="font-family: 'Lucida Console'">Database Copies</span>

<span style="font-family: 'Lucida Console'">Databases</span>

<span style="font-family: 'Lucida Console'">Disaster Recovery</span>

<span style="font-family: 'Lucida Console'">etc…</span>

(note: the “etc…” is not a management role …)

&nbsp;
<h1>Part II – Getting the list of role assignments and the type of each of these for a user connected on Exchange Online</h1>
NOTE: you must be connected to an Exchange Online tenant prior to be able to run the below commands - <a href="https://technet.microsoft.com/en-us/library/jj984289(v=exchg.160).aspx">see here about how to connect to your O365 tenant</a>

First get the current user name (see my friend <a href="https://blogs.technet.microsoft.com/rmilne/2016/09/19/remote-powershell-pssession-whoami/">Rhoderick’s post for more information about it</a> – you may specify a specific PSSession if you have more than one session within your currently opened Powershell console)
<p class="MsoNormal" style="background: #012456;margin: 0cm 0cm 0pt;line-height: normal"><span style="font-family: 'Lucida Console'"><span style="font-size: small"><span><span style="color: #ff4500">$CurrentExOUser</span></span><span><span style="color: #000000"></span><span><span style="color: #d3d3d3">=</span></span><span style="color: #000000"></span><span><span style="color: #f5f5f5">(</span></span><span><span style="color: #e0ffff">Get-PSSession</span></span><span><span style="color: #f5f5f5">)</span></span><span><span style="color: #d3d3d3">.</span></span><span><span style="color: #f5f5f5">Runspace</span></span><span><span style="color: #d3d3d3">.</span></span><span><span style="color: #f5f5f5">OriginalConnectionInfo</span></span><span><span style="color: #d3d3d3">.</span></span><span><span style="color: #f5f5f5">Credential</span></span><span><span style="color: #d3d3d3">.</span></span><span><span style="color: #f5f5f5">UserName</span></span></span></span></span></p>
Then call the same command as Part I except that you now specify the <span style="font-family: 'Courier New'">$CurrentExOUser</span> variable we provisioned above for the <span style="font-family: 'Courier New'">“-RoleAssignee</span>” parameter:

<span style="font-size: small;font-family: 'Lucida Console';background-color: #012456"><span style="color: #e0ffff">Get-ManagementRoleAssignment –</span></span><span style="font-size: small;font-family: 'Lucida Console';background-color: #012456"><span style="color: #ffe4b5">RoleAssignee </span><span style="color: #000000"></span><span style="color: #ff4500">$CurrentExOUser </span><span style="color: #000000"></span><span style="color: #d3d3d3">| </span><span style="color: #000000"></span><span style="color: #e0ffff">Group</span><span style="color: #000000"></span><span style="color: #ffe4b5">-Property </span><span style="color: #000000"></span><span style="color: #ee82ee">RoleAssigneeName</span><span style="color: #d3d3d3">,</span><span style="color: #ee82ee">RoleAssigneeType </span><span style="color: #000000"></span><span style="color: #d3d3d3">| </span><span style="color: #000000"></span><span style="color: #e0ffff">Sort </span><span style="color: #000000"></span><span style="color: #ee82ee">Name </span><span style="color: #000000"></span><span style="color: #d3d3d3">| </span><span style="color: #000000"></span><span style="color: #e0ffff">ft </span><span style="color: #000000"></span><span style="color: #ee82ee">Name </span><span style="color: #000000"></span><span style="color: #ffe4b5">-a</span></span>

this is what you get for example:

<span style="font-family: 'Lucida Console'">Name</span>

<span style="font-family: 'Lucida Console'">----</span>

<span style="font-family: 'Lucida Console'">Organization Management, RoleGroup</span>

And the above means that the user who has opened the local session has the “Organization Management” assignment, which is a Role Group.

&nbsp;
<h1>References</h1>
RBAC examples and tips and how to retrieve cmdlets and where they sit, etc… again from my friend Rhod:

<a href="https://blogs.technet.microsoft.com/rmilne/2014/02/18/exchange-rbac-tips-n-tricks-powershell/" title="https://blogs.technet.microsoft.com/rmilne/2014/02/18/exchange-rbac-tips-n-tricks-powershell/">https://blogs.technet.microsoft.com/rmilne/2014/02/18/exchange-rbac-tips-n-tricks-powershell/</a>

&nbsp;

Here’s Rhod’s post about the Get-PSSession to get the remotely logged on user name

<a href="https://blogs.technet.microsoft.com/rmilne/2016/09/19/remote-powershell-pssession-whoami/" title="https://blogs.technet.microsoft.com/rmilne/2016/09/19/remote-powershell-pssession-whoami/">https://blogs.technet.microsoft.com/rmilne/2016/09/19/remote-powershell-pssession-whoami/</a>

&nbsp;

And here’s my post about the basics of RBAC if you need

<a href="https://support.office.com/en-us/article/PowerShell-for-Office-365-administrators-40fdcbd4-c34f-42ab-8678-8b3751137ef1" title="https://support.office.com/en-us/article/PowerShell-for-Office-365-administrators-40fdcbd4-c34f-42ab-8678-8b3751137ef1">https://support.office.com/en-us/article/PowerShell-for-Office-365-administrators-40fdcbd4-c34f-42ab-8678-8b3751137ef1</a>

&nbsp;

RBAC Manager tool – a more graphic tool to help manage RBAC roles

<a href="https://rbac.codeplex.com/" title="https://rbac.codeplex.com/">https://rbac.codeplex.com/</a>
