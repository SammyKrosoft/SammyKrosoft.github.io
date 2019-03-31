---
layout: post
title: Autodiscover test tool - (c) from Kip Ng
date: 2018-05-28 11:58
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Hi all,

I was looking for a tool or a script that would help me to test the Exchange Autodiscover process a bit like the <a target="_blank" href="https://testconnectivity.microsoft.com/" rel="noopener">Microsoft Remote Connectivity Analyzer</a> would do, but standalone and more dedicated to Exchange or O365 (Exchange Online) Autodiscover process. And I found this very practical, simple to use, useful <a target="_blank" href="https://blogs.technet.microsoft.com/provtest/2010/08/13/exchange-server-2010-sp1-beta-hosting-deployment-part-9-autodiscover/" rel="noopener">Autodiscover test tool</a> that Mr <a target="_blank" href="https://social.technet.microsoft.com/profile/Kip+Ng" rel="noopener">Kip Ng</a> created for us, thanks Kip ! <em>(Disclaimer: Kip is now my manager, but it's not the reason I'm buttering him up or backslapping: I'm just mentionning his work on this tool because it helped me more than once. However, I also have to mention that he's a handsome guy and a great manager :-p )</em>

As a reminder, Autodiscover process is the very first step that E-mail clients like Outlook perform when a user first launch it and/or tries to connect to his mailbox for the first time. See more information about <a target="_blank" href="https://technet.microsoft.com/en-us/library/bb124251(v=exchg.150).aspx?f=255&amp;mspperror=-2147217396" rel="noopener">Autodiscover here</a>. The process has not changed much since Exchange 2007...

This Autodiscover test tool works very well for Exchange 2010, 2013, 2016, as well as Exchange Online (O365) !

I won't write a lot about this tool as Mr. Kip Ng already describes it well on <a target="_blank" href="https://blogs.technet.microsoft.com/provtest/2010/08/13/exchange-server-2010-sp1-beta-hosting-deployment-part-9-autodiscover/" rel="noopener">his blog post</a>, but basically it will use the same process that Outlook (2010 -&gt; 2016) is using to find a user's mailbox connection settings, querying Autodiscover just like an Outlook client would do, and showing the results (the discovered URLs for OWA, ECP, EWS, UM, ...) on the command line window.

Here are the links on both his blog post and a direct download link for convenience:
<ul>
 	<li><a target="_blank" href="https://blogs.technet.microsoft.com/provtest/2010/08/13/exchange-server-2010-sp1-beta-hosting-deployment-part-9-autodiscover/" rel="noopener">Kip Ng's blog post about Autodiscover and the tool he created</a> (even if it's for Exchange Hosted, it works for all mailboxes hosted on Exchange 2010-&gt; 2016 onprem, as well as mailboxes hosted on Exchange Online in O365)</li>
 	<li><a target="_blank" href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/telligent.evolution.components.attachments/01/6982/00/00/03/34/95/63/AutodiscoverTest.zip" rel="noopener">Direct download link to the Autodiscover test tool (Kip's Blog space)</a></li>
 	<li><a target="_blank" href="https://gallery.technet.microsoft.com/office/Autodiscover-test-tool-by-3393409b" rel="noopener">Alternate download link (TechNet Gallery)</a></li>
</ul>
&nbsp;

Here's an execution sample on my E2010 environment
<blockquote>Note: I tested on Exchange 2016 as well as on my corporate Exchange Online mailbox -&gt; all work good</blockquote>
<pre>C:\&gt;<strong>AutodiscoverTest.exe -email:User1@E2010Domain.com -username:User1@E2010Domain.com</strong>

Autodiscover Testing Utility:

AutodiscoverTest.exe -email:&lt;emailaddress&gt; [-allowselfsigned:{true|false}] [-username:&lt;username&gt;] [-password:&lt;password&gt;] [-url:&lt;autdiscover url&gt;]

where:

emailAddress - smtp address to autodiscover
 true|false - allow self signed certificates, default - false
 username - user name for https: requests authentication, eg. &lt;domain\user&gt; or
