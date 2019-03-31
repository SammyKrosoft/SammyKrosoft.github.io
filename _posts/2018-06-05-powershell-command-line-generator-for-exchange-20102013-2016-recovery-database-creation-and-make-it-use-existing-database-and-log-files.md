---
layout: post
title: PowerShell command line generator for Exchange (2010,2013, 2016) Recovery Database creation and make it use existing database and log files
date: 2018-06-05 10:11
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<h1><span>Description</span></h1>
Download link <a href="#download"> at the very end of this article</a>...

This program is a Graphical User Interface that will help you generating Exchange Management Shell command lines to:
<ol>
 	<li>Create a Recovery Database using temporary path for the database and its corresponding LOG files set</li>
 	<li>Point that newly created Recovery Database to an existing database file and its corresponding LOG files set</li>
 	<li>Display the newly created Recovery Database and its paths just modified</li>
 	<li>Mount the Recovery Database using the existing database file and corresponding LOG files set</li>
 	<li>(or 4bis) - Displaying the status and file paths of that database again (just to confirm)</li>
</ol>
<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/ExchangeRecoveryDatabasePowerShellCmdGenerator.png"><img alt="Fig.1" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/ExchangeRecoveryDatabasePowerShellCmdGenerator.png" /></a>
<h1><span>Launching it</span></h1>
Just type <code>.\Launch-RecoveryDatabaseCreateGUI.ps1</code> or "Run with Powershell" from Windows Explorer:

<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/RunWithPowerShell.png"><img alt="Fig.2" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/RunWithPowerShell.png" /></a>

Once you have the desired values for your Server name, your EDB file and LOG paths, etc..., Clicking on the [Copy to Clipboard] button will copy the command list to the Clipboard.

=&gt; that way, you will be able to paste your commands in a PowerShell script, or in a NOTEPAD, or directly in an Exchange Management Shell for immediate execution !

<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/CopiedToClipboard.png"><img width="546" height="214" alt="Fig.3" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/CopiedToClipboard.png" class="" /></a>
<blockquote>Note: In order to see the "Run with Powershell" context menu on Windows Explorer, you need to set "Notepad" as the default "open with" program for .PS1 files -&gt; see the "Miscellaneous" section below if you don't have your "Run with PowerShell" menu and if you with to get it back.</blockquote>
<blockquote>Note2: if you reset the ".PS1" file association with NOTEPAD, you'll lose any previous association you made with ".PS1" files...</blockquote>
<h1><span>Miscellaneous : How to retrieve your "Run with Powershell" menu from Windows Explorer</span></h1>
If you don't find your "Run with PowerShell" menu anymore when right-clicking a PS1 file, you just have to reset the default program to open .PS1 to be the basic Windows "Notepad". Here's how to do it :
<ol>
 	<li>Right-click to any .PS1 file, and click "Open with":</li>
</ol>
<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/SetDefaultNotepadForPS1-1of2.png"><img alt="Fig.3" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/SetDefaultNotepadForPS1-1of2.png" /></a>
<ol start="2">
 	<li>Choose Notepad, and ensure the <code>[x] Always use this app to open .ps1 files</code> is checked :</li>
</ol>
<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/SetDefaultNotepadForPS1-2of2.png"><img alt="Fig.4" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/SetDefaultNotepadForPS1-2of2.png" /></a>
<ol start="3">
 	<li>You're done ! From now on, and until you change the default editor for .ps1 files, you'll see the "Run with PowerShell" context menu when you right-click a .PS1 file:</li>
</ol>
<a href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/blob/master/Screenshots/SetDefaultNotepadForPS1-Conclusion.png"><img alt="Fig.5" src="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator/raw/master/Screenshots/SetDefaultNotepadForPS1-Conclusion.png" /></a>
<a id="download"></a>
<h1><span>Download</span></h1>
<a target="_blank" href="https://gallery.technet.microsoft.com/Exchange-2010-2013-2016-bcd61dc7" rel="noopener">Download it on TechNet Gallery</a>

Or get it on the <a target="_blank" href="https://github.com/SammyKrosoft/Exchange-Recovery-Database-PowerShell-commands-generator" rel="noopener">tool's GitHub page</a>
