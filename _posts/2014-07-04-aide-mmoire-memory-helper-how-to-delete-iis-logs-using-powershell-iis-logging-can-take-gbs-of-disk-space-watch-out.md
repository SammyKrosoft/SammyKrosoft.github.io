---
layout: post
title: Aide-mémoire – memory-helper – How to delete IIS logs using Powershell (IIS logging can take GBs of disk space, watch out !)
date: 2014-07-04 13:36
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>Hi all,</p><p>&nbsp;</p><p>A quick &ldquo;tip&rdquo; that can be useful as IIS logging usually generates GB of files that we don&rsquo;t necessary monitor, and I thought it was important enough to write a quick post about it, as I got lately several examples of customers for which C: free drive space fell below 10%; </p><p>You may ask yourself, why this guy is writing such a post on an Exchange Server blog ? What does this has to do with Exchange ?</p><p>- reason is Exchange is using the C:TEMP or C:WindowsTemp folder for a few things like message content conversion (the HUB role in particular). So if we run out of disk space on C:, there is a risk that the Transport Service stops; or even worse, the Windows host can stop working because there is no more space to handle temp files for other OS related tasks.</p><p>&nbsp;</p><p>So continue to monitor your disk space on the C: drive, and you can use the below command line to check and purge your IIS Logging directory (if you need IIS logging to stay activated) :</p><p><font face="Lucida Console">get-childitem -Path C:inetpublogsLogFiles -recurse | where {$_.lastwritetime -lt (get-date).addDays(-90)} | Foreach-Object {del $_.FullName}</font></p><p>&nbsp;</p><p>Also useful if it appears you don&rsquo;t need IIS logging because there is no troubleshooting need, you can simply deactivate IIS logging:</p><p>&nbsp;</p><p><strong>Enable or Disable Logging (IIS 7)</strong></p><p><a title="http://technet.microsoft.com/en-us/library/cc754631(v=WS.10).aspx" href="http://technet.microsoft.com/en-us/library/cc754631(v=WS.10).aspx">http://technet.microsoft.com/en-us/library/cc754631(v=WS.10).aspx</a></p><p>&nbsp;</p><p>Cheers,</p><p>Sam.</p></p>

