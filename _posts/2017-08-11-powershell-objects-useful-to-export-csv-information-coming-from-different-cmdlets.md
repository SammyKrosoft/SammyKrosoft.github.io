---
layout: post
title: Powershell Objects–How to export-csv information coming from different cmdlets and with potentially multi-valued attributes
date: 2017-08-11 13:59
author: sammykrosoft
comments: true
categories: [Uncategorized]
---
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Hi all,</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Since I keep forgetting where <a>this great post</a> is located, I’m just putting a bookmark here. This article is discussing about the different ways to populate a PowerShell Custom Object.</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">What can we use PowerShell Custom Object for ? Actually on Exchange for example (including Exchange Online), we sometimes need to have information on users and mailboxes that come from different cmdlets (like some info that you can get using “<span style="font-family: Courier New">Get-Recipient</span>” but not with “<span style="font-family: Courier New">Get-Mailbox</span>”, some other info you can get using “<span style="font-family: Courier New">Get-User</span>” but not with “<span style="font-family: Courier New">Get-Recipient</span>” nor “<span style="font-family: Courier New">Get-Mailbox</span>”, etc…), and you want to concatenate all these in a single object, making it easier to manipulate and export in a CSV eventually.</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Below I’ll also walk through a concrete example about what I’m referring to above, but initially this blog post was just meant to put a bookmark here so that we can refer to it later when we need – that way I don’t have to ask myself on which browser or which computer did I put that information – Below is a link to <strong>Don Jones</strong>’ article about the many ways to create PowerShell custom objects, thanks Don !</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"><span style="font-size: 16pt">Don Jones' PowerShell Object creation methods description</span></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><span lang="EN-US">Title:</span></b><span lang="EN-US"> Windows PowerShell: The Many Ways to a Custom Object</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><span lang="EN-US">Link</span></b><span lang="EN-US">: <a title="https://technet.microsoft.com/en-us/library/hh750381.aspx">https://technet.microsoft.com/en-us/library/hh750381.aspx</a></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><span lang="EN-US">Author</span></b><span lang="EN-US">: <b><u>Don Jones</u></b></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"><span style="font-size: 16pt">Application of Don's article in an Exchange example:</span></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">I will just show 2 of the many methods that Don Jones is exposing in his article, because these 2 are my favourites but I encourage you to have a look on his article too…</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Let’s just take a simple example where I want to have in a single PowerShell object that has information from 2 different cmdlets (Get-Mailbox and Get-Recipient), that I want to export in a CSV later along with other information (to add up a collection of Get-Mailbox / Get-Recipient, just do a loop looking like a “</span><span lang="EN-US"><span style="font-family: Courier New">ForEach ($user in $usercollection) {$mailbox = Get-mailbox $user ; $recipient = Get-Recipient; &lt;Put your object code sample like below here&gt;; $ObjectCollection += $object})</span></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Then to export all the collection just pipe $ObjectCollection to an Export-CSV cmdlet</span><span lang="EN-US"><span style="font-family: Courier New"> $ObjectCollection | Export-CSV C:\temp\YourCollection.csv</span></span><span lang="EN-US"></span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">For simplicity and Quick Ref, I’m just showing it for a single mailbox/recipient …</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><u><span lang="EN-US">Method #1</span></u></b><b><span lang="EN-US">: Add-Member with NoteProperty and Value for each property and associated value</span></b><span lang="EN-US"></span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Mailbox = Get-Mailbox User_Alias</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Recipient = Get-Recipient User_Alias</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object = New-Object –TypeName PSObject</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name Name –Value $mailbox.Name</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name EmailAddresses –Value ($mailbox.emailaddresses –join ";")</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name OrganizationalUnit –Value $Recipient.organizationalunit</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt"> </span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="FR"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Export-CSV $env:userprofile\desktop\yourobject.csv</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><u><span lang="FR">Quick explanation:</span></u></b></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; first we load the object and its mailbox properties on a variable</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Mailbox = Get-Mailbox User_Alias</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; second we load the object ant its recipient properties on another variable</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Recipient = Get-Recipient User_Alias</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; then we instantiate (=we "create") a new Powershell Object (aka PSObject) that we put in the $Object variable</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object = New-Object –TypeName PSObject</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; Finally we add the properties we want to this objects : the properties coming from the Get-Mailbox will be added from the $mailbox variable ($mailbox.property), and the properites coming from the Get-Recipient will be added from the $recipient variable ($recipient.property)</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name Name –Value $mailbox.Name</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; note the second one, where the -Value is like ($mailbox.emailaddresses -join ";") =&gt; that is in case you have a collection of non-string values, the "-join" function will concatenate these, and the separator specified immediately after will define the separator, and since it's in between double quotes, the whole concatenated item will be converted to Powershell String </span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name EmailAddresses –Value ($mailbox.emailaddresses –join ";")</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; And to add other properties, from another cmdlet result, here from the Get-Recipient one, as easy as above, just add a property specifying $recipients.property on the next "-value"</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Add-Member –MemberType NoteProperty –Name OrganizationalUnit –Value $Recipient.organizationalunit</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">-&gt; Finally, if you want to export your object to a CSV file (that will be a single-line because we just have one object in that example, see the text above using ForEAch to populate a variable with many objects): just pipe the variable to an Export-CSV, et voilà!</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Export-CSV $env:userprofile\desktop\yourobject.csv</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">(note: here the $Env:userprofile\desktop points to the current users's desktop – NOTE that if you did a "RunAs" of your powershell window from where you execute your lines, that $env:userprofile\desktop will refer to the profile of the user you are running your powershell window as.)</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><u><span lang="EN-US">Method #2</span></u></b><b><span lang="EN-US">: using a “hash table” (barbarian name for a sort of array that associate properties with values)</span></b><span lang="EN-US"></span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Mailbox = Get-Mailbox User_Alias</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$Recipient = Get-Recipient User_Alias</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt"> </span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$properties = @{'Name'=$mailbox.Name;</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span><span style="font-size: 10pt">                </span></span><span style="font-size: 10pt">'EmailAddresses'=($mailbox.emailaddresses –join ";");</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span><span style="font-size: 10pt">                </span></span><span style="font-size: 10pt">'OrganizationalUnit'=$Recipient.organizationalunit}</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt"> </span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object = New-Object –TypeName PSObject –Prop $properties</span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt"> </span></span></span></p>
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Export-CSV $env:userprofile\desktop\yourobject.csv</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN">Knowing Method #1 above, that is relatively simple to understand :</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN">We define each "<span style="font-family: Courier New">Name</span>" and "<span style="font-family: Courier New">Value</span>" of the "<span style="font-family: Courier New">NoteProperty</span>" that we added using "<span style="font-family: Courier New">Add-Member -Membertype</span>" all at once in a "hash table" (@{'Name1' = 'Value1'; 'Name2' = 'Value2', etc….}) – as a one-liner or a multiple-liner for better readability and flexibility, we put that hash table in a variable ($properties in the above example), and then we instantiate the object adding directly the variable containing the hash table :</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span lang="EN-US"><span style="font-family: Consolas"><span style="font-size: 10pt">$object = New-Object –TypeName PSObject –Prop $properties</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">And then if you need to export, same as the first method:</span></p>

