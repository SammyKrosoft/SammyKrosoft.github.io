---
layout: post
title: Event Logs CSV Collector - Created a Graphical User Interface around the Get-EventsFromEventLogs.ps1 script
date: 2018-05-23 11:26
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
*** Edited - added a function to check for Powershell V3 (<em>Function IsPSV3 () - required</em>) and exit if not ***

Hi mates,

&nbsp;

This is just a quick post to let you know that if you would prefer some mouse interaction and a more graphical input for the <a target="_blank" href="https://blogs.technet.microsoft.com/samdrey/2018/04/13/powershell-script-to-export-events-to-screen-andor-to-file-from-one-or-multiple-machines/" rel="noopener">Get-EventsFromEventLogs script</a>.

Basically, this GUI will let you fill the Computers, the Event ID and/or the Event Sources, as well as the Event Log types you wish to search in (Application and/or System and/or Security - note that for the "Security" event logs, you need Local Admin permissions on whichever machine you want to collect these), and the Events Level you want to search or export.

As you fill the boxes and check the options, you'll see the command line that will built itself as options are checked or unchecked, and as input boxes (Servers, Event ID(s), Event Sources) are filled or cleared:

<img src="https://msdnshared.blob.core.windows.net/media/2018/05/Get-EventsFromEventLogsG-GUI-CmdLineGenerator.png" />
<blockquote><em>NOTE: the "auto-fill" part of the PowerShell script is not more than a function that is called each time a mouse "click" event is registered on the Graphical Interface, and each time the text inside a box is changed, and that function checks all the WPF form controls (a control is a check box, or an inputbox, or a button, etc... it's an element of the form a user can interact with) and update the text in the "<strong>Function Command Line</strong>" box each time a "click" or "text changed" event is detected on the WPF form.</em></blockquote>
Here's the whole interface:

<a href="https://msdnshared.blob.core.windows.net/media/2018/05/Get-EventsFromEventLogsG-GUISample.png"><img width="790" height="497" class="alignnone size-full wp-image-5145" alt="" src="https://msdnshared.blob.core.windows.net/media/2018/05/Get-EventsFromEventLogsG-GUISample.png" /></a>
<blockquote><em>NOTE: when the "<strong>[X] Save events to file</strong>" checkbox is checked, the program will automatically save your results on a file on the same directory where the script is located, and open up the file in NOTEPAD at the end of the execution. If you leave this unchecked, the results will just be printed in the underlying PowerShell window (that's great to have an overview of the events as well as a summary of the Errors / Warnings / Critical / Information events before saving these in to a file for further analysis - with PowerBI Template for example :-) )</em><img /></blockquote>
&nbsp;
<blockquote><em>NOTE2: by default, if you don't specify a computers list, and if you don't check any log types (App / Sys), the program will check and display the 30 last events of the local machine where the script is executed, from the Application and System event logs.</em></blockquote>
&nbsp;
<blockquote><em>NOTE3: the "Speech" section is in alpha version for now, as 1) I didn't translate every messages into both French and English, and 2) PowerShell will wait until the computer finishes to speak before releasing the interface to you, and I made it speak each time you check a box ... </em></blockquote>
&nbsp;

You can <strong><a target="_blank" href="https://gallery.technet.microsoft.com/GUI-to-search-export-to-6d412882?redir=0" rel="noopener">download the PowerShell Events Collector GUI here</a></strong> - note that I also include the PowerBI Template in the archive, it's optional to use, just in case you'll like to test using PowerBI with event logs analysis some time...

Have a great one,
Cheers
Sam
