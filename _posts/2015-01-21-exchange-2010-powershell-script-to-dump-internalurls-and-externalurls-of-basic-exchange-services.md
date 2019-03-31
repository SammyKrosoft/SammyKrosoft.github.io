---
layout: post
title: Exchange 2007, 2010 and 2013 – Powershell script to dump InternalURLs and ExternalURLs of basic Exchange services
date: 2015-01-21 08:39
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<strong><span style="text-decoration:underline;">Update</span>: added a slightly modified version of the script to dump these way quicker&nbsp;by using the -ADPropertiesOnly switch along with the <strong>Get-WhicheverVirtualDirectory </strong>commandlets - this switch doesn't work on Exchange 2007, only works on Exchange 2010 and 2013 (and hopefully Exchange 2016 ;-)) and only reads the URLs in the local GC instead of reading these in each server's IIS "metabase".</strong>



Hey all,

&nbsp;

Here is a script to dump the Internal and External URL properties from the main Exchange services.

This script has been designed and tested to work out of the box on an Exchange 2010 environment, but it may work on Exchange 2007 and Exchange 2013 as well.

Why these properties ? Because on many engagements, I found that these URLs were not properly set, leading to users or servers latencies, performance issues, proxy or redirection not working between Active Directory sites, &hellip;

&nbsp;

Look at the URL configuration tables on the below link, which are Microsoft&rsquo;s recommendations to set correctly your URLs whether you have a Load Balancer or not:

<strong>Understanding Proxying and Redirection</strong>

<a title="https://technet.microsoft.com/en-us/library/bb310763(v=exchg.141).aspx" href="https://technet.microsoft.com/en-us/library/bb310763(v=exchg.141).aspx">https://technet.microsoft.com/en-us/library/bb310763(v=exchg.141).aspx</a>

&nbsp;

<table style="width:769px;" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="179">
Virtual directory /service
</td>
<td width="218">InternalURL</td>
<td width="194">ExternalURL (Internet-facing Active Directory site)</td>
<td width="176">ExternalURL (non-Internet-facing Active Directory site)</td>
</tr>
<tr>
<td width="179">/OWA</td>
<td width="218">NLB FQDN</td>
<td width="194">NLB FQDN</td>
<td width="176">$null</td>
</tr>
<tr>
<td width="179">/ECP</td>
<td width="218">NLB FQDN</td>
<td width="194">NLB FQDN</td>
<td width="176">$null</td>
</tr>
<tr>
<td width="179">/Microsoft-Server-ActiveSync</td>
<td width="218">NLB FQDN</td>
<td width="194">NLB FQDN</td>
<td width="176">$null</td>
</tr>
<tr>
<td width="179">/OAB</td>
<td width="218">NLB FQDN</td>
<td width="194">NLB FQDN</td>
<td width="176">$null</td>
</tr>
<tr>
<td width="179">/EWS</td>
<td width="218">NLB FQDN</td>
<td width="194">NLB FQDN</td>
<td width="176">$null</td>
</tr>
<tr>
<td width="179">POP/IMAP</td>
<td width="218">(InternalConnectionsSettings) <br>NLB FQDN</td>
<td width="194">Not applicable</td>
<td width="176">Not applicable</td>
</tr>
</tbody>
</table>

&nbsp;

Don&rsquo;t forget to double check your certificates as well, they should contain all the above used URLs.

&nbsp;

The below script will dump the Internal and External URLs for the above services to that you&rsquo;ll be able to check if your environment has been configured correctly (trust me, it&rsquo;s worth to triple-check because for lots of my customers we saw surprises, that explained some user or server performance issues we had at that time).

&nbsp;

This type of script is pretty common among administrators, pretty straightforward, anyways I tried to comment the script so that it&rsquo;s understandable by anyone, but leave me comments and suggestions if you don&rsquo;t understand something&hellip;

&nbsp;

<strong>You can either copy-paste the below lines (but the formatting will be a bit weird), or download the script from the following link.</strong>

<strong><span style="font-size:medium;"><a href="https://gallery.technet.microsoft.com/Powershell-script-to-ccde9d5f">Download the script without the -<em>ADPropertiesOnly</em> switch (slower, works on all Exchange versions)</a></span></strong>

<strong><span style="font-size:medium;"><a title="Download the script with the -ADPropertiesOnly switch (way faster, works on Exchange 2010, 2013+)" href="https://gallery.technet.microsoft.com/E2010-2013-Export-Exchange-df0112a2" target="_blank">Download the script with the -ADPropertiesOnly switch (way faster, works on Exchange 2010, 2013+)</a></span></strong>

&nbsp;
