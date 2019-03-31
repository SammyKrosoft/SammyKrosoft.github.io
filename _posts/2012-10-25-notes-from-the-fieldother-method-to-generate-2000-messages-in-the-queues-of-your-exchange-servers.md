---
layout: post
title: Notes from the field–Other method to generate 2000 messages in the queues of your Exchange servers
date: 2012-10-25 13:46
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>&nbsp;</p><p>1) Just create a text file and put in the following   <br></p><p>from: <a href="mailto:blabla@domain.com">blabla@domain.com</a>     <br>to: <a href="mailto:Recipient@domain.com"><u></u><u></u><u>Recipient@domain.com</u></a>    <br>subject: pick up folder test    <br>This is the body    <br></p><p>2)Then save the file as EML file extension   <br>3)stop the transport sevrice    <br>4)copy and paste the file to the pickup folder    <br>If you hold contol-v it will paste it constantly, then just monitor the folder item count until it's pastes it to the amount of messages you want to queue    <br></p><p>More information on <a href="http://support.microsoft.com/kb/297700">http://support.microsoft.com/kb/297700</a></p><p>&nbsp;</p><p>Thanks to my colleague and buddy <strong>Ed Bringas </strong>(former US Escalation Engineer, now Canadian Senior PFE) for this other tip !</p><p>&nbsp;</p><p>Sam</p></p>

