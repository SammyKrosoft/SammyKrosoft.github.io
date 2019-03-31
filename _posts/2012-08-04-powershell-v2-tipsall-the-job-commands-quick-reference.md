---
layout: post
title: Powershell v2 tips–All the Job commands (quick reference)
date: 2012-08-04 22:53
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>I&rsquo;m a red fish, so I wrote the Powershell&rsquo;s Job commands here so I&rsquo;ll have a repository.</p><p>An other way to list the Job commands is to type <font face="Courier New">get-command *-job <font face="Arial">or</font> get-command &ndash;Noun job</font></p><p><font face="Courier New"></font></p><ul>   <li>Start-Job :</li> </ul><p><font face="Courier New">Start-Job -ScriptBlock {dir &ndash;path c:windows &ndash;rec}</font></p><p><font face="Courier New">Start-Job -Filepath c:scriptssample.ps1</font></p><p><font face="Courier New">Invoke-Command -computername s1 `</font></p><p><font face="Courier New">-scriptblock {get-eventlog system} &ndash;asjob</font></p><p>&nbsp;</p><ul>   <li>Get-Job</li>    <li>Receive Job [-keep]</li>    <li>Wait-Job</li>    <li>Stop-Job</li> </ul><p><font face="Courier New">Get-Job &ndash;name n*| Stop-Job</font></p><p><font face="Courier New">Stop-Job *</font></p><ul>   <li>Remove-Job</li> </ul></p>

