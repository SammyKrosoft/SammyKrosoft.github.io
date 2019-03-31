---
layout: post
title: Exchange 2013/2016/2019 & ExO - a sample Get-Mailbox GUI
date: 2018-11-19 00:05
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Here is a GUI to get mailboxes information in an Exchange 2010, 2013, 2016, 2019 and/or Exchange Online (O365) environments.
I initially created this GUI just to illustrate how we can use Windows Presentation Foundation (WPF) forms to ease up the execution of PowerShell commands.
<a href="#DownloadLink">Download link is at the very end of this article.</a>
<h1>Principle</h1>

<hr />

For this GUI, I designed the interface using Windows Presentation Foundation, which is the next generation of Windows Forms, and which enables anyone to design Windows interfaces for underlying programs, to help users to get the most from computer programs.

Here's the principle used to create this Powershell application using WPF:
<ul>
 	<li>First, I designed the Windows Presentation Foundation forms using Visual Studio 2017 Community Edition (which is free), and pasted the generated XAML code from Visual Studio 2007 into a PowerShell script - in this script I pasted directly the XAML code into a PowerShell "here-string", but you can load the XAML code from a separate file using Get-Content for example, that way if you want to change your Windows WPF form design, you just have to modify or paste the new code from Visual Studio directly into that separate file and leave your .ps1 script alone.</li>
 	<li>then with a PowerShell code snippet I parsed the XAML code to get the form objects that PowerShell understands - buttons, check boxes, datagrids, labels, text blocks used as inputs as well as outputs sometimes (to put the PowerShell commands corresponding to the users input for example), and the form object itself, which is the container of all the other objects (buttons, checkboxes, etc...)</li>
 	<li>and finally I wrote the functions behind the form that I want to run when the form loads, when we click the buttons, when we check or uncheck the checkboxes, when we change the text in text boxes, or when we select items from datagrids =&gt; for code to execute when the user interacts with Windows WPF forms, we must use the WPF form object's "events" (add_click, add_textChanged, add_loaded, add_closing, etc...) =&gt; you can retrieve Windows WPF form objects on the MSDN, or simply on Visual Studio 2017 when you design your form (switch from object properties to the object events to view all available events for a selected object)</li>
</ul>
&nbsp;
<ul></ul>
I tried to make a synoptic view of the process in the below schema with small sample screenshots of my Visual Studio 2007 / Vistual Studio Code parts - you'll find the WPF - to - PowerShell code snippet sample I'm referring to in the below schema in <a target="_blank" href="https://github.com/SammyKrosoft/Code-Snippet-WPF-and-PowerShell" rel="noopener">this GitHub repository</a>...

<img alt="WPF from Visual Studio to PowerShell" src="https://github.com/SammyKrosoft/Code-Snippet-WPF-and-PowerShell/raw/master/DocResources/How-o-CreatePowerShellWPFApp.jpg" width="1541" height="1940" class="alignnone" />
<h1>Important notes</h1>

<hr />

This PowerShell app requires PowerShell V3, and also requires to be run from a PowerShell console with Exchange tools loaded, which can be an Exchange Management Shell window or a PowerShell window from where you imported an Exchange session, see my TechNet blog post for a summary on how to do this (<em>right-click =&gt; Open in a new tab otherwise below sites will load instead of this page</em>):
<blockquote>
<ul>
 	<li><a href="https://blogs.technet.microsoft.com/samdrey/2018/04/06/how-to-load-remote-powershell-session-on-exchange-2010-2013-2016-exchange-online-o365-2/">How-to – Load Remote Exchange PowerShell Session on Exchange 2010, 2013, 2016, Exchange Online (O365) – which ports do you need</a></li>
 	<li>
<p id="connect-to-exchange-online-powershell"><a target="_blank" href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/connect-to-exchange-online-powershell?view=exchange-ps" rel="noopener">Connect to Exchange Online PowerShell</a></p>

<ul>
 	<li>
<p id="connect-to-exchange-online-powershell"><em>Note: If you want to use multi-factor authentication (MFA) to connect to Exchange Online PowerShell, you need to download and use the Exchange Online Remote PowerShell Module. For more information, see <a href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell?view=exchange-ps">Connect to Exchange Online PowerShell using multi-factor authentication</a></em></p>
</li>
</ul>
</li>
 	<li><a href="https://blogs.technet.microsoft.com/samdrey/2017/12/17/how-to-load-exchange-management-shell-into-powershell-ise-2/">How To–Load Exchange Management Shell into PowerShell ISE</a></li>
</ul>
</blockquote>
<h1>Screenshots - because a picture is worth 1000 words...</h1>

<hr />

<h5>First window when launching the tool</h5>
<img alt="screenshot1" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image0.jpg" />
<h5>After a sample Get-Mailbox which name includes "user" string</h5>
<img alt="screenshot2" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image1.jpg" width="1013" height="529" class="alignnone" />

Note that for cloud mailbox, the "Location" column will tell you that the mailbox is hosted in the cloud:

<img alt="screenshot2.1" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image1-cloud_location.jpg" width="1017" height="531" class="alignnone" />
<h5>If you select "Unlimited" under the Resultsize (max number of mailboxes to search), or a number that is greater than 1000, you get a warning asking you if you want to continue</h5>
<img alt="screenshot3" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-Question-LotsOfItems.jpg" width="466" height="165" class="alignnone" /> <img alt="screenshot3.1" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-Question-LotsOfItems2.jpg" width="475" height="165" class="alignnone" />
<h5>Selecting mailboxes in the grid, notice the "Action on selected" button that becomes active</h5>
<img alt="screenshot4" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-SelectForAction.jpg" width="1013" height="529" class="alignnone" />
<h5>Action : After selecting some mailboxes in the grid, calling the "List Mailbox Features" action in the drop-down list</h5>
<img alt="screenshot5" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-Action-ListMbxFeatures.jpg" width="858" height="443" class="alignnone" />
<h5>Action: Anoter action possible, calling the Single Item Recovery and mailbox dumpster limits for the selected mailboxes</h5>
<img alt="screenshot6" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-Action-SingleItemRecoveryStatus.jpg" width="858" height="443" class="alignnone" />
<h5>Action: List mailbox quotas, including database quota for each mailbox</h5>
<em>Note that mailbox quotas list include the Database info quota - that is useful when mailboxes are configure to use Mailbox Database Quotas</em> <img alt="screenshot7" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-Action-ListMailboxQuotas.jpg" width="858" height="443" class="alignnone" />
<h5>On most actions, you can copy the list in Windows clipboard (will be CSV Formatted) for further analyis, reporting or documentation about your mailboxes</h5>
<img alt="screenshot8" src="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI/raw/master/DocResources/image-copyToClipBoard.jpg" width="301" height="165" class="alignnone" />
<h1 id="DownloadLink">More to come...</h1>

<hr />

<span style="font-size: x-large"><a target="_blank" href="https://gallery.technet.microsoft.com/Exchange-201320162019ExO365-e0a75aa4" rel="noopener">Download link</a></span>

You can also retrieve this project on <a target="_blank" href="https://github.com/SammyKrosoft/Exchange-Get-Mailboxes-GUI" rel="noopener">this page of my GitHub site</a>...
