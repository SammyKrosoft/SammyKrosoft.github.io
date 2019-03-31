---
layout: post
title: Exchange 2010 and later – use PowerBI to visualize Message Tracking log stats and use as an User Profile Analyzer
date: 2017-06-18 14:43
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Hi all ! My posts are pretty rare these days, some projects are still on the pipe but the pipe froze last winter…

Anyways I wanted to share with you guys the principle and methodology to use our <a target="_blank" href="https://powerbi.microsoft.com/en-us/" rel="noopener">excellent PowerBI</a> to analyze and visualize for you the <a target="_blank" href="https://technet.microsoft.com/en-us/library/bb124926(v=exchg.160).aspx" rel="noopener">Exchange Message Tracking Logs</a>. This has been brought up and created by our very Canadian <strong>Bernard Chouinard</strong>, former Microsoftee, now independent contractor, and who is our excellent Architect of messaging systems for organizations with more than 150,000 mailboxes, multiple datacenters, and taking advantages of all the bells and whistles that Exchange has to offer.

<em><strong>Note</strong>: the systems that Bernie architected never encountered any performance or technical issues other than flooding or datacenter explosions (not due to the Exchange design or planning issues , and yes, one of the datacenters exploded), and users never had service interruption thanks to <strong>Bernie’s</strong> great High Availability planning, design and deployment of his solution: the Disaster Recovery site seamlessly took over the service.</em>

In this instructional video, Bernie first demonstrate the dashboards he created, how he created the filters to get the relevant data, and then he shows you how to load the message tracking log data, showing each step to load and fix data so that PowerBI fully interpret them (remove extra lines that are by default on Exchange tracking logs for example – that has to be done only once and no need to do this anymore after, fix the date/time columns, make PowerBI to treat multi-recipient message as multiple messages to have accurate data).

<strong><u>1- before loading the message tracking logs:</u></strong>

<a target="_blank" href="https://www.youtube.com/watch?v=PnJ61q_sB_w&amp;feature=youtu.be" rel="noopener" title="PowerBI's Exchange Message Tracking Logs - before loading data"><img width="807" height="507" title="Bernie's PowerBI" alt="PowerBI Message Tracking Log Dashboard - Before loading data" src="https://msdnshared.blob.core.windows.net/media/2017/06/image432.png" border="0" /></a>

<strong><u>2- After loading the tracking logs:</u></strong>

<a target="_blank" href="https://www.youtube.com/watch?v=PnJ61q_sB_w&amp;feature=youtu.be" rel="noopener"><img width="805" height="472" title="Bernie's PowerBI" alt="PowerBI Message Tracking Log Dashboard - After loading data" src="https://msdnshared.blob.core.windows.net/media/2017/06/image430.png" border="0" /></a>

&nbsp;
<ul>
 	<li>&gt; This dashboard happens to be very useful for Exchange Architects when we have to define the Users Profiles when designing a new Exchange architecture: we can easily get from there the users sent/receives messages per day, on one day or on multiple days by just selecting boxes, and also the average message size that shows up on one of the graphs.</li>
 	<li>&gt; We can also get message usage patterns and trends, and then troubleshoot, adjust or evolve the infrastructure accordingly if needed.</li>
</ul>
&nbsp;

If you don’t feel like following all the steps from the video, Bernie gently provides us the PowerBI template from which you can just change the path where your Exchange 2010+ Message tracking logs are located, and just see the magic operate. Below is how to change the template’s path on the PowerBI Exchange Message Tracking Log template: go to “<strong>Edit Queries</strong>” menu, and change the “<b>Var_LogFolder</b>” Folder path to the one containing your Message Tracking Logs.

<a href="https://msdnshared.blob.core.windows.net/media/2017/06/clip_image0012.jpg"><img width="507" height="287" title="clip_image001" alt="clip_image001" src="https://msdnshared.blob.core.windows.net/media/2017/06/clip_image001_thumb2.jpg" border="0" /></a>

<a href="https://msdnshared.blob.core.windows.net/media/2017/06/clip_image00151.jpg"><img width="511" height="241" title="clip_image001[5]" alt="clip_image001[5]" src="https://msdnshared.blob.core.windows.net/media/2017/06/clip_image0015_thumb1.jpg" border="0" /></a>

&nbsp;

<a target="_blank" href="https://msdnshared.blob.core.windows.net/media/2018/02/Trackinglog-Final-Lab2.zip" rel="noopener">The template is located here</a>.

And a small bonus from our friend Bernie, to get Exchange tracking logs using Powershell : <a href="https://msdnshared.blob.core.windows.net/media/2017/06/GetTrackinglog.zip">GetTrackinglog</a>...

&nbsp;

<strong>Thanks very much to Bernard Chouinard (Canada) for this awesome effort and sharing it with the community</strong>, and I wanted to highlight how humble he is about all the awesome work he does (Bernie is what we call a “Silent Hero”).
