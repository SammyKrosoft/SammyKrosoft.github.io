---
layout: post
title: PowerShell–Script to export events to screen and/or to a CSV file from one or multiple machines
date: 2018-04-13 09:55
author: sammykrosoft
comments: true
categories: [Exchange, powershell]
---
<span style="color: #ff0000;font-size: medium">*** UPDATE *** Added a GUI wrapped around this script - you can <a href="https://gallery.technet.microsoft.com/GUI-to-search-export-to-6d412882?redir=0">check it out here</a> ...</span>

Type the following to see the latest examples:
<pre>Get-Help .\Get-EventsFromEventLogs.ps1 –Examples</pre>
Type the following to see the full help:
<pre>Get-Help .\Get-EventsFromEventLogs.ps1 –Full</pre>
&nbsp;

Hi all !

<span style="font-size: medium">Today I’ll give you a script that exports the events of your choice (you chose one or more Event IDs you want to export or one or more Event Sources), from one or multiple computers or servers. In this script I am also fixing events which description has carriage returns to ease up the Excel processing as I noticed when exporting Event logs as CSV from the Windows Event Viewer, events which descriptions had carriage returns were spanned across several Excel rows.</span>

<span style="font-size: small"><span style="font-size: medium">By default, the script will only output the events found on screen. You must specify the <span>–ExportToFile</span> switch to export the events found on a CSV file. This CSV file will be located in the same directory where the script is located, and will be formatted like the below - it will have the name or number of the first event D or source that you are searching the logs for:</span></span>
<pre><span><span>GetEventsFromEventLogs_EventIDorEventSource_Year-MONTH-DAY-Hour-Minute-Second.csv</span></span></pre>
<span><span><strong>
- Example:</strong></span></span>
<blockquote>Launching
<pre>.\Get-EventsFromEventLogs.ps1 -Computers Server01,Server02 -EventLevel Error,Critical –ExportToFile</pre>
got me the below file - with before the date the first event ID I was looking for:
<pre>GetEventsFromEventLogs_916_2018-04-13-09-52-08.csv</pre>
Located on the same directory where my script is …</blockquote>
<span style="font-size: small"><strong><span style="font-size: medium">- Other Example of the script’s full execution:</span></strong></span>
<blockquote>
<pre><span>.\Get-EventsFromEventLogs.ps1 -Computers $(Get-ExchangeServer) –EventLogName Application -EventSource "MSExchange ADAccess","MSExchangeADTopology" -NumberOfLastEventsToGet 30 -EventLevel Warning,Error -ExportToFile</span></pre>
<span style="font-size: medium">Note here that I am passing in the <em><strong>–Computers</strong></em> parameter all my Exchange Mailbox servers by using <em><strong>$(Get-MailboxServer)</strong></em> to check all my Exchange servers =&gt; in order for this to work, you must execute the script from an Exchange Management Shell-enabled PowerShell session – otherwise, you can specify a list of computers (<em><strong>-Computers EX01,EX02</strong></em>) or get a list of computer from a file (<em><strong>-Computer $(Get-Content C:\temp\MyServers.txt)</strong></em>)</span>

