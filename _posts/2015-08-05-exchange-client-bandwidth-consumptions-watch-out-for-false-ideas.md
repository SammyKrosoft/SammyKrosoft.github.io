---
layout: post
title: Exchange client bandwidth consumptions – Watch out for false ideas about Outlook cached mode versus Outlook online mode regarding bandwidth !
date: 2015-08-05 01:14
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<h1><strong>Introduction</strong></h1>
When we talk about Outlook cached mode and Outlook online mode recommendations, there is a point we always miss, which is the Network bandwidth.

Before discussing bandwidth, you guys know that we (Microsoft) recommend enabling Outlook cached mode at the first place, especially on these cloud days. Quoting:
<h4><em>When to use Cached Exchange Mode</em></h4>
<em>Cached Exchange Mode is the premier configuration in Outlook 2010. We recommend it in all circumstances, except those specifically indicated in </em><a href="http://technet.microsoft.com/en-us/library/cc179067(v=office.14).aspx#whentouse"><em>When to use Online Mode</em></a><em>.</em>

<em>Although we recommend Cached Exchange Mode in most user configurations, it is especially valuable in the following scenarios:</em>
<ul>
	<li><em>Portable computer users who frequently move in and out of connectivity.</em></li>
	<li><em>Users who frequently work offline or without connectivity.</em></li>
	<li><em>Users who have high-latency connections to the Exchange Server computer.</em></li>
</ul>
<em>See : </em><a href="http://technet.microsoft.com/en-us/library/cc179067(v=office.15).aspx"><em>http://technet.microsoft.com/en-us/library/cc179067(v=office.15).aspx</em></a><em> for Outlook 2013 and/or </em><a href="http://technet.microsoft.com/en-us/library/cc179067(v=office.14).aspx"><em>http://technet.microsoft.com/en-us/library/cc179067(v=office.14).aspx</em></a><em> for Outlook 2010</em>

&nbsp;

<strong><span style="text-decoration: underline">NOTE:</span></strong> But see about network, we talk about <strong>latency</strong>, and <strong>connectivity</strong>, which fall into the “<strong>Network reliability</strong>” field. Bandwidth does not fall into the “Network reliability” category, it’s more a network specificity or network property;
<h1><strong>Calculations results</strong></h1>
While calculating the bandwidth requirements with the Exchange client bandwidth calculator for a huge environment, we came up with the following conclusions :
<ul>
	<li>Outlook cached versus online mode bandwidth comparison</li>
</ul>
- Outlook <strong>online</strong> mode requires <strong>smaller latency</strong> than Outlook <strong>cached</strong> mode; on the other hand, <span style="text-decoration: underline">Outlook <strong>online</strong> mode uses <strong>less bandwidth</strong> than <strong>cached</strong> mode</span>

- Outlook 2007/2010 <strong>cached</strong> mode can potentially use from 20% to <strong>80% more network bandwidth</strong> than Outlook 2007/2010 <strong>online</strong> mode

This of course varies from a small Mailbox (couple of megs) with a small OAB to a large Mailbox (2 GB and more) – but bandwidth used by huge mailboxes can be mitigated a bit for big mailboxes with the use of Outlook 2013 “Mail to keep offline” slider:

<a href="http://msdnshared.blob.core.windows.net/media/2015/08/Outlook-how-many-mails-to-keep-offline-lighter-OST-file.png"><img class="alignnone  wp-image-1941" src="http://msdnshared.blob.core.windows.net/media/2015/08/Outlook-how-many-mails-to-keep-offline-lighter-OST-file-300x213.png" alt="Outlook - how many mails to keep offline - lighter OST file" width="604" height="429" /></a>

&nbsp;

A bit more to come soon ...

&nbsp;

Sam.
