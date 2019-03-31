---
layout: post
title: PowerShell scripts to Migrate Exchange Mailbox and Distribution Group Send As, Full Access and Send On Behalf Permissions into a CSV file–Part I - Export Script
date: 2018-04-29 22:42
author: sammykrosoft
comments: true
categories: [Exchange, powershell]
---
Hi !

This post is to introduce a script to export Mailbox Send As, Full Access and Send On Behalf permissions, and also Distribution Group Send As and Send On Behalf permissions into a CSV file, to help you achieve the following objectives:
<ul>
 	<li>You wish to audit the permissions globally set to your mailboxes across the whole organization –&gt; export the permissions, and analyze these within Excel with the power of “Format As Table”, filtering, pivot tables, … and why not, make executive reports of these permissions with our powerful <a href="https://powerbi.microsoft.com/en-us/">PowerBI</a> ! I'll also blog later on this one for a couple of examples...</li>
 	<li>You wish to migrate your mailboxes and your Distribution Groups to another forest, and you wish to keep the Send As, Full Access and Send On Behalf permissions. That’s in case you’re migrating your mailboxes cross forest not using the New-MoveRequest cmdlet cross forest, because these will keep those permissions (see <a target="_blank" href="https://blogs.technet.microsoft.com/exchange/2010/08/10/exchange-2010-cross-forest-mailbox-moves/" rel="noopener">this older but still accurate post from the Exchange Product Team for more information</a>)</li>
</ul>
This is the Part I which introduces and gives you the script to export the Mailbox / Distribution Groups permissions, named Export-MailboxFASAPermissions.ps1 ("FASA" stands for Full Access and Send As - although the scripts include the export/import of the Send On Behalf permissions as well). I'll refer to that script as to EXPORT script in this articles quick series (Export/Import - a bit equivalent to Migrate).

The Part II will come later and introduce the Import-MailboxFASAPermissions.ps1. I’ll refer to this Import-MailboxFASAPermissions.ps1 as to “IMPORT script”.
<blockquote><strong>NOTE 1</strong>: in the case of permissions "migration", which is nothing else than an Export of the permissions from an environment, to later import these to a target environment. The key used to match users between the source forest and the target forest will be the <strong>user SMTP address</strong> for the mailboxes to re-populate with permissions – because <strong>an SMTP address uniquely identifies a mailbox.</strong>

