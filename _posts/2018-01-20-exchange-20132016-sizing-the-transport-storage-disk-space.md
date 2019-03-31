---
layout: post
title: Exchange 2013/2016 - Transport Storage Disk Space Sizing Tool
date: 2018-01-20 22:16
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<em>** Updated the tool with control for "Servers Failure Tolerance per DAG" to not exceed the number of MBX servers entered + added disk space required for OS disk assuming you install both Windows and Exchange on the same drive - usually C:\ **</em>

All the information that led to that calculator come from the <a href="https://blogs.technet.microsoft.com/exchange/2013/05/06/ask-the-perf-guy-sizing-exchange-2013-deployments/"><strong><em>Ask the Perf Guy: Sizing Exchange 2013 Deployments</em></strong></a> article, not more, not less, and these calculations are also valid for Exchange 2016.

<strong>Disclaimer</strong>: this calculation formula/method gives an estimate of how much disk space on a worst case scenario can your <strong>mail.que</strong> database aka Transport Database grow to with the assumptions you take at inputs, and then enable you to allocate the adequate volume for your transport queue (<strong>mail.que</strong> database).

<strong>Challenge:</strong> I came up with this tool because as simple as it is to estimate the required disk space that will be necessary to host the transport database, many customers overlooked that part and then ended up with low disk space issues on their Exchange server resulting in one or more servers triggering their <a href="https://technet.microsoft.com/en-us/library/bb201658(v=exchg.150).aspx">Back Pressure</a> mechanism, and as a consequence refusing SMTP connections from other servers, and then message delivery slowing down or even stopping.

<strong>So how does it work?</strong> Most input metrics should be the same as for the <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a> ; maybe in a future version of that Exchange Server Role Requirements Calculator this Transport Queue database size estimate will be included, but in the meantime here's a separate calculator spreadsheet.
<div>
<table style="border-collapse: collapse" border="0" width="100%"><colgroup> <col style="width: 203px" /> <col style="width: 439px" /></colgroup>
<tbody valign="top">
<tr style="height: 21px">
<td style="padding-left: 7px;padding-right: 7px;border: solid 0.5pt"><strong>Nb mailboxes</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: solid 0.5pt;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">The number of mailboxes you're sizing for – should be the same as the number of mailboxes you put in the <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2011.png" alt="" />

Note: if you have multiple Tiers, add the total number of mailboxes</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Avg message size</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">The average size of the message sent throughout your organization – should be the same as the value you put on the <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2012.png" alt="" />

Note: if you have multiple Tiers, just calculate an average message size for all your tiers</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Msg per day</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">The total messages sent and received per day per user – again, should be the same as the value you put on the <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2013.png" alt="" />

Note: again for multiple Tiers, just to an average of messages sent/received per mailbox per day</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Max queue days tolerance</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">This is how long you will tolerate a stuck queue, or the "<strong>Message Queue Expiration (Days)</strong>" in the <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>

Uusually with a proper monitoring solution, you should know within the hour or two that a queue is stuck and piling up messages – the Technet sample uses the default of the "Message Queeu Expiration (Days)" which is 2 days, which is more than enough in 99,99% of the situations – also take into account that Exchange 2013/2016 have the ability to be taken into maintenance mode, with which you can drain the queues and pass on the message flow responsibility to another HUB server in the organization (See <a href="https://blogs.technet.microsoft.com/nawar/2014/03/30/exchange-2013-maintenance-mode/">this excellent article from my friend Nawaral here</a>)</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Safety net hold days</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">This is where you set how long Exchange 2013/2016 must keep the message copies in its Safety Net, which is part of the mail.que Transport Database. Default is 2 days on a brand new Exchange 2013/2016 environment, but it can be 7 days if Exchange 2013/2016 is deployed in a mixed Exchange 2010 environment.

