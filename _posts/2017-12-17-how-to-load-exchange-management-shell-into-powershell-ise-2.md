---
layout: post
title: How To–Load Exchange Management Shell into PowerShell ISE
date: 2017-12-17 11:32
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<span style="font-size: 16pt">1. Introduction
</span>

This post provides you with the method to load the Exchange Management Shell into ISE.

The ingredients we're using for this trick are:
<ul>
 	<li>the ISE console</li>
 	<li>the ISE PowerShell profile</li>
 	<li>the PowerShell script that loads into the Exchange Management Shell default shortcut</li>
</ul>
&nbsp;

<span style="font-size: 16pt">2. Prerequisites
</span>

The prerequisites are that your <strong>Exchange Management Tools</strong>, for Exchange 2010, Exchange 2013 or Exchange 2016, must be installed on that machine (which can be a desktop, a server dedicated for Exchange management or an Exchange server itself) – See <a href="https://technet.microsoft.com/en-us/library/ff728623(v=exchg.150).aspx">this link</a> to check which OS are supported to Install the Exchange Management Tools.

And see <a href="https://docs.microsoft.com/en-us/exchange/plan-and-deploy/post-installation-tasks/install-management-tools">this link</a> about how to install Exchange Management Tools (from the Exchange setup files, use something like <span>Setup.exe /Role:ManagementTools /IAcceptExchangeServerLicenseTerms</span>)
<blockquote><em>Note that if you don't have / don't want to install the Exchange Management tools on your management server/desktop, you can also choose to import a remote Powershell session from an Exchange 2010/2013/2016 environment to bring in the Exchange Powershell commands, but there are some limitations also - there are less Exchange cmdlets (784 Exchange cmdlets brought by remoting PowerShell into an Exchange session, versus 963 Exchange cmdlets brought by the Exchange Management Shell - that's for Exchange 2013 just to give you an idea), and both methods have their pros and cons. This article is about opening the ISE PowerShell console and load the Exchange Cmdlets <strong>exactly like the Exchange Management Shell console</strong> shortcut (2010, 2013 and/or 2016), and <strong>not</strong> using Remote PSSessions - that is another topic which Technet cover quite clearly for Exchange <a href="https://technet.microsoft.com/en-ca/library/dd335083(v=exchg.141).aspx">2010</a>, <a href="https://technet.microsoft.com/en-ca/library/dd335083(v=exchg.150).aspx">2013 </a>and <a href="https://technet.microsoft.com/en-ca/library/dd335083(v=exchg.160).aspx">2016</a> and <a target="_blank" href="https://blogs.technet.microsoft.com/samdrey/2018/04/06/how-to-load-remote-powershell-session-on-exchange-2010-2013-2016-exchange-online-o365-2/" rel="noopener">I summarize these TechNet articles in a Blog Post</a> as well, and you have as well my friend Rhoderick who describes this in <a href="https://blogs.technet.microsoft.com/rmilne/2015/02/02/using-exchange-powershell-remoting-with-integrated-scripting-environmentise/">one of his awesome posts specifically with ISE</a>, many resources on this topic for all tastes.</em></blockquote>
&nbsp;

<span style="font-size: 16pt">3. Principle
</span>

Basically what we do here is that we just copy the Exchange Management Shell "traditional" shortcut definition on the ISE profile file. In the below script, we also test that Exchange cmdlets are not already present before loading the Exchange Management cmdlets – for more information about ISE profile, see <a href="https://docs.microsoft.com/en-us/powershell/scripting/core-powershell/ise/how-to-use-profiles-in-windows-powershell-ise?view=powershell-5.1">this link</a>.

What I find convenient with ISE is that you can use intellisense and color highlight when editing your Exchange PowerShell Management scripts, and also copy/paste color-coded Exchange instructions for your blogging or documentation purposes, which I did for my last posts.

Also, with Powershell ISE, you have an action pane that drills down all the cmdlets that are loaded with your current session – loading the Exchange Management Shell within ISE will also provide you all the available Exchange (2010, 2013, 2016) cmdlets available for Exchange management. Note that this "action pane" in ISE also loads all O365 and/or Azure cmdlets if you import a Powershell Session where you are connected to your O365/Azure tenant.

<img alt="" src="https://msdnshared.blob.core.windows.net/media/2017/12/121717_1632_HowToLoadEx1.png" />

<span style="font-size: 12pt"><strong>#1- First start by forcing the creation of your PowerShell ISE profile file if it doesn't exist
</strong></span>

Type or paste the below directly on the command part of your ISE console and press "Enter" (or paste it on the script pane and then press "F5", which I did on the below screenshot)
<p style="background: #012456"><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: lightcyan">if </span><span style="color: whitesmoke">(<span style="color: lightgrey">!<span style="color: whitesmoke">(<span style="color: lightcyan">Test-Path </span><span style="color: moccasin">-Path </span><span style="color: orangered">$PROFILE</span> ))
</span></span></span></span><span style="color: whitesmoke;font-family: 'Lucida Console';font-size: 9pt">{</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightcyan">New-Item </span><span style="color: moccasin">-Type </span><span style="color: violet">File </span><span style="color: moccasin">-Path </span><span style="color: orangered">$PROFILE </span><span style="color: moccasin">-Force </span><span style="color: whitesmoke">}
</span></span></p>
<img alt="" src="https://msdnshared.blob.core.windows.net/media/2017/12/121717_1632_HowToLoadEx2.png" />

See <a href="https://docs.microsoft.com/en-us/powershell/scripting/core-powershell/ise/how-to-use-profiles-in-windows-powershell-ise?view=powershell-5.1">here</a> for more details about ISE PowerShell profile…

<span style="font-size: 14pt"><strong>#2- Second, within your ISE console, type the following
</strong></span>

<span style="font-family: Lucida Console"><strong>psEdit $profile
</strong></span>

<img alt="" src="https://msdnshared.blob.core.windows.net/media/2017/12/121717_1632_HowToLoadEx3.png" />

<strong>Note 1</strong>: this will open your <span style="font-family: Courier New">$profile</span> (for ISE $profile is <strong>Microsoft.PowerShellISE_Profile.ps1</strong> as you can notice in the script pane's title that just opened – again for more information about ISE profile, see <a href="https://docs.microsoft.com/en-us/powershell/scripting/core-powershell/ise/how-to-use-profiles-in-windows-powershell-ise?view=powershell-5.1">this link</a>) in the ISE script pane.

<strong>Note 2</strong>: this psEdit command is available in ISE only, not in the text-based PowerShell console.

<span style="font-size: 14pt"><strong>#3- in the script pane, just copy/paste the below blue script<span style="font-family: Courier New">
</span></strong></span>

The below script goes a little further than just loading the Exchange Management Shell into ISE:
<ul>
 	<li>The <span style="font-family: Courier New">Test-Command ($Command)</span> function enables us the ability to test if a cmdlet is already loaded in the current session – we just call <span style="font-family: Courier New">Test-Command</span> with a well known command from the Exchange module, "<span style="font-family: Courier New">Get-Mailbox</span>" for example, and if we don't <em>CATCH</em> any error (<strong>Test-Command("cmdlet")</strong> returns <strong>$true</strong>), that means that Exchange tools are already loaded and we just print <span style="font-family: Courier New">"Exchange cmdlets already present"</span>, ELSE we load the Exchange Management Tools by just calling the regular Exchange Management Shell loading scripts.</li>
 	<li>For the fun, I just added a .<a href="https://technet.microsoft.com/en-us/library/2009.03.heyscriptingguy.aspx">NET standard "StopWatch" object</a> just to measure the loading time – starting a new one while defining the <span style="font-family: Courier New">$StopWatch</span> variable <span style="font-family: Courier New">$StopWatch = [System.Diagnostics.StopWatch]::StartNew() </span>– and stopping it with <span style="font-family: Courier New">$StopWatch.Stop()</span>, then storing the Elapsed time in Milliseconds in a variable with <span style="font-family: Courier New">$Time=$StopWatch.Elapsed.TotalMilliseconds</span>, and printing it to the console using <span style="font-family: Courier New">Write-Host</span>…</li>
</ul>
<div class="WordSection1">
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: orangered">$<span class="SpellE">StopWatch</span></span><span style="font-size: 9.0pt;font-family: 'Lucida Console'"> <span style="color: lightgrey">=</span> <span style="color: lightgrey">[</span><span class="SpellE"><span class="GramE"><span style="color: darkseagreen">System.Diagnostics.StopWatch</span></span></span><span style="color: lightgrey">]::</span><span class="SpellE"><span style="color: whitesmoke">StartNew</span></span><span style="color: whitesmoke">()</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke"> </span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: lightcyan">Function</span><span style="font-size: 9.0pt;font-family: 'Lucida Console'"> <span style="color: violet">Test-Command</span> <span style="color: whitesmoke">(</span><span style="color: orangered">$Command</span><span style="color: whitesmoke">)</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke">{</span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: lightcyan">Try</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: whitesmoke">{</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>        </span><span style="color: lightcyan">Get-command</span> <span style="color: orangered">$command </span><span style="color: moccasin">-<span class="SpellE">ErrorAction</span></span> <span style="color: violet">Stop</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>        </span><span style="color: lightcyan">Return</span> <span style="color: orangered">$True</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: whitesmoke">}</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: lightcyan">Catch</span> <span style="color: lightgrey">[</span><span class="SpellE"><span style="color: darkseagreen">System.SystemException</span></span><span style="color: lightgrey">]</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: whitesmoke">{</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>        </span><span style="color: lightcyan">Return</span> <span style="color: orangered">$False</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: whitesmoke">}</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke">}</span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke"> </span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: lightcyan">IF</span><span style="font-size: 9.0pt;font-family: 'Lucida Console'"> <span style="color: whitesmoke">(</span><span style="color: lightcyan">Test-Command </span><span style="color: palevioletred">"Get-Mailbox"</span><span style="color: whitesmoke">)</span> <span style="color: whitesmoke">{</span><span style="color: lightcyan">Write-Host</span> <span style="color: palevioletred">"Exchange cmdlets already present"</span><span style="color: whitesmoke">}</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: lightcyan">Else</span><span style="font-size: 9.0pt;font-family: 'Lucida Console'"> <span style="color: whitesmoke">{</span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: orangered">$<span class="SpellE">CallEMS</span></span> <span style="color: lightgrey">=</span> <span style="color: palevioletred">". '</span><span style="color: orangered">$<span class="SpellE"><span class="GramE">env:ExchangeInstallPath</span></span></span><span class="GramE"><span style="color: palevioletred">\bin\RemoteExchange.ps1</span></span><span style="color: palevioletred">'; Connect-<span class="SpellE">ExchangeServer</span> -auto -<span class="SpellE">ClientApplication:ManagementShell</span> "</span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console'"><span>    </span><span style="color: lightcyan">Invoke-Expression</span> <span style="color: orangered">$<span class="SpellE">CallEMS</span></span><span style="color: whitesmoke"></span></span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke">
$stopwatch.Stop()
$msg = "`n`nThe script took $([math]::round($($StopWatch.Elapsed.TotalSeconds),2)) seconds to execute..."
Write-Host $msg
$msg = $null
$StopWatch = $null</span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke">}</span></p>
<p class="MsoNormal" style="margin-bottom: .0001pt;line-height: normal;background: #012456"><span style="font-size: 9.0pt;font-family: 'Lucida Console';color: whitesmoke"><span> </span></span></p>
<p class="MsoNormal"><span lang="FR-CA"> </span></p>

</div>
The ISE window will look like the below:

<img alt="" src="https://msdnshared.blob.core.windows.net/media/2017/12/121717_1632_HowToLoadEx4.png" />
<p style="background: white"><span style="color: #333333"><span style="font-size: 13pt"><strong>#4- Just save it, close your ISE, et voilà !</strong></span><span style="font-size: 10pt">
</span></span></p>
<p style="background: white"><span style="color: #333333">Now every time you open it, ISE will load the Exchange Management Tools like below, and even indicate the time it took to load the Exchange Management tools… Maybe milliseconds is a bit overkill, then just use the "TotalSeconds" property instead of "TotalMilliseconds" property in <span style="font-family: Lucida Console">$Time=$StopWatch.Elapsed.TotalMilliseconds </span>line within your ISE's <span style="font-family: Lucida Console">$Profile</span> …<span style="font-size: 10pt">
</span></span></p>
<img alt="" src="https://msdnshared.blob.core.windows.net/media/2017/12/121717_1632_HowToLoadEx5.png" />
