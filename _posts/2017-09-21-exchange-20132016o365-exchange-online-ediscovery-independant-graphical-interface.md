---
layout: post
title: Exchange 2013/2016/O365 Exchange Online – eDiscovery independent Graphical User Interface
date: 2017-09-21 21:37
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Hi all,

today I’m giving you a tool that proved its usefulness along the past couple of years for customers of mine. It works on Exchange 2013, Exchange 2016, and on Exchange Online from Office 365 from where it was designed: <b>an Exchange Search (eDiscovery) tool </b>that <strong>generates</strong> and executes<strong> Search-Mailbox</strong> (“single mailbox search”) or <strong>New-MailboxSearch</strong> (“multi-mailbox search”) cmdlets (you choose) with the most common options and search requests.

Some might ask me, why this tool, we already have the HTML based Compliance and eDiscovery page within the EAC? Here are the thoughts that let me to come up with this tool…

<strong><a href="#DownloadLink"><em>Note: Download Link is at the very end of this article</em></a></strong>

<b><span style="font-size: large">Challenges</span></b>

Here are a few challenges we found with HTML based interfaces:

- we have to be logged in an Exchange Control Panel or Exchange Admin Center to get to the eDiscovery interface, then sometimes wait for the page rendering on slower connections
- we don’t necessarily clearly see or understand when we’re using <strong>Search-Mailbox</strong> (single mailbox search, provides ability to delete unwanted or malicious mails provided admin has the <a target="_blank" href="https://blogs.technet.microsoft.com/samdrey/2011/02/16/exchange-2010-rbac-issue-mailbox-import-export-new-mailboximportrequest-and-other-management-role-entries-are-missing-not-available/" rel="noopener">Mailbox Import Export permissions</a>) or <strong>New-MailboxSearch</strong> (multi-mailbox search, but we can’t delete mails with it)
- we don’t see the underlying Powershell command line and options that are used
- we cannot choose any mailboxes to use as “Discovery Mailboxes”,
- some other limitations inherent to HTML based interfaces that doesn’t come in mind as I am writing this note…

Here are a few challenges with Powershell only Search-Mailbox / New-MailboxSearch:

- It’s easy to make typo mistakes
- we don’t necessarily have the cmdlet parameters handy to quickly perform a search or a mailbox purge – well you should know the “get-help” cmdlet with the –Full / –Details or –Examples parameters to get what you want.
- some IT administrators are simply not very used to or fond of the command line…

<b><span style="font-size: large">Solution</span></b>

… this <strong>Graphical User Interface</strong> (I’ll refer to this as “<strong>GUI</strong>” from now on) that guides you in the type of search you wish to perform, including purging unwanted or malicious e-mails from users mailboxes, and also including the ability to dynamically generate the Powershell command line as you graphically define your search / purge options.

Some dreamt about it, I made it real and sharing it here.

<img width="184" height="293" title="DVDBox Back - 1.1" style="border: 0px currentcolor" alt="DVDBox Back - 1.1" src="https://msdnshared.blob.core.windows.net/media/2017/09/DVDBox-Back-1.11.png" border="0" />

<b><u>Credits for starting point</u></b>

Initially I begun to start from scratch, but then I found a mate who did a simple interface using Search-Mailbox only, I then expanded this example to include a real-time cmdlet generator that you can copy/paste in another Exchange Management Shell window or on a documentation  for future references, and also included the choice to use either Search-Mailbox or New-MailboxSearch, and also included tabs to connect to remote Exchange organizations (Exchange Online, or remote Exchange OnPrem for example), and tabs to get the search statistics details, etc…

<strong>BIG THANKS TO PRATEEK SINGH</strong>

His script gave me the base for this application:
Based this tool on a script from the great Prateek Singh : <a href="http://en.gravatar.com/almostit">http://en.gravatar.com/almostit</a> - great guy !
And the page of his script is here:
<a href="https://geekeefy.wordpress.com/2016/02/26/powershell-gui-based-email-search-and-removal-in-exchange/">https://geekeefy.wordpress.com/2016/02/26/powershell-gui-based-email-search-and-removal-in-exchange/</a>

<strong><span style="font-size: large">Quick Users Guide</span></strong>

<a><strong>1 Connecting to an Exchange environment if needed</strong></a>

If you are not running the tool from an Exchange Management Shell already, you can either:

