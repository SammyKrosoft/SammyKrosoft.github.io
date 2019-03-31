---
layout: post
title: Exchange 2003 to Exchange 2010 Intra-Organization migration
date: 2011-09-23 07:24
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<table style="width:565px;" border="1" cellspacing="1" cellpadding="0">
<tbody>
<tr>
<td valign="top"></td>
<td></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Exchange 2003 to Exchange 2010 migration path</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Step</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Step Title</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Details (migration from Exchange 2003)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 1</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Certificate</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Acquire new commercial certificate for external client coexistence with CAS 2010 (legacy.contoso.ca namespace required for former CAS2007 for silent redirect). <br> It will neet at minimum 3 SAN values : <br> <b>- mail.contoso.ca (primary OWA/EAS/OA URL) <br> - autodiscover.contoso.ca <br> - legacy.contoso.ca (OWA/EAS legacy mailbox access)</b> <br> Subject name should be mail.contoso.ca, should match the "Certificate Principal Name" in Outlook profile for Outlook Anywhere</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 2</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Service pack level and AD version</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Use ExPDA (<b><i><span style="text-decoration:underline;">http://www.microsoft.com/downloads/en/details.aspx?FamilyID=88B304E7-9912-4CB0-8EAD-7479DAB1ABF2</span></i></b>) to : <br> a- Ensure all Exchange 2003 servers are SP2 <br> b- Ensure all Forest/Domain match Exchange 2010 prerequisites <br> Note : One of the prerequisites is the link state minor changes suppression. The procedure to deactivate it is on Step 8.1</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Prepare AD</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3.1.1</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Preparing Legacy Exchange Permissions (Exchange 2003 coexistence only)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>1.1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Setup /PrepareLegacyExchangePermissions </b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>1.2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Wait AD changes to replicate-track the progress using REPLMON tool</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3.1.2</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Preparing schema</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>2.1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Run on a DC in schema master domain (forest root domain) <br> Memberships required: Schema Admins group &amp; Enterprise Admins</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>2.2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Setup /PrepareSchema (Schema Admins group + Enterprise Admins)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>2.3</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Wait AD changes to replicate-track the progress using REPLMON tool</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3.1.3</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>3</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Preparing AD</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>3.1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Run on a DC in schema master domain (forest root domain) <br> Memberships required: Enterprise Admins</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>3.2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Setup /PrepareAD (Enterprise Admins)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>3.3</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Wait AD changes to replicate-track the progress using REPLMON tool</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3.1.4</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>4</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Preparing All Domains</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>4.1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Setup /PrepareAllDomains or setup /pad (Enterprise Admins)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>4.2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Verify the AD changes</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 3.1.5</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>5</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Repeatable Task:Preparing Domains (If Preparing All Domains not run-See notes)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>5.1</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>If Preparing All Domains task was run, skip this task</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>5.2</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Memberships required: <br> If domain created BEFORE prepareAD: <span style="text-decoration:underline;">Domain Admins</span> <br> If domain created AFTER prepareAD: <span style="text-decoration:underline;">Domain Admins &amp; Exchange Organization Administrators</span></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>5.3</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Setup /PrepareDomain (see notes)</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>5.4</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Verify the AD changes</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 4</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install CAS2010</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install CAS 2010 and configure it</b> <br> - specify the external namespace (e.g. mail.contoso.ca) <br> - install RPC over HTTP proxy component <br> - Configure OWA settings (FBA vs Basic auth) <br> - Configure EAS authentication (Basic vs Certificate Auth) <br> - Enable OA (Enable-OutlookAnywhere -Server:ServerName -ExternalHostName:mail.contoso.ca -SSLOffLoading $false)</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 4.1</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Configure CAS2010 ExternalURLS</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>If the Step 4 above - enter the external namespace - has not been done, configure it on this step:</b> <br> &bull;Offline Address Book: <b>Set-OABVirtualDirectory OAB* -ExternalURL https://mail.contoso.com/OAB</b> <br> &bull;Web Services: <b>Set-WebServicesVirtualDirectory EWS* -ExternalURL https://mail.contoso.com/ews/exchange.asmx</b> <br> &bull;ActiveSync: <b>Set-ActiveSyncVirtualDirectory -Identity Microsoft-Server-ActiveSync -ExternalURL https://mail.contoso.com/Microsoft-Server-ActiveSync</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 5</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Configure CAS2010 OWA</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Configure OWA on CAS2010 appropriately : <br> - <b>Outlook Web Access</b>: <br> . For environments with Exchange 2003 mailbox servers: Set-OWAVirtualDirectory OWA* -ExternalURL https://mail.contoso.com/OWA -<b>Exchange2003URL https://legacy.contoso.com/exchange</b> <br> - Exchange Control Panel : <br> . <b>Set-ECPVirtualDirectory ECP* -ExternalURL https://mail.contoso.com/ECP</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 7</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Configure CAS2010 Array (LB + DNS + PowerShell)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>For Outlook clients, you can configure the CAS2010 servers as an RPC Client Access Service array, which is highly recommended even if you have only one CAS2010 you start with : <br> - Create a LB array for CAS2010 (Step-by-step guide / installation template : <b><i><span style="text-decoration:underline;">http://technet.microsoft.com/en-us/library/cc742379(EXCHG.80).aspx</span></i></b>) <br> - Create a DNS entry in your<b> INTERNAL DNS</b> that resolves the<b> LB Array name</b> to the V<b>irtual IP Address (VIP) </b>of the CAS LB (e.g. <b>CASArray01.Contoso.ca</b>) <br> - Configure the LB array to load-balance the MAPI RPC ports : TCP 135 and TCP 1024 - 65 535 <br> - Create the CAS Array AD object : <b>New-ClientAccessArray -Name CASArray01.contoso.ca -FQDN CASArray01.contoso.ca -Site "Site1"</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 8</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install HUB2010 and MBX2010</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Install the HUB2010 and MBX2010 roles in the "Internet Facing AD Site" and configure them: <br> - You can change the OAB generation server and enable Web Distribution on CAS2010 : <br> &gt; Move the OAB : <b>Move-OfflineAddressBook "Default Offline Address List" -Server Server_Name</b> <br> &gt; add the CAS2010 server as a Web Distribution Point : <br> <b>&#9632;$OABVDir=Get-OABVirtualDirectory -Server <br> &#9632;$OAB=Get-OfflineAddressBook "Default Offline Address List" <br> &#9632;$OAB.VirtualDirectories += $OABVdir.DistinguishedName <br> &#9632;Set-OfflineAddressBook "Default Offline Address List" -VirtualDirectories $OAB.VirtualDirectories</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 8.1</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Suppress Link State Updates on Exchange 2003 </b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>HKEY_LOCAL_MACHINESystemCurrentControlSetServicesRESvcParameters</b> <br> 1. Right-click Parameters and select New | DWORD value. Name the new DWORD value <b>SuppressStateChanges.</b> <br> 2. Double-click <b>SuppressStateChanges.</b> <br> 3. In the Value data field, enter 1.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 9</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>External DNS : create legacy.contoso.ca namespace</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Create the legacy host record (<b>legacy.contoso.com</b>) in your <b>EXTERNAL DNS</b> infrastructure and associate it either with the <b>FE2003</b> infrastructure (less likely) or your <b>proxy</b> <b>infrastructure</b> (more likely).</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 10</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Environment without Exchange 2007 autodiscover</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>You will configure<b> EXTERNAL DNS</b> and/or your<b> reverse proxy infrastructure's publishing rules</b> to have the<b> autodiscover.contoso.com</b> namespace point to CAS2010</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Publish legacy.contoso.ca to point to CAS2007 on reverse proxy</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>If utilizing a reverse proxy infrastructure, you will publish the<b> legacy namespace to the FE2003</b> infrastructure so that at this point the <b>FE2003 infrastructure</b> can be accessed either via <b>mail.contoso.com</b> or <b>legacy.contoso.com</b> namespaces</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11.1</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Autodiscover setting to use NLB</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Set CAS property <b>AutoDiscoverServiceInternalUri</b> to the Load balanced FQDN : <br> <b>Get-ClientAccessServer | Set-ClientAccessServer -AutoDiscoverServiceInternalUri:"https://loadbalancerFQDN/Autodiscover/Autodiscover.xml" <br> Get-AutodiscoverVirtualDirectory | set-AutodiscoverVirtualDirectory -InternalUrl:"https://loadbalancerFQDN/Autodiscover/Autodiscover.xml"</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11.2</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Internet connectivity downtime (reconfigure 2003 FEs URLs to legacy.contoso.ca)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Schedule the internet protocol client downtime (should be small enough time to make change and validate the configurations) during which we'll perform the following steps : <br> - Reconfigure <b>External DNS</b> and/or your <b>reverse proxy infrastructure's publishing rules</b> to have the <b>mail.contoso.com </b>namespaces point to CAS2010.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11.3 <br> (Exchange 2003 EAS)</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Internet connectivity downtime (for Exchange 2003 EAS clients)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Exchange 2003 mailboxes : EAS client will try to connect through CAS2010 and will receive an error. <br> SOLUTION : enable Integrated Windows Authentication on the Microsoft-Server-ActiveSync vDir on Exchange 2003.</b> <br> &gt; Install <b><i><span style="text-decoration:underline;">http://support.microsoft.com/?kbid=937031</span></i></b> and then <b>use the Exchange System Manager to adjust the authentication settings of the ActiveSync virtual directory.</b> <br> &gt; OR, set the <b>msExchAuthenticationFlags </b>attribute to a value of <b>6 </b>on the <b>Microsoft-Server-ActiveSync </b>object within the <b>configuration container on each Exchange 2003 mailbox server. </b>An example script is provided at http://technet.microsoft.com/en-us/library/cc785437.aspx. <br> <b><span style="text-decoration:underline;">Note:</span></b> DO NOT use IIS Manager to change the authentication setting on the Microsoft-Server-ActiveSync virtual directory as the DS2MB process within the System Attendant will overwrite the settings that are stored in Active Directory.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11.4 <br> (OA)</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Internet connectivity downtime (disable Exchange 2007 OA)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="bottom"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Disable Outlook Anywhere by utilizing the Exchange System Manager and selecting the "<i>Not part of an Exchange managed RPC-HTTP topology</i>" radial button <b>on the RPC-HTTP tab of the Front-End server's properties</b>. <br> Optionally, you can also remove the RPC over HTTP proxy component (refer to your Windows Server documentation for more information). <br> <b><i><span style="text-decoration:underline;">Important:</span></i></b> This requires an up-front investment in CAS2010 architecture as all Outlook Anywhere clients will utilize CAS2010 once you transition the Outlook Anywhere endpoint. Be sure to follow all proper scalability planning documentation when deploying CAS2010 to ensure that you do not create a bottleneck in your CAS infrastructure due to Outlook Anywhere clients.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 11.6 <br> (test)</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Test</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Test all client scenarios</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 12</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Production launch</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p>Complete downtime and enable Internet protocol client usage</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 13</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>HUB</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install the Hub Transport server role</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 14</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>UM (optional)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install the Unified Messaging server role <br> </b>This step is optional. It's only necessary if you want to use Unified Messaging in your organization.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 15</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>UM Config (Optional)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Configure Unified Messaging <br> This step is optional. It's only necessary if you want to use Unified Messaging in your organization. </b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 16</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>MAILBOX</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install the Mailbox server role</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 17</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>EDGE (Optional)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Install the Edge Transport server role <br> </b>This step is optional. It's only necessary if you want to use the Edge server role in your organization.</p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 18</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Internet Mail Flow configuration</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Move Internet mail flow from Exchange 2003 to Exchange 2010 (DNS MX records changes and SMTP forwarder change - Ironport, ISS or whichever SMTP smarthost server is used in the perimeter network)</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i>Step 19</i></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Move Mailbox</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><b>Move mailboxes from Exchange 2003 to Exchange 2010</b></p>
<span style="font-family:Times New Roman;font-size:medium;"> </span></td>
</tr>
<tr>
<td valign="top"><span style="font-family:Times New Roman;font-size:medium;"> </span>
<p><i></i></p></td></tr></tbody></table>

