---
layout: post
title: PowerShell Scripting - Déclarer un tableau
date: 2010-10-28 21:27
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&nbsp;<p>D&eacute;clarer comme :</p><p>$a = @()</p><p># Initialise un tableau &hellip; utile pour des inputs successifs &agrave; stocker dedans par exemple</p><p>ou bien</p><p>[int32[]]$a</p><p># Force l&rsquo;utilisation de valeurs de type Int32 pour le tableau</p><p><em>Source : aide ci-desssous et Internet &hellip;</em></p><p>If the statement gets only a single process, the $LocalProcesses variable   <br>would not be an array. To ensure the command creates an array, use the    <br><strong>array subexpression operator</strong>, @, as shown in the following example: </p><p>&nbsp;&nbsp;&nbsp; $LocalProcesses = @(get-process co*) </p><p>Even if the command returns a single process, the $LocalProcesses variable   <br>is an array. Even if it has only one member, you can treat it like any    <br>other array. For example, you can add other objects to it. For more    <br>information, see about_Operators.</p><p><a title="http://technet.microsoft.com/en-us/library/dd315313.aspx" href="http://technet.microsoft.com/en-us/library/dd315313.aspx">http://technet.microsoft.com/en-us/library/dd315313.aspx</a></p></p>

