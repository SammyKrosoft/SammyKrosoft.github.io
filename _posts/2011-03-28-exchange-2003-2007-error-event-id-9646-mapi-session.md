---
layout: post
title: Exchange 2010/2013/2016 Event ID 9646 errors - multiple causes from network dropping to application scanning/downloading mails, attachments, folders too many times
date: 2011-03-28 10:50
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
9646 Exchange errors have different causes that can be narrowed down depending on the description of these errors :
<ul>
 	<li><em>9646 Type #1 error:</em> MAPI session &lt;GUID: DN of account&gt; exceeded the maximum of xxx objects of type objt&lt;object type&gt;, where objt&lt;Object type&gt; can be anything on the below:
<ul>
 	<li>objtMessage</li>
 	<li>objtFolder</li>
 	<li>objtAttachment</li>
 	<li>objtFolderView</li>
 	<li>objtMessageView</li>
 	<li>objtAttachView</li>
 	<li>objtStream</li>
 	<li>objtACLView</li>
 	<li>objtRulesView</li>
 	<li>objtFXSrcStrm</li>
 	<li>objtFXDstStrm</li>
 	<li>objtCStream</li>
 	<li>objtNotify</li>
</ul>
</li>
</ul>
&nbsp;
<ul>
 	<li><span><em>9646 Type #2 error:</em> Mapi session "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx: /o=MYDOMAIN/ou=First Administrative Group/cn=Recipients/cn=MYUSER" exceeded the maximum of 32 objects of type "session".</span></li>
</ul>
&nbsp;

&gt; <strong><span style="text-decoration: underline">9646 type #1 error</span></strong> : If the MAPI 9646 error refers to the first type (with "objt&lt;object type&gt;") in its description, then it can be:
<ul>
 	<li>a user who opened multiple Outlooks, likely in cached mode, that downloads the mailbox on his local workstation: that opens Folder, Message and Attachments objects simultaneously, which can meet the default limits on these</li>
 	<li>a desktop search or indexing engine that scans the mailbox : that opens all folders, messages and attachments potentially, meeting the limits and triggering this error</li>
 	<li>another application that scans all messages and folders (like BES5 agent, or any other mail related software, or a desktop antivirus that is set to scan Outlook application's messages)</li>
 	<li>...</li>
</ul>
&nbsp;

&gt;<span style="text-decoration: underline"> <strong>9646 type #2 error</strong> :</span> If the MAPI 9646 error refers to the second type (with exceeded the max objects of type "Session"), then this issue may go out by itself. But if the user is still experiencing the issue, try to close the IP connections, and investigate what on his desktop (or on other desktops if he has multiple opened connections) can generate too much MAPI connections.

Usually, it can be caused by VPN connections closing and users re-opening them. When a VPN connection closes, the MAPI session to an Exchange Server is not released. Then re-establighing the VPN then the MAPI connection just add more MAPI connections

It can also be caused by an external MAPI based device such as Blackberry or application such as scripts or third-party mail applications.

&nbsp;

<span style="text-decoration: underline"><strong>What are the limits ? Can we change them ?</strong></span>

For the <strong>Type #1 error,</strong> you can change the limits in the number of objects that can be opened by a single mailbox in a single session can be done, but you have to use it with care : we have to understand first why the user(s) are opening more than the default limit of items or sessions - there might be a reason, and if it's known, then we can raise the limits. If not, we need to investigate first why some users / application are trying to open more than the limited number of items in a single session or more than the limited number of sessions.

For the <strong>Type #2 error,</strong> same thing, investigate what's happening with the user first, then you can raise the nb of "sessions" limits using <a href="https://technet.microsoft.com/en-us/library/dd298094(v=exchg.141).aspx">Set-ThrottlingPolicy</a> and changing the <code>RCAMaxConcurrency parameter.</code><span></span>

&nbsp;

For information, here are the limits on Exchange 2010/2013/2016 (for the above "type #1" error):
<table summary="table">
<tbody>
<tr>
<th colspan="1" scope="col">Item type</th>
<th colspan="1" scope="col">Registry object type</th>
<th colspan="1" scope="col">Max opened per session</th>
</tr>
<tr>
<td colspan="1">ACL View</td>
<td colspan="1">objtACLView</td>
<td colspan="1">50</td>
</tr>
<tr>
<td colspan="1">Attachment</td>
<td colspan="1">objtAttachment</td>
<td colspan="1">500</td>
</tr>
<tr>
<td colspan="1">Attachment View</td>
<td colspan="1">objtAttachmentView</td>
<td colspan="1">500</td>
</tr>
<tr>
<td colspan="1">Cstream</td>
<td colspan="1">objtCStream</td>
<td colspan="1">50</td>
</tr>
<tr>
<td colspan="1">Folder</td>
<td colspan="1">objtFolder</td>
<td colspan="1">500</td>
</tr>
<tr>
<td colspan="1">Folder View</td>
<td colspan="1">objtFolderView</td>
<td colspan="1">500</td>
</tr>
<tr>
<td colspan="1">FX Destination Stream</td>
<td colspan="1">objtFXDstStrm</td>
<td colspan="1">50</td>
</tr>
<tr>
<td colspan="1">FX Source Stream</td>
<td colspan="1">objtFXSrcStrm</td>
<td colspan="1">50</td>
</tr>
<tr>
<td colspan="1">Message</td>
<td colspan="1">objtMessage</td>
<td colspan="1">250</td>
</tr>
<tr>
<td colspan="1">Message View</td>
<td colspan="1">objtMessageView</td>
<td colspan="1">500</td>
</tr>
<tr>
<td colspan="1">Notification</td>
<td colspan="1">objtNotify</td>
<td colspan="1">500,000</td>
</tr>
<tr>
<td colspan="1">Rule View</td>
<td colspan="1">objtRulesView</td>
<td colspan="1">50</td>
</tr>
<tr>
<td colspan="1">Stream</td>
<td colspan="1">objtStream</td>
<td colspan="1">250</td>
</tr>
</tbody>
</table>
As per the <a href="https://technet.microsoft.com/en-us/library/ff477612(v=exchg.141).aspx">Exchange Store Limits TechNet article</a>...
<div><a class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="LW_CollapsibleArea_Title"><strong>Configure Open Item Limits</strong>:  </span></a>You can limit the maximum number of resources that a MAPI client can use simultaneously. It's done using Registry, please see the above mentioned article for more information about how to modify</div>
&nbsp;

<span style="text-decoration: underline"><strong>But some investigations have to be made first before increasing the above limits:</strong></span>

1- First, checkout the IP address of UserAlias listed in the Distinguished Name of the event :

<strong>Get-LogonStatistics UserAlias | ft Name,ClientIPAddress</strong>

Note: if the client is not more connected to the Exchange server or the MAPI sessions finally closed by themselves, the ClientIPAddress property will be blank

&nbsp;

2- Using a tool like <a target="_blank" href="http://technet.microsoft.com/en-us/sysinternals/bb897437.aspx" rel="noopener">TCPView</a> (<a href="http://technet.microsoft.com/en-us/sysinternals/bb897437.aspx" title="http://technet.microsoft.com/en-us/sysinternals/bb897437.aspx">http://technet.microsoft.com/en-us/sysinternals/bb897437.aspx</a>), close the connections from your Exchange server that refer to the ClientIPAddress you just get with the PowerShell command in Step-1

&nbsp;

&nbsp;

Again, forcing TCP connections without knowing the cause of what caused them is not very clever. The MAPI connections exhaustion may come back.

Check with the user’s devices and applications he is using to connect to the Exchange server as you may be able to solution the issue for a broader number of users.
