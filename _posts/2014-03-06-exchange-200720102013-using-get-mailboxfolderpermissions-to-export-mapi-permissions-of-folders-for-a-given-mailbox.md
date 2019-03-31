---
layout: post
title: Exchange 2010/2013 – Using Get-MailboxFolderPermissions to export MAPI permissions of folders for a given mailbox
date: 2014-03-06 10:53
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
EDIT : This does NOT work with Exchange 2007 as "Get-MailboxFolderPermissions" is not an available cmdlet on E2007 - Use EWS or PFDavAdmin to get these permissions ...

Thanks <strong>Chris, Manfred and friends</strong> from the following article:

<a title="http://social.technet.microsoft.com/Forums/exchange/en-US/5ad656a5-fe70-477f-a608-0e588096f227/how-to-get-mailbox-folder-permissions-to-all-mailbox-folders-in-all-mailboxes?forum=exchangesvradminlegacy" href="http://social.technet.microsoft.com/Forums/exchange/en-US/5ad656a5-fe70-477f-a608-0e588096f227/how-to-get-mailbox-folder-permissions-to-all-mailbox-folders-in-all-mailboxes?forum=exchangesvradminlegacy">http://social.technet.microsoft.com/Forums/exchange/en-US/5ad656a5-fe70-477f-a608-0e588096f227/how-to-get-mailbox-folder-permissions-to-all-mailbox-folders-in-all-mailboxes?forum=exchangesvradminlegacy</a>