<span style="font-size: medium">First, I will get the confirmation of the options of the script and I am asked to validate to continue:</span></blockquote>
<a href="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-1-Confirm-parameters.png"><img width="651" height="175" class="alignnone wp-image-4565 size-full" alt="" src="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-1-Confirm-parameters.png" /></a>
<blockquote><span style="font-size: small"><span style="font-size: medium">Then it will run and show me what it’s doing:</span></span></blockquote>
<a href="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-2-Script-Completed-and-stats.png"><img width="995" height="506" class="alignnone size-full wp-image-4595" alt="" src="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-2-Script-Completed-and-stats.png" /></a>
<blockquote>Note that it searches on all computers I specified, displays warnings and errors (30 last events for each machine, as I specified in the script's <strong>–NumberOfLastEventsToGet</strong> parameter) from the 2 servers I have in my environment…

Also Note the summary of all the events found globally at the end of the script.

And finally note how quick it is to search and dump our events ! About half a second !</blockquote>
<blockquote><span style="font-size: small"><span style="font-size: medium">And as it finishes, it opens me the file in a NOTEPAD because I specified the <strong>–ExportToFile</strong> switch on the script launch:</span></span></blockquote>
<a href="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-3-Notepad-and-raw-CSV-view.png"><img width="1248" height="249" class="alignnone size-full wp-image-4575" alt="" src="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-3-Notepad-and-raw-CSV-view.png" /></a>
<blockquote>You can also open the file (or copy/paste the Notepad content) with Excel for filtering, analyzing, correlating, reporting, etc…</blockquote>
<span style="font-size: small"> <a href="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-4-Excel-CSV-view.png"><img width="1708" height="759" class="alignnone size-full wp-image-4605" alt="" src="https://msdnshared.blob.core.windows.net/media/2018/04/Figure-4-Excel-CSV-view.png" /></a></span>

<span style="font-size: small"><strong><span style="font-size: large"> </span></strong></span>

<span style="font-size: small"><strong><span style="font-size: large">Here are the switches you can use:</span></strong></span>
<pre><strong><span></span></strong><strong><span><span style="font-size: medium">-Computers &lt; Object&gt;</span></span></strong><strong><span></span></strong></pre>
<span style="font-size: medium">=&gt; <em><strong>where &lt;Object&gt; default = local computer</strong></em>, <strong><u> you can specify list of computers</u></strong>, strings separated by commas like <strong>-Computers “Server1”, “Server2”,”Server3”</strong> or get the computers from a list like<strong> -Computers $(Get-Content C:\temp\myservers.txt)</strong> or get the computers from a variable that contains a list of Exchange servers, etc…</span>

<span style="font-size: small"><span style="font-size: medium">I tried the below on my Exchange 2013 environment :</span></span>
<pre><span>.\Get-EventsFrom.ps1 <strong>-Computers $(Get-MailboxServer)</strong> -EventIDToCheck 2142 -EventLogName Application -ExportToFile</span></pre>
<span style="font-size: small"><span style="font-size: medium">And it got my last 30 events with Event ID 2142 from all my Exchange servers of my environment !</span></span>
<pre><span style="font-size: small"><strong><span><span style="font-size: medium"> </span></span></strong></span><span style="font-size: small"><strong><span><span style="font-size: medium">-EventLogName &lt; Object&gt;</span></span></strong><strong><span></span></strong></span></pre>
<span style="font-size: small"><span style="font-size: medium">=&gt; <em><strong>where &lt;Object&gt; default = Application and System logs</strong></em>, you can specify the Application LOG only (<em><strong>-EventLogName Application</strong></em>) or specify several event log types separated by commas (<em><strong>-EventLogName Application,  System, Security</strong></em>) </span></span>

<span><em>There is a defined set of Event Log Names that you can use, you can cycle through the valid values for this parameter by hitting &lt;<strong>TAB&gt;</strong> after you specify the <strong>–EventLogName</strong> parameter, and event hitting &lt;TAB&gt; for multiple values will cycle through each possible Event Log Name value. I love that PowerShell parameter validation functionality ! &lt;3</em></span>
<pre><strong>-EventLevel &lt;Level&gt;</strong></pre>
Level must be Information, Warning, Error, and/or Critical. You can specify multiple events type, by separating with commas, like:
<pre>.\Get-EventsFromEventLogs -EventLevel Warning,Error -ComputerName Computer01,Computer02</pre>
If you don't specify anything, all event types will be gathered (by default the first 30 events from each computer)

-EventSource &lt;String&gt;

This can be any source which Event you wish to dump. Example if you are looking for potential warnings and/or errors in Outlook from the local workstation you would use the below :
<pre>.\Get-EventsFromEventLogs -EventLevel Warning,Error -EventSource Outlook</pre>
&nbsp;
<pre><span style="font-size: small"><strong><span><span style="font-size: medium"> </span></span></strong></span><span style="font-size: small"><strong><span><span style="font-size: medium">-EventID &lt; Object&gt;</span></span></strong><strong><span></span></strong></span></pre>
<span style="font-size: small"><span style="font-size: medium"><em><strong><span>=&gt; where &lt;Object&gt; default = “All” </span>, if you don’t specify it, the script will search for all Event IDs</strong></em>, and if you want you can specify a list of IDs to search for or to check : just enter each Event ID separated by commas (like <em><strong>-EventID 1220,2020,605</strong></em>)</span></span>
<pre><span style="font-size: small"><span style="font-size: medium"><strong> </strong></span></span><span style="font-size: small"><span style="font-size: medium"><strong>-EventSource &lt;Object&gt;</strong></span></span></pre>
<span style="font-size: small"><span style="font-size: medium">=&gt; <em><strong>where &lt;Object&gt; default = “All”</strong>, if you don't specify this parameter, the script will search for all Event Sources, and you can also specify a source name like <strong>-EventSource Outlook</strong> (no need for the quotes) or several source names like <strong>-EventSource Outlook, Disk</strong> … </em></span></span><span style="font-size: medium"><em><span style="font-size: small"><span style="font-size: medium"><strong> </strong></span></span></em></span>
<pre><span style="font-size: medium"><span style="font-size: small"><span style="font-size: medium"><strong>-EventLevel &lt;Object&gt;</strong></span></span></span></pre>
<span style="font-size: small"><span style="font-size: medium">=&gt; <em><strong>where &lt;Object&gt; default = “All”</strong>, if you don't specify this parameter, the script will search all Levels (Info, Warning, Errors, Critical, …)</em></span></span>

<span style="font-size: small"><span style="font-size: medium"><em>You can specify an event level like <strong>-EventLevel Error</strong> or several levels like <strong>-EventLevel Warning, Error</strong> … </em></span></span>

<span style="font-size: small"><em><span style="font-size: medium">There is a defined set of event level you can use, you can cycle through the valid values for this parameter by hitting “<strong>TAB</strong>” after you specify the <strong>–EventLevel</strong> parameter. </span></em></span>

<span style="font-size: small"><em><span style="font-size: medium">Possible values are: "All" (default),"Information","Warning","Error","Critical", "Verbose"…</span></em></span>
<pre><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: medium"> </span></span></strong></span></span></span></strong></span></span></span></strong></span></span></span></strong></span></span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: small"><span style="font-size: small"><strong><span><span style="font-size: medium">-NumberOfLastEventsToGet &lt;Int32&gt;</span></span></strong><strong><span></span></strong></span></span></span></strong></span></span></span></strong></span></span></span></strong></span></span></pre>
<span style="font-size: small"><span style="font-size: medium">=&gt; <em><strong>where &lt;Int32&gt; default = 30</strong></em>, and this will dump the 30 or less (if the event log has less, it will dump all of these)</span></span>
<pre><span style="font-size: small"><strong><span><span style="font-size: medium"> </span></span></strong></span><span style="font-size: small"><strong><span><span style="font-size: medium">-ExportToFile [&lt;SwitchParameter&gt;]</span></span></strong><strong><span></span></strong></span></pre>
<span style="font-size: small"><span style="font-size: medium">=&gt; if you don’t specify <strong>-ExportToFile</strong> the script will just dump the events into screen. If you specify it, it will dump the event into a CSV file for easy parsing in Excel !</span></span>

<span style="font-size: small"><span style="font-size: medium">To check the full help of the script, type </span></span>
<pre>get-help .\Get-EventsFromEventLogs.ps1 -Full</pre>
<span style="font-size: small"><span style="font-size: medium">To check the examples only type:</span></span>
<pre>get-help .\Get-EventsFromEventLogs.ps1 -Examples</pre>
<strong> </strong>

<span style="font-size: medium"><a target="_blank" href="https://gallery.technet.microsoft.com/View-or-Export-To-CSV-62ebd49c" rel="noopener"><strong><span>Click here to get to the Download Link on TechNet Gallery</span></strong></a></span>

<span style="font-size: small"><span style="font-size: medium"> </span></span>

<span style="font-size: small"><span style="font-size: medium">Here is the dump of the Examples:</span></span>

&nbsp;
<pre>NAME
 C:\Users\SammyKrosoft\documents\Github\Powershell\Get-EventsFromEventLog\Get-EventsFromEventLogs.ps1
 
SYNOPSIS
 Searches and Get specific events from any computer, local or remote, or from a computer list.
 
 
 -------------------------- EXAMPLE 1 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1
 
 Launching the script without options will :
 - Ask you which event(s) you wish to search for (separated by commas if you want multiple event IDs to search)
 - Search the local computer
 - Search the Application and System logs
 - Get 30 events of the type specified
 
  
 
 -------------------------- EXAMPLE 2 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -Computers MyServers -EventLevel Error
 
 This will collect the Error events (the last 30 errors by default) from the computer named MyServers. 
 It won't store it into a file as we didn't call the "-ExportToFile" parameter, just dump into the screen
 to have an idea if your server is okay or if it's full of errors
 
  
 
 -------------------------- EXAMPLE 3 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -Computers SRV-EX-01,SRV-EX-02,SRV-EX-03 -EventLevel Error -ExportToFile
 
 This will collect the Warning, Error, Critical events on computers SRV-EX-01, 02 and 03. The results
 will be dumped into a file labelled GetEventsFromEventLogs-Date-time.csv as we specified the
 ExportToFile parameter.
 Note that the computers list can come from a txt file as well (see next example)
 
  
 
 -------------------------- EXAMPLE 4 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventsLogs.ps1 -Computers $(Get-Content .\ServersList.txt) -EventLevel Error,Critical -ExportToFile
 
 This will collect Error and Critical events on computers list defined in the "ServersList.txt" file on the current 
 directory from where you launched the script (.\ refers to the current user directory, NOT the directory where the
 script is) and store it into a file.
  
 
 
 -------------------------- EXAMPLE 5 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -NumberOfLastEventsToGet 10 -EventID 916,105 -ExportToFile
 
 - Search for the 10 last events (-NumberOfLastEventsToGet 10) 
 - Search for event IDs 916 and 105
 - As no Event Log name (Application, System, Security, etc...) were specified, 
 the script will look inside the Application AND System logs by default.
 - We asked the script to look for Event IDs 916 and 105 (-EventID 916, 105)
 
 The exported file will be named GetEventsFromEventLogs_916-105_2018-04-13-10-01-55.csv
 as I ran the script on 13th April 2018 at 10h01 and 55 seconds in the morning.
  
 
 
 -------------------------- EXAMPLE 6 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -NumberOfLastEventsToGet 30 -EventID 26 -EventLogName Application
 
 - Search for the last 30 events (-NumberOfLastEventsToGet 30)
 - Search for Event ID 26 only
 - Search in the Application Log only
 - We don't output any file, just print the results on the screen
 
  
 
 -------------------------- EXAMPLE 7 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -EventSource "Outlook"
 
 - Search all events generated by the "Outlook" application (all Event IDs, all Level (Info, Warning, etc...))
 - Search in Application and System (because I didn't specify which event log)
 - Search the last 30 events of type "Outlook" - if there are less, it will just print less
 - We don't output any file because I didn't specify the -ExportToFile parameter
 
 MachineName LogName TimeCreated LevelDisplayName Id Message
 ----------- ------- ----------- ---------------- -- -------
 12345678901 Application 4/13/2018 11:57:06 AM Information 63 La demande de service web Exchange GetAppManifestssuccÃ¨de Ã .&lt;/0w&gt;
 12345678901 Application 4/13/2018 7:57:00 AM Information 63 La demande de service web Exchange GetAppManifestssuccÃ¨de Ã .&lt;/0w&gt;
 12345678901 Application 4/13/2018 7:56:59 AM Information 63 Outlook a dÃ©tectÃ© une notification de modification pour vos applications et va t...
 12345678901 Application 4/13/2018 7:56:55 AM Information 45 Outlook a chargÃ© le(s) complÃ©ment(s) suivant(s) :...
  
 
 
 -------------------------- EXAMPLE 8 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -EventSource "disk","Outlook" -EventLevel Warning -NumberOfLastEventsToGet 1000
 
 - Search all events which source are "Disk" and "Outlook"
 - Search only "Warning" events of the above defined sources
 - All Event IDs of these (because I didn't specify any ID to filter)
 - Get the 1000 last events of the above criteria
 - didn't specify the -ExportToFile so will just display to screen
  
 
 
 -------------------------- EXAMPLE 9 --------------------------
 
 PS C:\&gt;.\Get-EventsFromEventLogs.ps1 -EventSource "disk" -NumberOfLastEventsToGet 1000 -EventLevel Critical,Warning,Error -ExportToFile
 
 - Search all events about the "disk"
 - Search only Critical, Warning and Error events
 - Search the 1000 last events about the above criteria
 - Export into a file (like GetEventsFromEventLogs_None_2018-04-14-04-34-27.csv)</pre>
&nbsp;

&nbsp;

<span style="font-size: medium"><a target="_blank" href="https://gallery.technet.microsoft.com/View-or-Export-To-CSV-62ebd49c" rel="noopener"><strong>Download link (same as above)</strong></a></span>

<span style="font-size: medium">Try it and let me know your thoughts !</span>

<span style="font-size: medium">Cheers</span>

<span style="font-size: small"><span style="font-size: medium">Sam</span></span>
