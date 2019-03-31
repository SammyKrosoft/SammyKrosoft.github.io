---
layout: post
title: All Exchange Versions (pretty much)–How to connect to the System Attendant Mailbox
date: 2011-03-02 12:08
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>For some particular issues, if you don&rsquo;t have the means to create a new System Attendant Mailbox (that is by moving all users mailboxes from the store where the SA Mailbox resides)</p><p>We need the following tool: <a href="http://mfcmapi.codeplex.com/">http://mfcmapi.codeplex.com/</a></p><p>1.) Create an Outlook Profile, in Online Mode.</p><p>2.) Launch MfcMapi.exe, select Session -&gt; Logon Only ( Does Not display stores ), choose the profile created under the previous step, select MDB &ndash; get Mailbox Table.</p><p>3.) Enter the name of the server that owns the store/mailbox you wish to view.</p><p>4.) Double click the SystemMailbox.</p><p>To reset the FreeBusy messages if FreeBusy information are missing when some users are booking meetings, search for the system messages for Free/Busy informations :</p><p>5.) Under SpecialPrivateFolderFreeBusyStorage , you will see the Free Busy Messages, select them, right click them and do a hard delete and you're done.</p><p>&nbsp;</p><p>&nbsp;</p><p>From: Patrick from his blog - <a title="http://blogs.technet.com/b/ehlro/archive/2010/11/01/using-mfcmapi-to-connect-to-the-exchange-system-attendant-mailbox.aspx" href="http://blogs.technet.com/b/ehlro/archive/2010/11/01/using-mfcmapi-to-connect-to-the-exchange-system-attendant-mailbox.aspx">http://blogs.technet.com/b/ehlro/archive/2010/11/01/using-mfcmapi-to-connect-to-the-exchange-system-attendant-mailbox.aspx</a></p></p>

