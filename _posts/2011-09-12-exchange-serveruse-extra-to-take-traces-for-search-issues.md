---
layout: post
title: Exchange Server–Use ExTRA to take traces for search issues
date: 2011-09-12 12:28
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p><strong>To diagnose a problem more indepth, we will use the Exchange Troubleshooting Analyzer (from the Toolbox on the Exchange Management Console) :</strong></p><p>- Select a task</p><p>- Click on trace control</p><p>- Click on set manual trace log</p><p>- On components to trace, choose Store.</p><p>- On trace tags, choose: tagQueryCi , tagQueryCiDump, and tagSearch</p><p>- Click on start tracing</p><p>- Then check the application log, once you see the event on the application log, </p><p>stop tracing. </p><p>- an MS Support Engineer will then analyze the *.etl resulting file</p><p>&nbsp;</p><p>MS Support will then use an internal debugging tool to decrypt or transform the resulting etl file to a readable format, and analyze it and eventually give you the analyzis results.</p></p>

