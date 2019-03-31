---
layout: post
title: Note - Comportement des groupes AD lors de déplacements d'utilisateurs entre deux domaines
date: 2009-03-20 06:52
author: sammykrosoft
comments: true
categories: [Active Directory, groupes]
---
<p><strong><u>IContexte :</u></strong><p>For&ecirc;t A<br>- Domaine_A<br>- Domaine_B</p><p><br>User_A : Domaine_A</p><p><br>User_A est membre de :<br>Groupe Universel 1<br>Groupe Global 1<br>Groupe Local 1</p><p><br><strong><u>IIAction :</u></strong></p><p><br>ADMT : Move Domaine_AUser_A --&gt; Domaine_B<br>User_A se logge maintenant en Domaine_BUser_A</p><p><br><u><strong>IIICons&eacute;quences sur les</strong></u> <u><strong>groupes:</strong></u></p><p>. D'apr&egrave;s la <strong>console graphique ADUC</strong> (Active Directory USers and Computers), User_A <strong>n'est plus</strong> dans les groupes "<strong>Groupe Global 1</strong>" et "<strong>Groupe Local 1</strong>", mais <strong>est toujours</strong> dans le "<strong>Groupe Universel 1</strong>" (par d&eacute;finition d'un groupe universel)</p><p><br><u><strong>Par contre, ce qu'il faut savoir:</strong></u> </p><p><br>-<strong> le SID de User_A est toujours dans le Groupe Local 1<br></strong>On ne le voit plus dans l'interface graphique, mais on peut le voir directement dans l'AD avec ADSIEDIT<br>==&gt; Les ressources pour lesquelles l'utilisateur User_A avait acc&egrave;s via le groupe Groupe Local 1 sont toujours accessibles par User_A</p><p><br><strong>- Toute trace de User_A a disparu du Groupe Global 1</strong><br>==&gt; Les ressources pour lesquelles l'utilisateur User_A avait acc&egrave;s via le groupe Groupe Global 1 ne sont plus accessibles par User_A</p></p>

