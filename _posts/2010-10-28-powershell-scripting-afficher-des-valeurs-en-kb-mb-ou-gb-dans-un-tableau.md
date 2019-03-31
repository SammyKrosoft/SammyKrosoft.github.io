---
layout: post
title: PowerShell Scripting – Afficher des valeurs en KB, MB ou GB dans un tableau
date: 2010-10-28 21:35
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p>&#160;</p>  <p>Après la CmdLet “<strong>Format-Table</strong>”, lister les propriétés à afficher, puis quand arrive la propriété de type numérique à convertir, utiliser le caractère <strong>@</strong> suivi des accollades<strong> {}</strong> :</p>  <p><strong>@{name=”Label(MB)”;expression={$_.propriété/1MB}}</strong></p>  <p>Emplacer le<strong> /1MB</strong> par <strong>/1GB</strong> pour convertir le nombre en GB, ou par<strong> /1KB</strong> pour convertir en KB.</p>  <p>N’oubliez pas de remplacer le <strong>Label(MB)</strong> en conséquence pour plus de lisibilité ou pour éviter les appreciations trompeuses …</p>  <p>&#160;</p>  <p><u>APPLICATION :</u></p>  <p>Taille de mémoire d’un processus – ici Notepad.exe – sans conversion (en Octets)</p>  <p>&#160;</p>   <pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 600px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">PS C:\Users\samdrey&gt; <span style="color: #0000ff">Get</span>-Process notepad | Ft ProcessNAme,VirtualMemorysize -autosize 
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">ProcessName	VirtualMemorySize
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">-----------	-----------------
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">notepad		84176896</pre></pre>

<p>&#160;</p>

<p>Taille de mémoire de ce même processus avec conversion (en MégaOctets)</p>

<pre style="border-bottom: #cecece 1px solid; border-left: #cecece 1px solid; padding-bottom: 5px; background-color: #fbfbfb; min-height: 40px; padding-left: 5px; width: 600px; padding-right: 5px; overflow: auto; border-top: #cecece 1px solid; border-right: #cecece 1px solid; padding-top: 5px"><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">PS C:\Users\samdrey&gt; <span style="color: #0000ff">Get</span>-Process notepad | Ft ProcessNAme,@{name=&quot;<span style="color: #8b0000">VMSize(MB)</span>&quot;;Expression={$_.Virtualmorysize/1MB}} -autosize
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">ProcessName	VMSize(MB)
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">-----------	----------
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px">notepad		80,27734375
</pre><pre style="background-color: #fbfbfb; margin: 0em; width: 100%; font-family: consolas,&#39;Courier New&#39;,courier,monospace; font-size: 12px"></pre></pre>