<a href="https://technet.microsoft.com/en-us/library/jj657495(v=exchg.160).aspx">See the details of the Safety Net in this Technical Article</a></td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Number of database copies</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">This is how many database copies you have in your environment – should be the same as what you set on your <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a> around here:

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2014.png" alt="" />

<strong>NOTE</strong>: the above screenshot shows 3 HA database copies (first line), PLUS 1 lagged copy (second line) – The "Number of database copies" of the Transport Database will then be "4" in this case.

The last 2 lines are to specify how to distribute the HA (the real-time one) and the Lagged Database  copies across the primary site and secondary site in case you chose to have a site resilient environment.</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Number of MBX Servers</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">The number of Exchange server in your designed / sized solution.

On the  <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>, you'll find it here :

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2015.png" alt="" />

<strong>Note:</strong> multiply by 2 if you have a "site resilient" design as this is the number of Mailbox servers in the "Primary Datacenter" only...</td>
</tr>
<tr style="height: 20px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Number of DAGs</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">The number of DAG in your designed / sized solution.

On the  <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>, you'll find it here, just below the Number of Mailbox Servers Hosting Active Mailboxes / DAG (Primary Datacenter) field:

<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2016.png" alt="" /></td>
</tr>
<tr style="height: 21px">
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: solid 0.5pt;border-bottom: solid 0.5pt;border-right: solid 0.5pt"><strong>Severs Failure Tolerance per DAG</strong></td>
<td style="padding-left: 7px;padding-right: 7px;border-top: none;border-left: none;border-bottom: solid 0.5pt;border-right: solid 0.5pt">In your design, how many servers can be down at the same time per DAG without affecting the users…

<strong>Hint</strong> : use the "Distribution" tab in your <a href="https://gallery.technet.microsoft.com/office/Exchange-2013-Server-Role-f8a61780">Exchange Server Role Requirements Calculator</a>, and click on each "Fail server" button until there are some "failed" databases – the number of servers that can fail without failing any databases are the "Servers Failure Tolerance" per DAG…</td>
</tr>
</tbody>
</table>
</div>
&nbsp;

Quickly going through the "tool" appearance as it's quite simple:

<a href="https://msdnshared.blob.core.windows.net/media/2018/01/TransportCalculator.png"><img src="https://msdnshared.blob.core.windows.net/media/2018/01/TransportCalculator.png" alt="" width="884" height="382" class="aligncenter wp-image-4085 size-full" /></a>
<ul>
 	<li>You retrieve the table where you input your values described above, you can see as a result the "<strong>Per server Transport DB size</strong>" in the cylinder (supposed to represent a database)</li>
 	<li>You can see the sizing result, with the Transport DB Size (aka <strong>mail.que </strong>database), as well as an estimate of total minimal space recommended for your C:\ drive including Windows 2012/2016 type OS and Exchange 2013 / 2016 binaries + Exchange logs…</li>
 	<li>You see the "<strong>Sub-Calculations</strong>" part which you can expand clicking on the first "+" sign on the spreadsheet margin – it just shows the intermediate calculations just like the <a href="https://blogs.technet.microsoft.com/exchange/2013/05/06/ask-the-perf-guy-sizing-exchange-2013-deployments/">Technet article</a> (Overall Transport DB Size)…</li>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2018.png" alt="" />
<ul>
 	<li>Finally I've taken some of the useful details including the links to the important TechNet articles detailing the general Exchange Sizing rule, and also the Technet article with the sample values for the Transport Database Storage calculations (that's to confirm the formula has been transposed the right way into the "tool")</li>
</ul>
<img src="https://msdnshared.blob.core.windows.net/media/2018/01/012118_0331_Exchange2019.png" alt="" />
<p style="margin-left: 18pt"><span style="font-size: 14pt"><strong>Click here to go to the <a href="https://gallery.technet.microsoft.com/Exchange-20132016-3355cb9e">Download page on the TechNet Gallery</a>…
</strong></span></p>