Manfred Preissner : <a title="http://social.technet.microsoft.com/profile/manfred%20preissner/?ws=usercard-mini" href="http://social.technet.microsoft.com/profile/manfred%20preissner/?ws=usercard-mini">http://social.technet.microsoft.com/profile/manfred%20preissner/?ws=usercard-mini</a>

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$SpecialExchangeFolders</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">=</span> <span style="color: maroon;font-family: 'Courier New';font-size: 12pt">"Top of Information Store|Recoverable Items|Deletions|Purges|Versions"</span></div>
&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$CurrentUser</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">=</span> <b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">gci</span></b><span style="color: black;font-family: 'Courier New';font-size: 12pt"> env:username | </span><b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">%</span></b><span style="color: black;font-family: 'Courier New';font-size: 12pt"> { </span><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$_</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">.value } </span></div>
&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: black;font-family: 'Courier New';font-size: 12pt">[</span><span style="color: teal;font-family: 'Courier New';font-size: 12pt">string</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">[]] </span><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$FolderPaths</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">=</span> <b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">Get-MailboxfolderStatistics</span></b> <span style="color: purple;font-family: 'Courier New';font-size: 12pt">$CurrentUser</span><span style="color: black;font-family: 'Courier New';font-size: 12pt"> | </span><b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">%</span></b><span style="color: black;font-family: 'Courier New';font-size: 12pt"> {</span><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$_</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">.folderpath} </span></div>
&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$ExchangeFolderPaths</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">=</span> <span style="color: purple;font-family: 'Courier New';font-size: 12pt">$FolderPaths</span><span style="color: black;font-family: 'Courier New';font-size: 12pt"> | </span><b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">%</span></b><span style="color: black;font-family: 'Courier New';font-size: 12pt"> {</span><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$CurrentUser</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">+</span> <span style="color: maroon;font-family: 'Courier New';font-size: 12pt">":"</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">+</span> <span style="color: purple;font-family: 'Courier New';font-size: 12pt">$_</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">.replace(</span><span style="color: maroon;font-family: 'Courier New';font-size: 12pt">'/'</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">,</span><span style="color: maroon;font-family: 'Courier New';font-size: 12pt">'\'</span><span style="color: black;font-family: 'Courier New';font-size: 12pt">)} </span></div>
&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$UsableExchangeFolderPaths</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">=</span> <span style="color: purple;font-family: 'Courier New';font-size: 12pt">$ExchangeFolderPaths</span><span style="color: black;font-family: 'Courier New';font-size: 12pt"> | </span><b><span style="color: cadetblue;font-family: 'Courier New';font-size: 12pt">where</span></b><span style="color: black;font-family: 'Courier New';font-size: 12pt"> { </span><span style="color: purple;font-family: 'Courier New';font-size: 12pt">$_</span> <span style="color: red;font-family: 'Courier New';font-size: 12pt">-notmatch</span> <span style="color: purple;font-family: 'Courier New';font-size: 12pt">$SpecialExchangeFolders</span><span style="color: black;font-family: 'Courier New';font-size: 12pt"> } </span></div>
&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"><span style="color: purple;line-height: 107%;font-family: 'Courier New';font-size: 12pt">$UsableExchangeFolderPaths</span><span style="color: black;line-height: 107%;font-family: 'Courier New';font-size: 12pt"> | </span><b><span style="color: cadetblue;line-height: 107%;font-family: 'Courier New';font-size: 12pt">%</span></b><span style="color: black;line-height: 107%;font-family: 'Courier New';font-size: 12pt"> { </span><b><span style="color: cadetblue;line-height: 107%;font-family: 'Courier New';font-size: 12pt">get-mailboxfolderPermission</span></b> <span style="color: purple;line-height: 107%;font-family: 'Courier New';font-size: 12pt">$_</span><span style="color: black;line-height: 107%;font-family: 'Courier New';font-size: 12pt"> } | </span><b><span style="color: cadetblue;line-height: 107%;font-family: 'Courier New';font-size: 12pt">ft</span></b><span style="color: black;line-height: 107%;font-family: 'Courier New';font-size: 12pt"> FolderName, User, </span><span style="color: maroon;line-height: 107%;font-family: 'Courier New';font-size: 12pt">Acces</span><span style="color: black;line-height: 107%;font-family: 'Courier New';font-size: 12pt"> sRights, </span><span style="color: maroon;line-height: 107%;font-family: 'Courier New';font-size: 12pt">Identity</span> <i><span style="color: cadetblue;line-height: 107%;font-family: 'Courier New';font-size: 12pt">-auto</span></i></div>
&nbsp;

&nbsp;

&nbsp;
<div style="line-height: normal;margin-bottom: 0pt"></div>
<span style="font-family: Lucida Console;font-size: small"><strong><u>Result will be like :</u></strong></span>

&nbsp;

<span style="font-family: Lucida Console;font-size: small">FolderName                   User      AccessRights
----------                   ----      ------------
Calendar                     Default   {Owner}
Calendar                     Anonymous {None}
Calendar                     User177   {CreateItems, EditOwnedItems}
Calendar                     User1769  {CreateItems, EditOwnedItems}
Calendar                     User1768  {CreateItems, EditOwnedItems}
Calendar                     User1767  {CreateItems, EditOwnedItems}
Calendar                     User1766  {CreateItems, EditOwnedItems}
Calendar                     User1765  {CreateItems, EditOwnedItems}
Calendar                     User1764  {CreateItems, EditOwnedItems}
Calendar                     User1763  {CreateItems, EditOwnedItems}
Calendar                     User1762  {CreateItems, EditOwnedItems}
Calendar                     User1761  {CreateItems, EditOwnedItems}
Calendar                     User1760  {CreateItems, EditOwnedItems}
Contacts                     Default   {None}
Contacts                     Anonymous {None}
Conversation Action Settings Default   {None}
Conversation Action Settings Anonymous {None}
Deleted Items                Default   {None}
Deleted Items                Anonymous {None}
Drafts                       Default   {None}
Drafts                       Anonymous {None}
Inbox                        Default   {None}
Inbox                        Anonymous {None}
Journal                      Default   {None}
Journal                      Anonymous {None}
Junk E-Mail                  Default   {None}
Junk E-Mail                  Anonymous {None}
News Feed                    Default   {None}
News Feed                    Anonymous {None}
Notes                        Default   {None}
Notes                        Anonymous {None}
Outbox                       Default   {None}
Outbox                       Anonymous {None}
Quick Step Settings          Default   {None}
Quick Step Settings          Anonymous {None}
RSS Feeds                    Default   {None}
RSS Feeds                    Anonymous {None}
Sent Items                   Default   {None}
Sent Items                   Anonymous {None}
Suggested Contacts           Default   {None}
Suggested Contacts           Anonymous {None}
Tasks                        Default   {None}
Tasks                        Anonymous {None}</span>

&nbsp;

Cherry on the cake, visualizing the number of permissions per folder in an Excel graph.

To do so, just copy paste the result above, OR in your script, export the results into a CSV file (Export-CSV can do the trick, as well as Out-File or redirecting “&gt;” into a file, up to you !) and then “Text to columns” Excel menu, then “Format as table” and then “Insert pivot graph”, et voilà :

<a href="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/4645.image_6.png"><img title="image" src="https://msdnshared.blob.core.windows.net/media/TNBlogsFS/prod.evol.blogs.technet.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/73/61/metablogapi/1462.image_thumb_2.png" alt="image" width="526" height="313" border="0" /></a>

&nbsp;

This is just an example showing what we can do with Excel and a Cut/n/Paste from Powershell raw data…
