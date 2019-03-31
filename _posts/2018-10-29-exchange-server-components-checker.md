---
layout: post
title: Exchange Server 2013/2016/2019 Components Checker
date: 2018-10-29 22:15
author: sammykrosoft
comments: true
categories: [Exchange, Exchange 2013, exchange 2016, Managed Availability, Server Components, Uncategorized]
---
Here’s the latest addition in Exchange 2013/2016/2019 server quick tools. Sometimes, for maintenance or issues detected by Exchange Managed Availability, some components like Client Access, Autodiscover, Mail flow, etc... can be down on Exchange servers, which "exclude" these servers from the pool of Exchange servers. Here's a tool that helps you to check the state of every Exchange Server’s components and optionally start these if they are inactive (<a href="#download"><strong>tool download link at the end of this article</strong></a>)

NOTE: this tool needs to be run from an Exchange Management Shell, or from a PowerShell host where you imported an Exchange PowerShell session

These “Server Components” were introduced back in Exchange 2013, and are part of the Exchange Managed Availability features, and are there to ease server maintenance: inactive components are not targeted by the Client Access Services so that you can perform maintenance without having to affect the other Exchange services.
<ul>
 	<li>Choose your server version, Exchange 2013 or Exchange 2016 (Exchange 2019 coming soon), to check the components from</li>
 	<li><strong>[ ] CheckOnly</strong> checkbox will just check and show each component from each of your organization’s servers: if checked, the tool will not attempt to start the inactive components</li>
 	<li><strong>[ ] HybridServer</strong> checkbox will check two additional components that are used on Hybrid Exchange – O365 scenarios</li>
 	<li><strong>[ ] Show Inactive Only</strong> : this checkbox will filter out all the Active components, so that you can check right away which servers and which components have Inactive services. You can check or uncheck it anytime, before or after the Server Components collection</li>
 	<li>Once you are happy with the chosen options, click the <strong>[Run]</strong> button to start gathering the information (at the bottom is the status bar along with a status label)</li>
</ul>
<a href="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/blob/master/DocResources/image0.jpg"><img alt="screenshot1" src="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/raw/master/DocResources/image0.jpg" /></a>

NOTE: once the components are gathered and show up, you can select all (click on one of the result cell, then CTRL+ a followed by CTRL + v) and paste it in a report, in Excel:

<a href="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/blob/master/DocResources/image1.jpg"><img alt="screenshot2" src="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/raw/master/DocResources/image1.jpg" /></a>

And finally, you're able to start the Exchange Server Components with the requester of your choice (Maintenance, SideLine, Functional, Deployment,...) - just uncheck the <strong>[ ] CheckOnly</strong> check box, choose your requester, and click <strong>[Run]</strong>

<a href="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/blob/master/DocResources/image2.jpg"><img alt="screenshot3" src="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck/raw/master/DocResources/image2.jpg" /></a>

More information on these Exchange Server Components: <a href="https://blogs.technet.microsoft.com/exchange/2013/09/26/server-component-states-in-exchange-2013/" id="download">blogs.technet.microsoft.com/exchange/2013/09/26/server-component-states-in-exchange-2013</a>
&nbsp;

<a target="_blank" href="https://gallery.technet.microsoft.com/Exchange-Server-Component-855076f7" rel="noopener"><strong>Download this tool here.</strong></a>

&nbsp;

You can also retrieve this project on <a target="_blank" href="https://github.com/SammyKrosoft/Start-E2016ServerComponentsCheck" rel="noopener">this page of my GitHub site</a>...