<strong>NOTE 2</strong>: When exported with Get-ADPermission or Get-MailboxPermission, the Send As and Full Access permissions are referenced with DOMAIN\Alias of the users. When exported with Get-Mailbox, GrandSendOnBehalfTo permissions are referenced with the DOMAIN/OU1/SUB-OU/Alias type notation.</blockquote>
The EXPORT script exports the<strong> Send As</strong> and <strong>Full Access</strong> permissions in the form <strong>DOMAIN\Alias</strong>. If you import in a different forest, the IMPORT script will let you specify a domain to populate these permissions in the form of <strong>TARGET_DOMAIN\Alias</strong> - easily achieved by splitting the SOURCE_DOMAIN\alias string using the "\" as a separator, and concatenating the "alias" with the "TARGET_DOMAIN" using PowerShell's "-Join" of "$MyArray.Join("\")" method (by default the IMPORT Script will import these permissions as they were on the source environment, using SOURCE_DOMAIN\Alias).
<blockquote><strong>NOTE 3:</strong> GrantSendOnBehalfTo can only be given to Mailbox-Enabled users, Mail-Enabled Users, or Mail-Enabled Security Groups. These are exported in the notation indicated in NOTE 2, but the script converts these to their SMTP Address – remember the GrantSendOnBehalfTo can only contain Mailbox-Enabled users, Mail-enabled users or mail-enabled security groups =&gt; that way, the Get-Recipient cmdlet resolves the DOMAIN/OU1/SUB-OU/…/Alias into an SMTP address =&gt; since an SMTP address uniquely identify a recipient, converting the DOMAIN/OU/…/Alias notation to an SMTP address in the CSV file simplifies the Import as well, as we will just use Set-Mailbox &lt;MailboxSMTPAddress&gt; –GrantSendOnBehalfTo “User1SMTPAddress”, “User2SMTPAddress”, “…” which will work provided the UserxSMTPAddress exist in the target environment.</blockquote>
&nbsp;

As an important usage note, you'll notice on the Get-Help dump below that the script has 3 syntaxes:
<ul>
 	<li>One that exports Shared and Resource mailboxes only</li>
</ul>
<pre>.\Export-MailboxFASAPermissions.ps1 -SharedMailboxes -ResourceMailboxes</pre>
<ul>
 	<li>One that exports Distribution group with the option to Include or not the Dynamic Distribution groups</li>
</ul>
<pre>.\Export-MailboxFASAPermissions.ps1 -DistributionGroupsOnly -IncludeDynamic $true/$false</pre>
<ul>
 	<li>One that checks the script version</li>
</ul>
<pre>.\Export-MailboxFASAPermissions.ps1 -CheckVersion</pre>
There is one other use which is just launching the script without options:
<pre>.\Export-MailboxFASAPermissions.ps1</pre>
This will export all the Mailboxes permissions.

Here’s the help of the EXPORT script – the download link is at the very end of the help dump:
<pre>PS&gt;get-help .\Export-MailboxFASAPermissions.ps1 -full

NAME
     .\Export-MailboxFASAPermissions.ps1
     
SYNOPSIS
     Export Exchange Mailbox Send As, Full Access, and Send On Behalf permissions
     in a CSV file in order to later import them in another environment using the 
     output CSV file.
     
     
SYNTAX
     .\Export-MailboxFASAPermissions.ps1 [[-SharedMailboxes]] [[-ResourceMailboxes]] [[-OutputFile] &lt;String&gt;] [&lt;CommonParameters&gt;]
     
     .\Export-MailboxFASAPermissions.ps1 [[-DistributionGroupsOnly]] [[-IncludeDynamic] &lt;Boolean&gt;] [&lt;CommonParameters&gt;]
     
     .\Export-MailboxFASAPermissions.ps1 [[-CheckVersion]] [&lt;CommonParameters&gt;]
     
     
DESCRIPTION
     This script requires the Exchange tools to run.
     
     It exports the following Exchange Mailbox permissions in a CSV file 
     - Send As
     - Full Access
     - Send On Behalf To
     in order to be able to import them later in another environment using 
     the output CSV file.
     
     The Output CSV file will contain the following information for each mailbox permissions
     information exported:
     
     Display Name, Primary SMTP Address, Full Access permissions, Send As permissions, Send On Behalf permissions
     
     The permissions can have one or more entries, which will be separated by semicolons (";")
     
     To import back the permissions if needed , you can use the associated Import-MailboxFASAPermissions.ps1 
     script.
     
     Since the Send As and Full Access permissions can be granted to non-mailbox or
     non-mail enabled users, these are stored in the CSV in the form of DOMAIN\Alias.
     
     On the other hand, the Send On Behalf permission can be granted only to mailbox-enabled users,
     mail-enabled users and/or mail-enabled security groups only. For some reason, it is stored in 
     the form of DOMAIN\OU1\Sub-OU1\...\Name - then, the script is designed to convert these - actually
     the script resolve these using Get-Mailbox -Identity DOMAIN\OU\...\Name to get and store the
     PrimarySMTPAddress of these users so that we have two advantages:
         &gt; Not only we are sure that each SMTP address represents a unique user
         &gt; Also it will be way easier for the IMPORT script to import these permissions back, wherever OU the
         target user will be located !
     
     This is because the IMPORT script uses Set-Mailbox with the -SendOnBehalfTo, where we can
     specify an SMTP address, which will be converted to the corresponding DOMAIN\OU\Name of the 
     corresponding user in the target environment.
     
     In other words, the SMTP address will be the KEY to match the SendOnBehalfTo permission to the
     right users and mailboxes on the target environments.
    

PARAMETERS
     -SharedMailboxes [&lt;SwitchParameter&gt;]
         This indicates the script to export the SharedMailboxes only
         
         When combined with the -ResourceMailboxes, the script will export
         the Shared Mailboxes, and the Room and Equipment Mailboxes as well !
         
         To export ALL mailboxes, just don't specify neither the SharedMailboxes
         nor the ResourceMailboxes parameter.
         
         Required?                    false
         Position?                    1
         Default value                False
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     -ResourceMailboxes [&lt;SwitchParameter&gt;]
         This indicates the script to export the ResourceMailboxes only which
         consist of the Room and the Equipment Mailboxes.
         
         When combined with the -SharedMailboxes, the script will export the
         Shared Mailboxes, the Room and the Equipment mailboxes as well !
         
         To export ALL mailboxes, just don't specify neither the SharedMailboxes
         nor the ResourceMailboxes parameter.
         
         Required?                    false
         Position?                    2
         Default value                False
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     -DistributionGroupsOnly [&lt;SwitchParameter&gt;]
         
         Required?                    false
         Position?                    4
         Default value                False
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     -IncludeDynamic &lt;Boolean&gt;
         
         Required?                    false
         Position?                    5
         Default value                True
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     -OutputFile &lt;String&gt;
         Sets the file to which we want to store the results.
         By default, the script will generate a CSV report with the name of the script, 
         with the date and time appended to it.
         
         Required?                    false
         Position?                    6
         Default value                
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     -CheckVersion [&lt;SwitchParameter&gt;]
         This parameter just dumps the script version.
         
         Required?                    false
         Position?                    7
         Default value                False
         Accept pipeline input?       false
         Accept wildcard characters?  false
         
     &lt;CommonParameters&gt;
         This cmdlet supports the common parameters: Verbose, Debug,
         ErrorAction, ErrorVariable, WarningAction, WarningVariable,
         OutBuffer, PipelineVariable, and OutVariable. For more information, see 
         about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216). 
     
INPUTS
     The script will scan all the mailboxes, but database by database to avoid to use
     all the RAM of the machine from which it's executed.
     
     