UPN
 password - user password for https: requestsauthentication
 url - specify a specific url for autodiscover test

If username/password/domain are not specified, default credentials are used.

----------------------------

Password not defined!
Please enter your password: ********

..starting Autodiscover test for 'User1@E2010Domain.com'
..using the specified credentials for HTTPS
..username: User1@E2010Domain.com
..domain:
..verifying E-mail address.
..starting SCP Lookup for domainName=E2010Domain.com
..Scanning for SCP urls for the current computer Site=SITE2
..SCP Url with other Site keyword found 'https://mail.e2010domain.com/autodiscover/autodiscover.xml'
..trying 'User1@E2010Domain.com' at 'https://mail.e2010domain.com/autodiscover/autodiscover.xml'

User/DisplayName=User1
User/LegacyDN=/o=E2010ORG/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Recipients/cn=User14a8User/DeploymentId=37c0a5c5-1928-4ce9-a03d-e0b0e66cf138
Account/AccountType=email
Account/Action=settings
Account/Protocol/Type=EXCH
Account/Protocol/ASUrl=https://mail.e2010domain2.com/ews/exchange.asmx
Account/Protocol/DirectoryPort=0
Account/Protocol/MdbDN=/o=E2010ORG/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Configuration/cn=Servers/cn=Outlook.contoso.com/cn=Microsoft Private MDBAccount/Protocol/OABUrl=https://mail.e2010domain2.com/oab/ef0acab4-4923-499b-86ed-413979af4c74/
Account/Protocol/OOFUrl=https://mail.e2010domain2.com/ews/exchange.asmx
Account/Protocol/Port=0
Account/Protocol/ReferralPort=0
Account/Protocol/Server=Outlook.contoso.com
Account/Protocol/ServerDN=/o=E2010ORG/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Configuration/cn=Servers/cn=Outlook.contoso.com
Account/Protocol/ServerVersion=7383807B
Account/Protocol/UMUrl=https://mail.e2010domain2.com/ews/UM2007Legacy.asmx
Account/Protocol/PublicFolderServer=E2010-NODE2.E2010Domain.com
Account/Protocol/AD=E2010ENV-GC1.E2010Domain.com
Account/Protocol/EwsUrl=https://mail.e2010domain2.com/ews/exchange.asmx
Account/Protocol/EcpUrl=https://mail.e2010domain2.com/ecp/
Account/Protocol/EcpUrl-um=?p=customize/voicemail.aspx&amp;exsvurl=1
Account/Protocol/EcpUrl-aggr=?p=personalsettings/EmailSubscriptions.slab&amp;exsvurl=1
Account/Protocol/EcpUrl-mt=PersonalSettings/DeliveryReport.aspx?exsvurl=1&amp;IsOWA=&lt;IsOWA&gt;&amp;MsgID=&lt;MsgID&gt;&amp;Mbx=&lt;Mbx&gt;
Account/Protocol/EcpUrl-ret=?p=organize/retentionpolicytags.slab&amp;exsvurl=1
Account/Protocol/EcpUrl-sms=?p=sms/textmessaging.slab&amp;exsvurl=1
Account/Protocol/Type=EXPR
Account/Protocol/AuthPackage=Basic
Account/Protocol/DirectoryPort=0
Account/Protocol/OABUrl=https://mail.e2010domain2.com/oab/ef0acab4-4923-499b-86ed-413979af4c74/
Account/Protocol/Port=0
Account/Protocol/ReferralPort=0
Account/Protocol/Server=mail.e2010domain.com
Account/Protocol/SSL=On
Account/Protocol/Type=WEB
Account/Protocol/DirectoryPort=0
Account/Protocol/Port=0
Account/Protocol/ReferralPort=0
Account/Protocol/Internal/OWAUrl[@AuthenticationMethod="Basic, Ntlm, Fba, WindowsIntegrated"]=https://mail.e2010domain2.com/owa/
Account/Protocol/Internal/Protocol/Type=EXCH
Account/Protocol/Internal/Protocol/ASUrl=https://mail.e2010domain2.com/ews/exchange.asmx</pre>
