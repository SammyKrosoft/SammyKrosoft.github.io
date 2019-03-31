---
layout: post
title: How to use a list of servers in a file to process operations on multiple servers
date: 2012-01-24 07:27
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>Example : I&rsquo;d like to ping several servers listed in a servers.txt file :<p>- use get-content &lt;filename&gt; in between brackets after a named parameted :</p><p><font face="Courier New">Test-Connection -ComputerName (get-content servers.txt) -Count 1</font></p><p>You can also do the same without the parameter name if it&rsquo;s a positional parameter :</p><p><font face="Courier New">Test-Connection (get-content servers.txt) -Count 1</font></p><p><font face="Courier New"><em>More examples to come later&hellip;</em></font></p></p>

