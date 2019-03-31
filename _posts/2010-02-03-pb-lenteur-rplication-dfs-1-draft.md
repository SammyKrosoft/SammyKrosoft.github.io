---
layout: post
title: Pb lenteur réplication DFS – 1 (DRAFT !)
date: 2010-02-03 02:46
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>PROBLEM: Slow DFS replication between the member servers   <br>RESOLUTION: Checked and found that getting event id 5002 and 5012 and users unable     <br>to access the shares.    <br>&agrave;AS both the member servers are windows 2003 r2 machines so asked to install the     <br>hotfix number kb 931685    <br>&agrave;Also under Registry made the following changes    <br>--&gt;HKLMSystemCCSServicesTcpIpParameters    <br>EnableRSS,    <br>EnableTcpChimmey    <br>EnableTCPA    <br>And changed the entries to 0    <br>&agrave;Restart the server    <br>&agrave;Issue resolved</p>

