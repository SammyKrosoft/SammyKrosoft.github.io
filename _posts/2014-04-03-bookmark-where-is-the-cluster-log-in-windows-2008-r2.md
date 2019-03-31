---
layout: post
title: Bookmark – Where is the Cluster Log in Windows 2008 R2 ?
date: 2014-04-03 09:11
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p><b>Disclaimer: ---------------------------------------------------------</b><p><b>Thanks to </b><a href="http://blogs.msdn.com/51517/ProfileUrlRedirect.ashx">Symon Perriman MSFT</a> for this information !</p><p>Putting it in my blog as well as I&rsquo;m using my blog as an extension of my (red fish) memory</p><p><b>Check out his blog for the details on Cluster Logs : <a title="http://blogs.msdn.com/b/clustering/archive/2008/09/24/8962934.aspx" href="http://blogs.msdn.com/b/clustering/archive/2008/09/24/8962934.aspx">http://blogs.msdn.com/b/clustering/archive/2008/09/24/8962934.aspx</a></b></p><p><strong>---------------------------------------------------------------------------End of Disclaimer</strong></p><p><strong></strong></p><p><strong></strong></p><p><b>CREATING THE CLUSTER.LOG:</b></p><p>From one of the nodes of the cluster, </p><p>- open a Command Prompt with Administrator rights.&nbsp; </p><p>- The simplest command to create the log is to type &ldquo;<strong><font face="Lucida console">cluster log /g</font></strong>&rdquo;.&nbsp; </p><p>- A <strong>cluster.log </strong>file will be generated and stored in the <strong>%windir%ClusterReports </strong>directory on each node of the cluster.&nbsp; </p><p><em>Note that with all commands you can use either &ldquo;cluster &hellip; &rdquo; or &ldquo;cluster.exe &hellip;&rdquo; as they have the same functionality.</em></p><p></p><p>Again, thanks Symon ! </p><p></p><p>Sam.</p></p>

