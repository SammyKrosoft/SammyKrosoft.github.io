---
layout: post
title: Consoles MMC Exchange 2007 lentes à l’ouverture (message de timeout)
date: 2009-03-27 05:17
author: sammykrosoft
comments: true
categories: [Exchange 2007, Exchange Server 2007, mmc, Powershell, Uncategorized]
---
<p>Tr&egrave;s bon <a href="http://blogs.technet.com/frmcsuc/archive/2009/03/20/lenteur-l-ouverture-des-consoles-exchange.aspx" target="_blank" mce_href="http://blogs.technet.com/frmcsuc/archive/2009/03/20/lenteur-l-ouverture-des-consoles-exchange.aspx">Post de Nicolas Ramouche : lenteurs &agrave; l'ouverture des consoles Exchange</a>, que je me permets de reprendre car l&rsquo;astuce m&eacute;rite d&rsquo;&ecirc;tre republi&eacute;e vu son efficacit&eacute; :-), pour r&eacute;soudre certains cas de lenteurs d&rsquo;ouverture des consoles d&rsquo;administration Exchange 2007, en r&eacute;sum&eacute; :<p>- Cr&eacute;er les deux fichiers suivants:</p><p><strong>%systemroot%system32mmc.exe.config</strong></p><p><strong>%systemroot%system32WindowsPowerShellv1.0powershell.exe.config</strong></p><p>- Ins&eacute;rer le texte suivant (du code XML) dans ces fichiers :</p><p><strong>&lt;configuration&gt; <br>&nbsp; &lt;runtime&gt; <br>&nbsp;&nbsp;&nbsp; &lt;generatePublisherEvidence enabled=&rdquo;false&rdquo; /&gt; <br>&nbsp; &lt;/runtime&gt; <br>&lt;/configuration&gt;</strong></p><p>- Sauvegardez ces deux fichiers</p><p><em>R&eacute;f&eacute;rences : <br></em>- <a href="http://blogs.technet.com/frmcsuc/archive/2009/03/20/lenteur-l-ouverture-des-consoles-exchange.aspx" target="_blank" mce_href="http://blogs.technet.com/frmcsuc/archive/2009/03/20/lenteur-l-ouverture-des-consoles-exchange.aspx">Nicolas Ramouche sur le site de l&rsquo;&eacute;quipe MCS UC</a> <br>- <a href="http://support.microsoft.com/kb/944752/en-us" target="_blank" mce_href="http://support.microsoft.com/kb/944752/en-us">Article KB n&deg;944752 &ndash; Exchange Server 2007 managed code services &hellip;.</a></p><p>Sam.</p></p>