OUTPUTS
     A CSV file with either a name that you specify with the OutputFile parameter, or if not,
     the name of the script, containing the users Display Names, primary SMTP addresses,
     and the list of Send-As, Full Access and SendOnBehalfTo for each of these mailboxes.
     
     If the Send-As, Full Access and SendOnBehalfTo are multi-values, they are stored in the columns
     as semi-colon separated values, like Value1;value2;value3;...
     
     =&gt; when processing each permissions set, just use something like $ImportedCSV.SendAsPermissions -split ";" 
     or $ImportedCSV.SendAsPermissions.Split(";") ...
     
     
NOTES
     
     
         This script can be use alone to export a permissions map, but the output is designed so that it
         can be used with the Import-MailboxFASAPermissions.ps1 script to migrate permissions to another
         environment such as a LAB or a brand new one with the same users (Inter-Forest migration for example
         or move from an On-Prem to an outsourced environment such as Office 365)
         
         Some simple facts about the permissions exported on this script:
         
         "Sens As" permissions
             . Stored in the form of "DOMAIN\Alias"
             . Is set with Add-ADPermission
             . <a href="https://docs.microsoft.com/en-us/powershell/module/exchange/active-directory/Add-ADPermission?view=exchange-ps">https://docs.microsoft.com/en-us/powershell/module/exchange/active-directory/Add-ADPermission?view=exchange-ps</a>
         
         "Full Access" Permissions
             . Stored in the form of "DOMAIN\Alias" as well
             . Is set with Add-MailboxPermission
             . <a href="https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/Add-MailboxPermission?view=exchange-ps">https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/Add-MailboxPermission?view=exchange-ps</a>
         
         "Send On Behalf Of" permissions
             . Stored in the form of "Domain.com/OU_Name/Sub_OU/Name"
             . Is set with Set-Mailbox
             . <a href="https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/Set-Mailbox?view=exchange-ps">https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/Set-Mailbox?view=exchange-ps</a>
             . -GrantSendOnBehalfTo parameter accepts one or more values from the below :
                     Display name
                     Alias
                     Distinguished name (DN)
                     Canonical DN
                     &lt;domain name&gt;\&lt;account name&gt;
                     Email address
                     GUID
                     LegacyExchangeDN
                     SamAccountName
                     User ID or user principal name (UPN)
     
     -------------------------- EXAMPLE 1 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASAPermissions.ps1
     
     Will run the script and export the mailbox Display Names, primary SMTP Addresses, and all the
         Send As, Full Access and Send On Behalf To permissions on a CSV file.
     
     
     
     
     -------------------------- EXAMPLE 2 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASAPermissions.ps1 -OutputFile C:\temp\EnvironmentPermissions.csv
     
     Will run the script and export permissions for all mailboxes, in the file specified on the 
         OutputFile parameter : C:\temp\EnvironmentPermissions.csv
     
     
     
     
     -------------------------- EXAMPLE 3 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASA.ps1 -SharedMailboxes
     
     Will run the script and export the Shared Mailboxes permissions as well as the Room and
         Equipment Mailboxes permissions, and store the result on the default CSV file named after
         the script, appended with the date and time of the execution, on the script directory
     
     
     
     
     -------------------------- EXAMPLE 4 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASA.ps1 -ResourceMailboxes c:\temp\ResourceMailboxPermissions.csv
     
     Will run the script and export only the Room and Equipment Mailboxes permissions, and store
         the results in a CSV file c:\temp\ResourceMailboxPermissions.csv
     
     
     
     
     -------------------------- EXAMPLE 5 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASA.ps1 -DistributionGroupsOnly
     
     Will run the script and export only the Distribugion Group permissions (Send As, GrantSendOnBehalfTo) in 
         the default Output file format (Script_Name_Date_time.csv). This includes the Dynamic Distribution 
         Groups.
     
     
     
     
     -------------------------- EXAMPLE 6 --------------------------
     
     PS C:\&gt;.\Export-MailboxFASA.ps1 -DistributionGroupsOnly -IncludeDynamic $false
     
     Will run the script to export permissions of Distribution Groups, excluding the Dynamic Distribugion
         Groups.
     
     
     
     
     
RELATED LINKS
     <a href="https://technet.microsoft.com/en-ca/library/jj919240(v=exchg.150).aspx">https://technet.microsoft.com/en-ca/library/jj919240(v=exchg.150).aspx</a>
     <a href="https://docs.microsoft.com/en-us/powershell/module/exchange/active-directory/add-adpermission?view=exchange-ps">https://docs.microsoft.com/en-us/powershell/module/exchange/active-directory/add-adpermission?view=exchange-ps</a>
     <a href="https://technet.microsoft.com/en-us/library/jj919240(v=exchg.150).aspx">https://technet.microsoft.com/en-us/library/jj919240(v=exchg.150).aspx</a>
     https://github.com/SammyKrosoft</pre>
&nbsp;

<a href="https://gallery.technet.microsoft.com/Export-Mailbox-and-0654fa53"><span style="text-decoration: underline"><strong>CLICK HERE TO GO TO THE TECHNET GALLERY TO DOWNLOAD THIS SCRIPT...</strong></span></a>
