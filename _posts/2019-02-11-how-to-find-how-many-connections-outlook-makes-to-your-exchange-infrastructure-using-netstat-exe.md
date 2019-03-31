---
layout: post
title: How-To – Find how many connections Outlook makes to your Exchange infrastructure using Netstat.exe
date: 2019-02-11 13:05
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
In this quick procedure we leverage <strong>TaskList.exe</strong> command (default Windows command line utility) to get a specific process name and ID, and the <strong>Netstat.exe</strong> command (also a default Windows command line utility) to quickly find the opened connections from a specific application.

&nbsp;

<strong><em>Note</em></strong><em>: you can use other tools to get these, like the Windows Task Manager for example to find the process names and IDs, and/or the excellent <span><a target="_blank" href="https://docs.microsoft.com/en-us/sysinternals/downloads/tcpview" rel="noopener">TCPView from sysinternals</a></span> to graphically see the opened connections along with process names… or you can use any network analyzer tool (like Microsoft Message Analyzer, Network Monitor, Wireshark, …) or even the Windows Resource Monitor (Resmon.exe)  if you prefer.</em>

<strong> </strong>

When you launch an application like Outlook, it usually opens a process that has a specific ID. We are going to get the ID for the Outlook application, which associated process and then run Netstat to find out how many lines, in other words how many network connections and on which port do we have opened for Outlook.

<strong> </strong>
<ul>
 	<li>
<h1><strong>Step 1: Get your Outlook PID using Tasklist</strong></h1>
</li>
</ul>
&nbsp;

Run the below command:
<pre>Tasklist | find /I “outlook.exe”</pre>
This command is leveraging MS-DOS TaskList.exe command, and we use the “Find” command after a pipe to just output the line of the whole output that include the “outlook.exe” process.

The output will look like the below:

![Figure 1](/assets/media/2019/02/Fig-1.png)

<span>Fig. 1 - Outlook PID = 4028</span>

<span> </span>
<ul>
 	<li>
<h1><strong>Step 2 : Get the opened connections by this PID using Netstat:</strong></h1>
</li>
</ul>
&nbsp;

Run the below command:

```powershell
netstat-ano -p TCP | find /I "PID_Found_Above"
```

This command leverages Netstat with the -a -n -o options combined into one -ano parameter, and focussing on the TCP protocol. Then we use the “Find” command after a pipe to just output the line that include our process ID that we found on Step 1.

![Figure 2](/assets/media/2019/02/Fig-2.png)

<i>Fig. 2 – 2 entries, one for Mailbox connection, one for Directory connection</i>

<span>Here I see 2 entries:</span>

<span> </span>
<pre><span>TCP        192.168.2.56:50701               192.168.2.50:443            ESTABLISHED               4028</span>
<span>TCP        192.168.2.56:50702               192.168.2.50:443            ESTABLISHED               4028</span></pre>
<span> </span>

<span>One is for Mailbox connection (download e-mail body and attachments, mark e-mail as read, move and/or delete e-mails, etc...), and the other one is for Directory connection (for address book resolution in Outlook, address book searches, etc…).</span>

<span> </span>
<h1><strong><span>Conclusion</span></strong></h1>
<span>When you see 2 entries like the above example that I paste below again:</span>
<pre>TCP        192.168.2.56:50701               192.168.2.50:443            ESTABLISHED               4028
TCP        192.168.2.56:50702               192.168.2.50:443            ESTABLISHED               4028</pre>
This means I have 2 network connections that Outlook is making to my Exchange server. Note that this is the case for MAPI over HTTP, <strong><u>AND</u></strong> when you don’t have any additional mailboxes (shared mailboxes or a mailbox where you have full mailbox access and brought to your profile using auto-mapped mailboxes).

<span>If you are using Outlook Anywhere aka RPC over HTTP (which is deprecated in Office 365’s Exchange Online), we will have an additional connection, as RPC over HTTP needs 2 connections as opposed to MAPI over HTTP: one RPC over HTTP connection for e-mail sending (RPC-OUT : uploading e-mail data from Outlook to Exchange), and one RPC over HTTP connection for e-mail receiving (RPC-IN : downloading e-mail data from Exchange to Outlook) – that way with the Exchange Directory connection, we will have at least 3 connections for an Outlook with a profile that just has one mailbox configured in Online mode).</span>

<span> </span><span> </span>

<span>On Outlook (in Online mode) Connection Status you will find 3 entries:</span>
<ul>
 	<li><strong>Exchange Directory which session type is Foreground</strong></li>
 	<li><strong>Exchange Mail which session type is Foreground as well</strong></li>
 	<li>And another Exchange Directory which session type is “Background”</li>
</ul>

![Figure 3](/assets/media/2019/02/Fig-3.png)

<i>Fig.3 – Outlook Connection Status view</i>

<span> </span><span> </span>

<span>The Exchange Directory “Background” is essentially there for the cached entries, but does not account for a network connection. As long as Netstat shows only 2 entries, I guess we can tell the network guys that Outlook / Exchange in MAPI over HTTP mode, without other Public Folder and without additional groups and shared mailboxes, only needs 2 network connections opened between Outlook and Exchange…</span>

<span> </span><strong><span>Note</span></strong><span>: the TCPView tool mentioned above enabled you to get the application name and its opened Network connections a bit faster, here’s an example here:</span>

![Figure 1](/assets/media/2019/02/Fig-4.png)

<span>Fig. 4 - TCPView graphical user friendly interface to achieve the same as the above 2 steps, in one tool</span>

<span> </span>
<h1><span>Resources</span></h1>
<span> </span><span>Remove Automapping for a shared mailbox in Office 365</span>

<span><a target="_blank" href="https://support.microsoft.com/en-us/help/2646504/how-to-remove-automapping-for-a-shared-mailbox-in-office-365" rel="noopener">https://support.microsoft.com/en-us/help/2646504/how-to-remove-automapping-for-a-shared-mailbox-in-office-365</a></span>

Microsoft Message Analyzer:

<span><a target="_blank" href="https://www.microsoft.com/en-us/download/details.aspx?id=44226" rel="noopener">https://www.microsoft.com/en-us/download/details.aspx?id=44226</a></span>

<span>Network Monitor v3.4 (deprecated but still functional)</span>

<span><a target="_blank" href="https://www.microsoft.com/en-us/download/4865" rel="noopener">https://www.microsoft.com/en-us/download/4865</a></span>

&nbsp;
