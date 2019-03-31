---
layout: post
title: Exchange Server 2010 (RTM et SP1)–Calendar Repair Agent
date: 2011-04-05 19:21
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>Des rendez-vous qui disparaissent chez les organisateurs de r&eacute;unions ou chez les participants ?</p><p>L&rsquo;agent de r&eacute;paration de calendrier est votre ami !</p><p>&nbsp;</p><p>Sous Exchange 2010 RTM, l&rsquo;agent de r&eacute;paration de Calendrier (Calendar Repair Agent (CRA)) fonctionnait avec la mise en place d&rsquo;un planning d&rsquo;ex&eacute;cution configur&eacute; avec le param&egrave;tre CalendarRepairSchedule.</p><p><strong>Exemple :</strong></p><p>Avant, on configurait le CRA pour un fonctionnement tous les jours de la semaine de 22:00 &agrave; 23:00 :</p><p>Set-MailboxServer -Identity MBX2 -CalendarRepairSchedule 1.22:00-1.23:00, 2.22:00-2.23:00, 3.22:00-3.23:00, 4.22:00-4.23:00, 5.22:00-5.23:00, 6.22:00-6.23:00, 7.22:00-7.23:00</p><p>&nbsp;</p><p>Sous Exchange 2010 SP1, le CRA n&rsquo;utilise plus de planification, mais une fr&eacute;quence de mise &agrave; jour (throttle en Anglais). On sp&eacute;cifie cette fois le param&egrave;tre CalendarRepairWorkCycle : le CRA finira le traitement de toutes les bo&icirc;tes aux lettres du serveur indiqu&eacute; dans le temps sp&eacute;cifi&eacute; au niveau de ce param&egrave;tre.</p><p>La liste des bo&icirc;tes aux lettres trait&eacute;es sera mise &agrave; jour dans l&rsquo;intervalle d&eacute;fini par le param&egrave;tre CalendarRepairWorkCycleCheckpoint.</p><p>&nbsp;</p><p><strong>Exemple:</strong></p><p>Pour obliger le CRA &agrave; terminer le traitement de toutes les bo&icirc;tes aux lettres sur le serveur MBX1 dans une p&eacute;riode de 6 heures, et mettre &agrave; jour la liste des bo&icirc;tes aux lettres &agrave; traiter toutes les 30 minutes, on lancera :</p><p>Set-MailboxServer &ndash;Identity MBX1 -CalendarRepairWorkCycle 06:00:00&nbsp; &ndash;CalendarRepairWorkCycleCheckpoint 00:30:00</p></p>

