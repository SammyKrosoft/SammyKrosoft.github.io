---
layout: post
title: LogParser or ExRAP tools “Log row too long” issue
date: 2012-12-11 08:38
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>Bookmark to Rhoderick&rsquo;s blog :</p><p><a href="http://blogs.technet.com/b/rmilne/archive/2012/07/05/how-to-fix-log-parser-log-row-too-long.aspx">http://blogs.technet.com/b/rmilne/archive/2012/07/05/how-to-fix-log-parser-log-row-too-long.aspx</a></p><p><em>Note : The maximum value is *<b>not</b>* 0xFFFFFFFF.&nbsp; It&rsquo;s actually 0x00785111 (hard coded in the LogParser code)</em></p><p><em>To determine the value to put on the below registry key, take the performance log file that you have in CSV format (works only on CSV formats), copy the headers only, and paste it in Notepad, then save it as a txt file. The size of the txt file determines the minimum value we need to put on the CSVInMaxRowSize attribute (in Bytes) :</em></p><p><b>x86</b></p><p><b><font face="Courier New">HKEY_LOCAL_MACHINESOFTWAREMicrosoftLog Parser</font></b></p><p><b><font face="Courier New">REG_DWORD CSVInMaxRowSize 393216 decimal</font></b></p><p>To automate adding on x86 machines:</p><p><b><i><font face="Courier New">REG.exe ADD "HKLMSoftwareMicrosoftLog Parser" /v CSVInMaxRowSize /t REG_DWORD /d 393216</font></i></b></p><p><b>x64</b></p><p><b><font face="Courier New">HKLMSoftwareWow6432NodeMicrosoftLog Parser</font></b></p><p><b><font face="Courier New">REG_DWORD CSVInMaxRowSize 393216 decimal</font></b></p><p>To automate adding on x64 machines:</p><p><b><i><font face="Courier New">REG.exe ADD "HKLMSoftwareWow6432NodeMicrosoftLog Parser" /v CSVInMaxRowSize /t REG_DWORD /d 393216</font></i></b></p></p>