<div style="background: whitesmoke;line-height: normal;border: #cccccc 1pt solid;padding: 7pt">
<p class="MsoNormal" style="background: whitesmoke;margin: 0cm 0cm 7.5pt;line-height: normal;padding: 0cm"><span><span style="font-family: Consolas"><span style="font-size: 10pt">$object | Export-CSV $env:userprofile\desktop\yourobject.csv</span></span></span></p>

</div>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN">That's pretty straightforward, simpler to write as you don't have to repeat the "</span><span lang="EN"><span style="font-family: Courier New">Add-Member -MemberType NoteProperty -Name "Name1" -Value $variable.property</span></span><span lang="EN">" each time – cool eh !</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><b><u><span lang="EN-US">NOTE</span></u></b><span lang="EN-US">: I just showed 2 of the methods to populate PSObjects because these are my favorites for now, but <a>Don Jones shows more variants</a>, and I encourage you to have a look on his post as well.</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US"> </span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Have a great one PowerShell mates !</span></p>
<p class="MsoNormal" style="background: white;margin: 0cm 0cm 7.5pt;line-height: normal"><span lang="EN-US">Sam</span></p>
<p class="MsoNormal" style="margin: 0cm 0cm 8pt;line-height: 12pt"><span lang="EN-US"><span style="font-family: Calibri"><span style="color: #000000;font-size: 11pt"> </span></span></span></p>
