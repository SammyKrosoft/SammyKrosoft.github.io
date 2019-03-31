---
layout: post
title: How-to – Load Remote Exchange PowerShell Session on Exchange 2010, 2013, 2016, Exchange Online (O365) - which ports do you need
date: 2018-04-06 22:14
author: sammykrosoft
comments: true
categories: [Exchange, Remote Powershell]
---
<span style="font-size: 22pt">1-Intro</span>

In a previous blog post, I exposed the <a target="_blank" href="https://blogs.technet.microsoft.com/samdrey/2017/12/17/how-to-load-exchange-management-shell-into-powershell-ise-2/" rel="noopener">trick to load the Exchange Management Shell on ISE</a>, the Integrated Scripting Environment of PowerShell, just like the Exchange Management Shell shortcut that is installed when you install the Exchange Management Tools. That trick needed the Exchange Management Tools to be installed locally, which is not bad as your ISE will behave exactly like your Exchange Management Shell, but with the benefits of the ISE.

Now, and as George highlighted in a comment in this previous blog post's comments section, you can also load Exchange cmdlets from a remote PowerShell session, using <span style="font-family: Courier New">New-PSSession</span> and <span style="font-family: Courier New">Import-PSSession</span>. Note that not all Exchange cmdlets and functions are available using that way (what you "lose" is a topic for a future blog post but basically you can just compare Exchange cmdlets list side by side after dumping these using "<span style="font-family: Courier New">get-excommand</span>")

As mentioned in the same previous blog post, that PowerShell session import method has its own advantages, like the ability to load Exchange management cmdlets to any workstation or server that has PowerShell and the proper ports opened, without the need to install the Exchange Management Shell. It can be very useful for your applications relying on Exchange Powershell cmdlets for example…

Below I pasted the PowerShell snippets to connect to your Exchange platform (Exchange 2010, 2013, 2016 and Exchange Online), along with the related TechNet links if you need or want more details.

&nbsp;

<span style="font-size: 22pt">2- Ports needed between management console and the remote server</span>

Ports between that station and the remote Exchange server(s) or the remote Exchange Load Balancer must be opened – <strong>by default</strong>, your Exchange servers WinRM listener will use ports <strong>5985 for HTTP</strong> and <strong>5986 for HTTPS</strong>
<ul>
 	<li>To check which ports your Exchange servers listen to, you can run the following:</li>
</ul>
<p style="background: white"><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: blue">Get-WSManInstance </span><span style="color: navy">–ResourceURI </span><span style="color: blueviolet">winrm/config/listener </span><span style="color: navy">–Enumerate
</span></span></p>
Which will show the <strong>5985</strong> port only… meaning that if only port 80 is opened between your station and your Exchange server, you won't be able to import a PS Session from it – you will need to enable listening on "traditional" http and/or https ports, respectively TCP port 80 for HTTP and TCP port 443 for HTTPS, you will need to run the below on each of your Exchange servers you will want to connect to:
<p style="background: white"><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: blue">Set-Item </span><span style="color: blueviolet">WSMan:\localhost\Service\EnableCompatibilityHttpListener </span><span style="color: navy">-Value </span><span style="color: blueviolet">true</span>
</span></p>
<p style="background: white"><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: blue">Set-Item </span><span style="color: blueviolet">WSMan:\localhost\Service\EnableCompatibilityHttpsListener </span><span style="color: navy">-Value </span><span style="color: blueviolet">true
</span></span></p>

<ul>
 	<li><span style="text-decoration: underline"><strong>Before</strong> adding the HTTP port 80 and/or HTTPs port 443 Listeners</span>, <span style="font-family: Courier New">Get-WSManInstance</span> command above gives you :</li>
</ul>
<p style="background: white"><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">cfg : http://schemas.microsoft.com/wbem/wsman/1/config/listener
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">xsi : http://www.w3.org/2001/XMLSchema-instance
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">lang : en-US
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Address : *
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">Transport : HTTP
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Port : 5985
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">Hostname :
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Enabled : true
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">URLPrefix : wsman
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">CertificateThumbprint :
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">ListeningOn : {127.0.0.1, 192.168.2.51, ::1, fe80::5efe:192.168.2.51%13, fe80::d42:e6f1:faff:ecfb%12}
</span></p>

<ul>
 	<li><span style="text-decoration: underline"><strong>And after</strong> adding these using the two <span style="font-family: Lucida Console;font-size: 9pt"><span style="color: blue">Set-Item </span><span style="color: blueviolet">WSMan</span></span> commands</span>, running the above <span style="font-family: Courier New">Get-WSManInstance</span> will output the previous one, plus the HTTP + HTTPS ones highlighted in yellow:</li>