- Connect to your Office 365 Exchange tenant by clicking on the “Connect to Exchange” button (check that the below label says “O365 Exchange”

- connect to an On-Premise (local) Exchange environment by checking the “On-Premise Exchange” box and fill in the Exchange environment’s URL

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image00216.png"><img width="504" height="536" title="clip_image002" alt="clip_image002" src="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image002_thumb10.png" border="0" /></a>

<a>Figure </a>1 - Connect to O365 tenant

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image00411.png"><img width="504" height="539" title="clip_image004" alt="clip_image004" src="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image004_thumb7.png" border="0" /></a>

<a>Figure </a>2 - Connect to On-Premise Exchange 2010, 2013 or 2016

<b><i><u>NOTE</u></i></b><i>: if you run the tool from an Exchange Management Shell window, you won’t have to connect – you’ll already be connected, and the tool will show it to you ! But you can still use the “Connect to Exchange” button to connect either to another Exchange tenant/environment, or connect using other user’s credentials.</i>

Ø Then check the rights you have by checking the “Connection Status” information:

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image0067.png"><img width="504" height="536" title="clip_image006" alt="clip_image006" src="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image006_thumb6.png" border="0" /></a>

<a>Figure </a>3 - Connected to Exchange, but no Discovery Management rights

As you see above, you are connected to Exchange (i.e. you have the main Exchange Powershell cmdlets available), but you don’t have the rights to search (need the “Discovery Management” RBAC role membership) =&gt; you won’t be able to execute any Mailbox search commands.

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image0081.png"><img width="504" height="527" title="clip_image008" alt="clip_image008" src="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image008_thumb1.png" border="0" /></a>

<a>Figure </a>4 - Connected to Exchange, have Discovery Management rights, but no Mailbox Import Export rights mandatory to be able to purge searched items from source mailbox

Here you can see that you can definitely search using all options from the tool (Search-Mailbox, and New-MailboxSearch). You just cannot purge e-mails from mailboxes.

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image0102.png"><img width="504" height="539" title="clip_image010" alt="clip_image010" src="https://msdnshared.blob.core.windows.net/media/2017/09/clip_image010_thumb2.png" border="0" /></a>

<a>Figure </a>5 - Discovery Management + delete with Search-Mailbox rights brought by Mailbox Import Export rights

And finally the green status above shows that you can search for mail, but you can also purge the mails that you searched for if needed (for example to remove any sensitive e-mails that mailboxes shouldn’t have received, or remove any malicious e-mails, …)

<a><strong>2 Search e-mails</strong></a>

This is the core of the tool – you can launch e-mail searches.

- Specify as many mailboxes to search in as you want (can be email addresses, display names, aliases, or a mix of all of these)

<i>NOTE: New-MailboxSearch is just </i><a href="https://technet.microsoft.com/en-CA/library/dn798915%28v=exchg.150%29.aspx"><i>limited to 10,000 mailboxes</i></a><i> so the alternative is either to use Search-Mailbox (uncheck the blue highlighted checkbox) or to do several searches with 10,000 mailboxes each.</i>

- View the corresponding search command line that builds up as you type – you can even copy/paste it in any Exchange Management Shell window or in a document …

- Default checked boxes will use New-MailboxSearch, and only estimate the number of findings and their sizes

<i>NOTE: Uncheck the “Estimate only” check box to copy the e-mails from searches results on the Discovery Mailbox.</i>

<i>NOTE: only the “Search-Mailbox” method enable purging e-mails from a source mailbox (for example in case of a phishing e-mail, or a sensitive e-mail that has been sent to many users by mistake, or a mail with malicious links, etc…)</i>

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/image252.png"><img width="704" height="500" title="image" alt="image" src="https://msdnshared.blob.core.windows.net/media/2017/09/image_thumb225.png" border="0" /></a>

<a>Figure </a>6 - Exchange eDiscovery aka Search interface with search options and dynamic Exchange Powershell command line

<i>NOTE: By default, the “Search a bit quicker using New-MailboxSearch (cannot delete mail)” check box will be checked, along with the “Estimate only” check box.</i>

<a><strong>3 Check the status of your search (searches launched with New-Mailbox only)</strong></a>

Just click on the “<i>Get previous mailbox search</i>” button to check when the Exchange server will have the search complete. The other button will show the status of all previous searches (only the ones performed with “New-MailboxSearch” – see previous Tab):

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/image253.png"><img width="704" height="596" title="image" alt="image" src="https://msdnshared.blob.core.windows.net/media/2017/09/image_thumb226.png" border="0" /></a>

<a>Figure </a>7 - Getting New-MailboxSearch progress on server side...

<a><strong>4 View search statistics, delete previous searches, directory access the corresponding Discovery Mailbox</strong></a>

This tab enables you to retrieve the statistics of searches, and as a bonus, will give you the link to connect directly, using OWA, to the Discovery Mailbox used for that given search.

Note that just like the previous Tab, these information are retrieved for the searches performed with the New-MailboxSearch cmdlet:

<a href="https://msdnshared.blob.core.windows.net/media/2017/09/image254.png"><img width="704" height="450" title="image" alt="image" src="https://msdnshared.blob.core.windows.net/media/2017/09/image_thumb227.png" border="0" /></a>

<a>Figure </a>8 - Getting search statistics and Discovery Mailbox OWA link...

<strong><span style="font-size: large">Download link</span></strong>

Download the tool <a target="_blank" href="https://gallery.technet.microsoft.com/Exchange-eDiscovery-GUI-7a36223c" rel="noopener">here…</a>

<a target="_blank" href="https://gallery.technet.microsoft.com/Exchange-eDiscovery-GUI-7a36223c" rel="noopener" id="DownloadLink"><img width="144" height="236" title="image" alt="image" src="https://msdnshared.blob.core.windows.net/media/2017/09/image257.png" /></a>
