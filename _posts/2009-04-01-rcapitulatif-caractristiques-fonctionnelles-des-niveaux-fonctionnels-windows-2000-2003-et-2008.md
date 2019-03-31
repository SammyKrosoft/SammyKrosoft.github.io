---
layout: post
title: Récapitulatif - Caractéristiques fonctionnelles des niveaux fonctionnels Windows 2000, 2003 et 2008
date: 2009-04-01 04:53
author: sammykrosoft
comments: true
categories: [Active Directory, AD]
---
<p>A l&rsquo;occasion de l&rsquo;installation d&rsquo;une for&ecirc;t Windows 2008, la console graphique d&rsquo;installation r&eacute;sume les caract&eacute;ristiques de chaque niveau fonctionnels de for&ecirc;t Windows au moment du choix du niveau fonctionnel du domaine. <p>Je les reprends ici pour r&eacute;f&eacute;rences futures, cela &eacute;vitera de se replonger dans les docs &agrave; tout bout de champ quand on recherche ce genre d&rsquo;infos ( c&rsquo;est un peu le but de mon Blog :-) ) :</p><p>A noter que les descriptions des niveaux fonctionnels Windows sont reprises avec tous les d&eacute;tails dans l&rsquo;aide de Windows Server 2008 (lien <em>en savoir plus sur les <u>niveaux fonctionnels de for&ecirc;t et de domaine</u></em> dans la fen&ecirc;tre d&rsquo;installation, ou plus directement le fichier d&rsquo;aide HTML compil&eacute; <strong>ad_ds.chm</strong> dans le r&eacute;pertoire <strong>%systemroot%helpmui40C</strong> &ndash;&gt; 040C pour le Fran&ccedil;ais si vous avez install&eacute; Windows 2008 en Fran&ccedil;ais ou le Language Pack Fran&ccedil;ais).</p><meta content="fr" http-equiv="Content-Language"><style type="text/css">
.style1 {
	border-style: solid;
	border-width: 1px;
	text-align: center;
}
.style2 {
	border: 1px solid #735C35;
	text-align: center;
	background-color: #C0C0C0;
}
.style3 {
	white-space: normal;
	border-style: solid;
	border-width: 1px;
	background-color: #99FF66;
}
.style4 {
	border: 1px solid #735C35;
	text-align: center;
	background-color: #99FF66;
}
.style5 {
	border-collapse: collapse;
	border: 0px solid #735C35;
}
.style6 {
	white-space: normal;
	text-align: left;
	border-style: solid;
	border-width: 1px;
	background-color: #99FF66;
}
</style><link rel="stylesheet" type="text/css" href="StylesheetBlog.css" mce_href="StylesheetBlog.css"><table style="WIDTH: 100%" class="style5" cellspacing="0">
<tbody>
<tr>
<td style="WIDTH: 589px; HEIGHT: 50px" class="style2"></td>
<td style="WIDTH: 715px; HEIGHT: 50px" class="style2"><strong>Niveau fonctionnel</strong></td>
<td style="HEIGHT: 50px" class="style2"><strong>Windows 2000 Natif (*)</strong></td>
<td style="HEIGHT: 50px" class="style2"><strong>Windows 2003 Natif</strong></td>
<td style="HEIGHT: 50px" class="style2"><strong>Windows 2008 Natif</strong></td></tr>
<tr>
<td style="WIDTH: 589px; HEIGHT: 50px" class="style4"><strong>Fonctionnalit&eacute;s</strong></td>
<td style="WIDTH: 715px; HEIGHT: 50px" class="style4"></td>
<td class="style1"></td>
<td class="style1"></td>
<td class="style1"></td></tr>
<tr>
<td class="style3" colspan="2"><strong>Groupes universels</strong></td>
<td class="style1">x</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2"><strong>Imbrication de groupes</strong></td>
<td class="style1">x</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2"><strong>Conversion de types de groupes</strong></td>
<td class="style1">x</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2"><strong>Historique SID</strong></td>
<td class="style1">x</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style6" colspan="2"><strong>D&eacute;l&eacute;gation contrainte</strong>, qu&rsquo;une application peut utiliser pour b&eacute;n&eacute;ficier de la d&eacute;l&eacute;gation s&eacute;curis&eacute;e des informations d&rsquo;identification de l&rsquo;utilisateur au moyen du protocole d&rsquo;authentification Kerberos.</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Mises &agrave; jour de <strong>lastLogonTimestamp</strong> : l&rsquo;attribut <strong>lastLogonTimestamp</strong> est mis &agrave; jour avec la derni&egrave;re heure de connexion de l&rsquo;utilisateur ou de l&rsquo;ordinateur, et r&eacute;pliqu&eacute; sur le domaine.</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Capacit&eacute; &agrave; d&eacute;finir l&rsquo;attribut <strong>userPassword</strong> en tant que mot de passe effectif sur <strong>inetOrgPerson</strong> et les <strong>objets utilisateur</strong>.</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Capacit&eacute; &agrave; rediriger les conteneurs <strong>Utilisateurs et ordinateurs</strong> afin de d&eacute;finir un nouvel emplacement connu pour les comptes d&rsquo;utilisateurs et d&rsquo;ordinateurs.</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Prise en charge de la <strong>r&eacute;plication du syst&egrave;me de fichiers DFS (Distributed File System) </strong>pour<strong> SYSVOL</strong>, qui offre une r&eacute;plication plus robuste et granulaire du contenu de SYSVOL.</td>
<td class="style1">&nbsp;</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Prise en charge des <strong>services de chiffrement avanc&eacute;s AES </strong>(Advanced Encryption Services) 128 et 256 pour le protocole <strong>Kerberos</strong>.</td>
<td class="style1">&nbsp;</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2">Derni&egrave;res <strong>informations de connexion interactives</strong>, qui affichent l&rsquo;<strong>heure de la derni&egrave;re connexion interactive</strong> r&eacute;ussie d&rsquo;un utilisateur, le nombre de <strong>tentatives de connexion ayant &eacute;chou&eacute;</strong> depuis la derni&egrave;re connexion et l&rsquo;heure du dernier &eacute;chec de connexion.</td>
<td class="style1">&nbsp;</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td></tr>
<tr>
<td class="style3" colspan="2"><strong>Strat&eacute;gies de mot de passe affin&eacute;es</strong>, qui permettent aux <strong>utilisateurs</strong> et aux <strong>groupes de s&eacute;curit&eacute; global</strong> d&rsquo;un domaine de sp&eacute;cifier des strat&eacute;gies de mot de passe et de verrouillage de compte.</td>
<td class="style1">&nbsp;</td>
<td class="style1">&nbsp;</td>
<td class="style1">x</td></tr></tbody></table><p><span style="LINE-HEIGHT: 115%; FONT-FAMILY: 'Calibri','sans-serif'; FONT-SIZE: 11pt; mso-ascii-theme-font: minor-latin; mso-fareast-font-family: calibri; mso-fareast-theme-font: minor-latin; mso-hansi-theme-font: minor-latin; mso-bidi-font-family: 'Times New Roman'; mso-bidi-theme-font: minor-bidi; mso-ansi-language: fr; mso-fareast-language: en-us; mso-bidi-language: ar-sa">* <em>Si vous disposez de contr&ocirc;leurs de domaine ex&eacute;cutant des versions ult&eacute;rieures de Windows Server, certaines fonctionnalit&eacute;s avanc&eacute;es ne seront pas disponibles sur ces contr&ocirc;leurs de domaine tant que le domaine restera au niveau fonctionnel Windows 2000 natif.</em></span></p></p>