</ul>
<p style="background: white"><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">cfg : http://schemas.microsoft.com/wbem/wsman/1/config/listener
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">xsi : http://www.w3.org/2001/XMLSchema-instance
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">lang : en-US
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Address : *
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">Transport : HTTP
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Port : 5985
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">Hostname :
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Enabled : true
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">URLPrefix : wsman
</span><span style="color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">CertificateThumbprint :
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt">ListeningOn : {127.0.0.1, 192.168.2.51, ::1, fe80::5efe:192.168.2.51%13, fe80::d42:e6f1:faff:ecfb%12}
</span></p>
<p style="background: white"><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">cfg : http://schemas.microsoft.com/wbem/wsman/1/config/listener
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">xsi : http://www.w3.org/2001/XMLSchema-instance
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Source : Compatibility
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">lang : en-US
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Address : *
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Transport : HTTP
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Port : 80
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Hostname :
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Enabled : true
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">URLPrefix : wsman
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">CertificateThumbprint :
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">ListeningOn : {127.0.0.1, 192.168.2.51, ::1, fe80::5efe:192.168.2.51%13, fe80::d42:e6f1:faff:ecfb%12}</span></p>
<p style="background: white"><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">cfg : http://schemas.microsoft.com/wbem/wsman/1/config/listener
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">xsi : http://www.w3.org/2001/XMLSchema-instance
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Source : Compatibility
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">lang : en-US
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Address : *
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Transport : HTTPS
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Port : 443
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">Hostname : E2013N1.E2013CANADA.CA
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">Enabled : true
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">URLPrefix : wsman
</span><span style="color: darkgreen;font-family: Lucida Console;font-size: 9pt;background-color: yellow">CertificateThumbprint :
</span><span style="background-color: yellow;color: darkgreen;font-family: 'Lucida Console';font-size: 9pt">ListeningOn : {127.0.0.1, 192.168.2.51, ::1, fe80::5efe:192.168.2.51%13, fe80::d42:e6f1:faff:ecfb%12}</span></p>
For more information about the above, you can check the below TechNet page:

<a target="_blank" href="https://blogs.msdn.microsoft.com/wmi/2009/07/22/new-default-ports-for-ws-management-and-powershell-remoting/" rel="noopener"><em>https://blogs.msdn.microsoft.com/wmi/2009/07/22/new-default-ports-for-ws-management-and-powershell-remoting/</em></a><em>
</em>

&nbsp;

<span style="font-size: 22pt">3- Code snippet to open a remote session to your Exchange environment
</span>

You can use the below snippets to test Exchange PowerShell "session remoting". I pasted here the code to connect to Exchange 2010, 2013, 2016 and Exchange Online, taken from the TechNet (now Microsoft Docs) pages for each technology – feel free to check the corresponding Microsoft Docs page if you want more information about these.
<p style="background: #012456"><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#For all versions, including for EoL, it's recommended you remove the PSSession as closing the PowerShell window will let the remote session opened on the server side, and the session will have to time out, and the quota for the maximum number of concurrent connections may prevent you from connecting back to the service on a timely basis.
#using <b><i>Remove-PSSession $Session</i></b> once finished will remove and close the remote session</span></p>
For all <strong>Exchange on-premise versions</strong>, the below snippets come from <a href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-server/connect-to-exchange-servers-using-remote-powershell?view=exchange-ps">this docs.microsoft.com link</a> (these are progressively replacing the TechNet pages).

For <strong>Exchange Online</strong>, the snippet come from <a href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/connect-to-exchange-online-powershell?view=exchange-ps">this docs.microsoft.com link</a>.
<h2>Exchange 2010 with current user</h2>
<p style="background: #012456"><span style="color: palegreen;font-family: 'Lucida Console';font-size: 9pt">#Exchange 2010 with current user
</span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: orangered">$ExchangeOrNLBFQDN </span><span style="color: lightgrey">= </span><span style="color: palevioletred">"E2013N1.E2013Canada.ca"
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$Session</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">New-PSSession </span><span style="color: moccasin">-ConfigurationName </span><span style="color: violet">Microsoft.Exchange </span><span style="color: moccasin">-ConnectionUri </span><span style="color: palevioletred">"http://<span style="color: orangered">$ExchangeOrNLBFQDN<span style="color: palevioletred">/PowerShell/" </span><span style="color: moccasin">-Authentication </span><span style="color: violet">Kerberos
</span></span></span></span><span style="color: lightcyan;font-family: 'Lucida Console';font-size: 9pt">Import-PSSession </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: orangered">$Session
</span></span><span style="color: palegreen;font-family: 'Lucida Console';font-size: 9pt">
#(Remove-PSSession $Session once finished)</span></p>

<h2>Exchange 2010 with other user (Run As)</h2>
<p style="background: #012456"><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#Exchange 2010 with other user (runAs type)
</span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$ExchangeOrNLBFQDN</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: palevioletred">"E2013N1.E2013Canada.ca"
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$UserCredential </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: lightgrey">= </span><span style="color: lightcyan">Get-Credential
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$Session</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">New-PSSession </span><span style="color: moccasin">-ConfigurationName </span><span style="color: violet">Microsoft.Exchange </span><span style="color: moccasin">-ConnectionUri </span><span style="color: palevioletred">"http://<span style="color: orangered">$ExchangeOrNLBFQDN<span style="color: palevioletred">/PowerShell/" </span><span style="color: moccasin">-Authentication </span><span style="color: violet">Kerberos </span><span style="color: moccasin">-Credential</span> $UserCredential
</span></span></span><span style="color: lightcyan;font-family: 'Lucida Console';font-size: 9pt">Import-PSSession</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: orangered">$Session
</span></span><span style="color: palegreen;font-family: 'Lucida Console';font-size: 9pt">
#(Remove-PSSession $Session once finished)</span></p>

