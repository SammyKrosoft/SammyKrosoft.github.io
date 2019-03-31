---
layout: post
title: Outlook 2010/2013/206–General recommendations to avoid latencies, crashes, or general bad user experience
date: 2018-03-08 10:25
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
Since I started to work with Outlook and Exchange in 1999, I am still seeing Outlook clients used or configured with bad practices in 2018. And I have to repeat or resend the same links. That’s why I think it useful to re-write a more recent post about what we, as Microsoft on-site, hands-on engineers, recommend regarding how to configure Outlook regarding the use of PSTs, file-level anti-virus exclusions, and some add-ins that scan e-mails already in Outlook or scan e-mail as users are writing them, or as users are attaching files to it, or as users are clicking on the “Send” button.

&nbsp;

Microsoft do not recommend using an Outlook Antivirus Add-In (confirmed at the <a href="https://technet.microsoft.com/en-us/library/dn769141.aspx">bottom of this article</a>), as not only it is known to badly impact the user experience, but also it’s redundant not only to <b>file level antivirus that is already installed on the Windows Desktop</b>, that will already scan any unwanted files reaching your hard-drive wherever these are : downloaded from a browser, downloaded from Outlook via saving an attachment, etc… . It’s also redundant to the following Anti-Virus that we hope you usually have at the server and gateway level:
<ul>
 	<li><b>At the Exchange server level</b> –&gt; Server e-mail based antivirus engines are scanning e-mails in the database as well as e-mails in transit, outgoing and incoming</li>
 	<li><b>At the Gateway level</b> –&gt; Gateway level (SMTP appliances, Exchange Edge servers, etc…) anti-malware engines are scanning outgoing and incoming e-mails as well – Microsoft recommends using <a href="https://products.office.com/en-ca/exchange/exchange-email-security-spam-protection">Exchange Online Protection</a> to scan your incoming / outgoing e-mails. On Exchange Online Protection, we use at least 3 different Anti-Virus engines that are constantly updated to block threats.</li>
</ul>
<img alt="A chart showing how Exchange Online Protection protects your organization’s email." src="https://c.s-microsoft.com/en-ca/CMSImages/Image_Security_Reliability_713x300.png?version=d542f29f-c17c-ccff-ff6d-184cc11c49c1" />

&nbsp;

Also, using Outlook-side DLP add-ins can affect the responsiveness of the Outlook client and lead to bad user experience – in Exchange 2013 and forward, <strong>DLP functionality is also included on the server side</strong>. The advice here would be to work with your security team to shift any custom DLP requirements you have to the Server side. Here’s an overview of <a href="https://blogs.technet.microsoft.com/exchange/2014/02/25/data-loss-prevention-in-exchange-just-got-better/">DLP on Exchange 2013</a>, and an overview of <a href="https://technet.microsoft.com/en-us/library/jj150527(v=exchg.160).aspx">DLP in Exchange 2016</a>.

Finally, here are some general recommendation regarding Outlook configuration and behavior – some include the above to highlight the importance of these recommendations :
<ul>
 	<li><strong>Ensure no PST files hosted on a network share</strong>
<ul>
 	<li>Sometimes “My Documents” are redirected to a network share for backup purposes</li>
 	<li>Also ensure you don’t have too big PST files as the biggest these are, the most sensitive to corruption these are as well – choose smaller PSTs, or best : no PSTs at all – rely on Exchange Archiving (archive your e-mails <a href="https://technet.microsoft.com/en-us/library/dd979800(v=exchg.160).aspx">On Prem using In-Place archiving servers</a> or online using <a href="https://products.office.com/en-ca/exchange/microsoft-exchange-online-archiving-email">Exchange Online O365 servers</a>) or any other archiving solution you may already have.</li>
 	<li><b>Consequences having PSTs on a network share:</b> at best, outlook freezes and crashes, at worst PST corruption, between these, weird behaviors like messages staying in the Outbox …</li>
</ul>
</li>
 	<li><strong>Anti-Virus / general e-mail scanning Outlook Add-Ins are known to cause user experience issues</strong>
<ul>
 	<li>Client side DLP or anti-virus Add-ins : scans e-mail and attachments while attaching and/or sending =&gt; slows down user experience</li>
 	<li>DLP functions are already included in Exchange 2013/2016</li>
 	<li>Microsoft recommends scanning e-mails at the server and at the gateway level, and best of the best, use Exchange Online Protection (see the schema and links above)</li>
 	<li>On top of that, you most likely already have <strong>file level anti-virus scanning enabled on workstations, </strong>scanning files in real time (“on-file created” scan) which protects already your company in case a malicious attachments makes its way up to the Desktop file system.</li>
 	<li><b><u>Recommendation</u></b>: rely on <a href="https://technet.microsoft.com/en-us/library/jj150527(v=exchg.160).aspx">Exchange Data Leak Protection</a> and <a href="https://technet.microsoft.com/en-us/library/jj150547(v=exchg.160).aspx">Antimalware</a> included in Exchange 2013/2016 and your gateway(s) and/or Exchange Online Protection if you want to scan your messages multiple times.</li>
</ul>
</li>
 	<li><strong>Check Outlook file exclusions by the File level antivirus</strong> – there are plenty of articles explaining that you have to exclude Outlook files, especially if you’re using Outlook cache mode. Here are the main ones:
<ul>
 	<li><a href="https://technet.microsoft.com/en-us/library/dn769141.aspx">Exclude certain Outlook files from being scanned</a></li>
 	<li><a href="https://technet.microsoft.com/en-us/library/dn769141.aspx">Planning antivirus scanning for Outlook 2013/2016</a></li>
 	<li><a href="https://blogs.technet.microsoft.com/samdrey/2014/06/20/outlook-antivirus-exclusions-reminder/">TechNet post from myself on antivirus exclusions to set on Outlook workstation</a></li>
</ul>
</li>
 	<li><strong>Recommendation is to NOT use any other e-mail desktop indexing software than Windows indexing engine</strong></li>
 	<li>
<ul>
 	<li>If you use 3rd party Desktop general purposes indexing software, exclude e-mails indexing</li>
 	<li>Outlook 2013 blocks the ability to search for Outlook items by using Windows Desktop Search – but it uses Windows indexing capabilities for instant search – users just don’t access Outlook search through Windows Desktop Search, but Outlook interface directly instead</li>
 	<li><strong>Reference</strong>: <a href="https://support.microsoft.com/en-ca/help/2769651/outlook-search-returns-no-matches-found">article explaining what to do when Outlook instant search is not working</a></li>
</ul>
</li>
 	<li><strong>OST : Outlook cache mode is recommended to improve client experience.</strong></li>
 	<li>
<ul>
 	<li>As part of Outlook files exclusions for the File Level antivirus, OST must NOT be scanned</li>
 	<li>OST file must NOT be stored on a Network Share as well (check that your user profile – pointed by the %UserProfile% environment variable – is not on a network share)</li>
</ul>
</li>
</ul>
&nbsp;

Also a <a href="https://blogs.technet.microsoft.com/samdrey/2012/10/10/outlook-20072010some-basic-advice-if-you-experience-odd-behaviour-startup-latencies-calendar-item-losses/">place to start to investigate about outlook issues</a> – a bit old but some points are still applicable in 2018…

&nbsp;

Hope this helps,

Kindest regards,

Sam
