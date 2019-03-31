---
layout: post
title: Aide-Mémoire – Converting ExTra ETL trace to CSV
date: 2013-09-13 05:38
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>Just a reminder for me (Extrace.exe is an internal tool used for deep debugging).</p><p>&nbsp;</p><p>extrace.exe -c -v ExchangeDebugTraces.etl &gt;TraceOut.csv</p><p>&nbsp;</p><p>We have to use the Microsoft.Exchange.Diagnostics.dll matching the Exchange build where the trace come from. To check your Exchange server build, checkout the following link:</p><p><a title="http://social.technet.microsoft.com/wiki/contents/articles/240.exchange-server-and-update-rollups-build-numbers.aspx" href="http://social.technet.microsoft.com/wiki/contents/articles/240.exchange-server-and-update-rollups-build-numbers.aspx">http://social.technet.microsoft.com/wiki/contents/articles/240.exchange-server-and-update-rollups-build-numbers.aspx</a></p><p>&nbsp;</p><p>Sam.</p></p>