<h2>Exchange 2013</h2>
<p style="background: #012456"><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#E2013
</span><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#Exchange 2013 with other user (just like using RunAs) – to connect using the current user credentials, remove or comment the $UserCredential = Get-Credential line and remove the "-Credential $UserCredential" parameter from the $Session = New-PSSession...
</span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$ExchangeOrNLBFQDN</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: palevioletred">"E2013N1.E2013Canada.ca"
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$UserCredential</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">Get-Credential
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$Session </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: lightgrey">= </span><span style="color: lightcyan">New-PSSession </span><span style="color: moccasin">-ConfigurationName </span><span style="color: violet">Microsoft.Exchange </span><span style="color: moccasin">-ConnectionUri </span><span style="color: palevioletred">"http://<span style="color: orangered">$ExchangeOrNLBFQDN<span style="color: palevioletred">/PowerShell/" </span><span style="color: moccasin">-Authentication </span><span style="color: violet">Kerberos </span><span style="color: moccasin">-Credential</span> $UserCredential
</span></span></span><span style="color: lightcyan;font-family: 'Lucida Console';font-size: 9pt">Import-PSSession </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: orangered">$Session
</span></span><span style="color: palegreen;font-family: 'Lucida Console';font-size: 9pt">
#(Remove-PSSession $Session once finished)</span></p>

<h2>Exchange 2016</h2>
<p style="background: #012456"><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#E2016
</span><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#Exchange 2016 with other user (just like using RunAs) – to connect using the current user credentials, remove or comment the $UserCredential = Get-Credential line and remove the "-Credential $UserCredential" parameter from the $Session = New-PSSession...
</span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$ExchangeOrNLBFQDN </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: lightgrey">= </span><span style="color: palevioletred">"E2013N1.E2013Canada.ca"
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$UserCredential</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">Get-Credential
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$Session</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">New-PSSession </span><span style="color: moccasin">-ConfigurationName </span><span style="color: violet">Microsoft.Exchange </span><span style="color: moccasin">-ConnectionUri </span><span style="color: palevioletred">"http://<span style="color: orangered">$ExchangeOrNLBFQDN<span style="color: palevioletred">/PowerShell/" </span><span style="color: moccasin">-Authentication </span><span style="color: violet">Kerberos </span><span style="color: moccasin">-Credential</span> $UserCredential
</span></span></span><span style="color: lightcyan;font-family: 'Lucida Console';font-size: 9pt">Import-PSSession</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: orangered">$Session
</span></span><span style="color: palegreen;font-family: 'Lucida Console';font-size: 9pt">
#(Remove-PSSession $Session once finished)</span></p>

<h2>Exchange Online (Office 365)</h2>
<p style="background: #012456"><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#EO365
</span><span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">#Exchange Online
</span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$UserCredential</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">Get-Credential
</span></span><span style="color: orangered;font-family: 'Lucida Console';font-size: 9pt">$Session</span><span style="font-family: Lucida Console;font-size: 9pt"> <span style="color: lightgrey">= </span><span style="color: lightcyan">New-PSSession </span><span style="color: moccasin">-ConfigurationName </span><span style="color: violet">Microsoft.Exchange </span><span style="color: moccasin">-ConnectionUri </span><span style="color: violet">https://outlook.office365.com/powershell-liveid/ </span><span style="color: moccasin">-Authentication </span><span style="color: violet">Basic </span><span style="color: moccasin">-Credential </span><span style="color: orangered">$UserCredential </span><span style="color: moccasin">-AllowRedirection
</span></span><span style="color: lightcyan;font-family: 'Lucida Console';font-size: 9pt">Import-PSSession </span><span style="font-family: Lucida Console;font-size: 9pt"><span style="color: orangered">$Session
<span style="color: palegreen;font-family: Lucida Console;font-size: 9pt">
#(Remove-PSSession $Session once finished)</span></span></span></p>
<strong>References and Great Articles:</strong>
<ul>
 	<li><a href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-server/connect-to-exchange-servers-using-remote-powershell?view=exchange-ps">Connect to Exchange servers using remote PowerShell</a></li>
 	<li><a href="https://docs.microsoft.com/en-us/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/connect-to-exchange-online-powershell?view=exchange-ps">Connect to Exchange Online PowerShell</a></li>
 	<li><a target="_blank" href="https://docs.microsoft.com/en-us/office365/enterprise/powershell/connect-to-office-365-powershell" rel="noopener">Connect to Office 365 PowerShell</a></li>
</ul>
