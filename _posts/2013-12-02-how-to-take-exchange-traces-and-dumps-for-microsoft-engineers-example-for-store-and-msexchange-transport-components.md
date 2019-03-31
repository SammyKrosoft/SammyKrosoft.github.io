---
layout: post
title: How to take Exchange traces and dumps for Microsoft Engineers – Example for STORE and MSExchange Transport components
date: 2013-12-02 12:19
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p></p>
<p></p>
<p><strong><span style="font-size:xx-large;">Prerequisites</span></strong></p>
<p>We&rsquo;ll use the ExTra.exe (Exchange 2007/2010 only, not available anymore&nbsp;on Exchange 2013) and ProcDump Tools to take traces that will capture the issue.</p>
<p>To take memory dump of Store.exe and EdgeTransport.exe for deep code level analysis.</p>
<p>Download ProcDump: <a href="http://technet.microsoft.com/en-US/sysinternals/dd996900">http://technet.microsoft.com/en-US/sysinternals/dd996900</a></p>
<p>The instructions</p>
<p></p>
<p><strong><span style="font-size:xx-large;">Scenario</span></strong></p>
<p>Mails get stuck in some servers, not always the same, but the common point between the servers is that it&rsquo;s always queues which point to the same database (messages for users in a particular database).</p>
<p>Event Log analysis does not show any issues, event with Event Logging set to &ldquo;Expert&rdquo; for MSExchange Transport and MSExchangeIS store delivery components.</p>
<p>We need to take traces that will capture what Exchange is trying to do, and that will enable Microsoft to tell what object is blocking the queue(s).</p>
<p></p>
<p></p>
<p><strong><span style="font-size:xx-large;">Traces</span></strong></p>
<p><strong><span style="text-decoration:underline;">Traces: take both ExTra traces and Dumps from Store and Transport (or other process if it&rsquo;s for a different scenario)</span></strong></p>
<p><strong><span style="text-decoration:underline;">These will be analyzed by an accredited Microsoft Engineer.</span></strong></p>
<p><strong><span style="text-decoration:underline;">1 of 2 &gt; Extra.exe</span></strong></p>
<p>Launch ExTra.exe (available by default on Exchange 2010 -&nbsp;<span style="color:#ff0000;"><strong>With the introduction of Exchange 2013, this tool is no longer packaged with the product &amp; is only available as a separate package from MS Support</strong></span>) and choose the &ldquo;Trace Control&rdquo; option</p>
<p></p>
<p><a href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/3632.image_2.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/3632.image_5F00_2.png"><img width="244" height="202" title="image" style="display:inline;" alt="image" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/6215.image_thumb.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/6215.image_5F00_thumb.png" border="0" /></a></p>
<p></p>
<p>Then click on &ldquo;Set manual trace tags&rdquo; on the next screen:</p>
<p><a href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/3034.image_4.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/3034.image_5F00_4.png"><img width="244" height="132" title="image" style="display:inline;" alt="image" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/8814.image_thumb_1.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/8814.image_5F00_thumb_5F00_1.png" border="0" /></a></p>
<p></p>
<p>Then select the components we with to trace (for our scenario example, Transport and StoreDriver)</p>
<p><a href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/1423.image_6.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/1423.image_5F00_6.png"><img width="244" height="156" title="image" style="display:inline;" alt="image" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/2844.image_thumb_2.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/2844.image_5F00_thumb_5F00_2.png" border="0" /></a></p>
<p></p>
<p>and click on the below &ldquo;Start Tracing&rdquo; link.</p>
<p>To stop the tracing, when the issue will occur and when the Dumps below will be taken, click on &ldquo;Stop Tracing now&rdquo;</p>
<p></p>
<p><a href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/4118.image_8.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/4118.image_5F00_8.png"><img width="244" height="125" title="image" style="display:inline;" alt="image" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/0218.image_thumb_3.png" original-url="http://blogs.technet.com/cfs-file.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-00-73-61-metablogapi/0218.image_5F00_thumb_5F00_3.png" border="0" /></a></p>
<p></p>
<p></p>
<p><strong><span style="text-decoration:underline;">2 of 2&gt; process memory dump - <span style="background-color:#ff0000;">NOTE : do not copy/paste the below lines, just retype them as for some reason it takes invisible special characters</span>:</span></strong></p>
<ul>
<li>On one HUB, run this from the ProcDump directory:</li>
</ul>
<p><span style="font-family:Courier New;font-size:large;"><strong>procdump.exe -ma <strong><span style="font-family:Courier New;font-size:large;">edgetransport.exe </span></strong>&ndash;n 3 &ndash;s 15 -accepteula c:\dumps</strong></span></p>
<p></p>
<ul>
<li>On the Mailbox Server, run this from the ProcDump directory - NOTE the &quot;-mp&quot; parameter instead of the &quot;-ma&quot;, because otherwise we&#39;ll get 10s GBs of data that we don&#39;t necessarily need for store.exe debug:</li>
</ul>
<p><span style="font-family:Courier new;font-size:large;"><strong>procdump.exe -mp store.exe &ndash;n 3 &ndash;s 15 -accepteula c:\dumps</strong></span></p>
<p></p>
<p><strong>Note: </strong>you may not want to place the dump output files in the C:\ drive, you can specify any path you want replacing <strong>c:\dumps</strong> by <strong>any other directory</strong>.</p>
<p></p>
<p><span style="text-decoration:underline;">More great and very useful information on this link :</span></p>
<p><strong>Procdump: How to PROPERLY gather dump (.dmp) files for crashes and hangs, CPU spikes, etc, including gathering PERF data, for Exchange issues &ndash; </strong>by Kris Waters, Premier Field Engineer in Microsoft US.</p>
<p><a title="http://blogs.technet.com/b/kristinw/archive/2012/10/03/procdump-how-to-properly-gather-dump-dmp-files-for-crashes-and-hangs.aspx" href="/b/kristinw/archive/2012/10/03/procdump-how-to-properly-gather-dump-dmp-files-for-crashes-and-hangs.aspx">http://blogs.technet.com/b/kristinw/archive/2012/10/03/procdump-how-to-properly-gather-dump-dmp-files-for-crashes-and-hangs.aspx</a></p>
<p></p>
<p>Sam.</p>
