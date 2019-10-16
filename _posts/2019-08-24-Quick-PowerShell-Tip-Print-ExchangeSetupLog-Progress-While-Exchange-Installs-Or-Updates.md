# a note from my lab...

## Looping Get-Content -Tail <number> Of A File
When installing a product like Exchange, we would like to know what is it doing beyond the standard setup output showing the high level steps like :

```
C:\E2016CU23>setup.exe /Mode:Install /Roles:Mailbox /TargetDir:"C:\Program Files\Microsoft\Exchange Server" /IAcceptExchangeServerLicenseTerms

Microsoft Exchange Server 2016 Cumulative Update 13 Unattended Setup

Copying Files...
File copy complete. Setup will now collect additional information needed for installation.

Languages
Mailbox role: Transport service
Mailbox role: Client Access service
Mailbox role: Unified Messaging service
Mailbox role: Mailbox service
Mailbox role: Front End Transport service
Mailbox role: Client Access Front End service

Performing Microsoft Exchange Server Prerequisite Check

    Configuring Prerequisites                                                                         COMPLETED
    Prerequisite Analysis                                                                             COMPLETED

Configuring Microsoft Exchange Server

    Preparing Setup                                                                                   COMPLETED
    Stopping Services                                                                                 COMPLETED
    Copying Exchange Files                                                                            COMPLETED
    Language Files                                                                                    COMPLETED
    Restoring Services                                                                                COMPLETED

```

Here I've been waiting for a while, but I knew that the Exchange setup is logging its granular actions in its c:\ExchangeSetupLogs\ExchangeSetup.Log file.

I then decided to loop the reading of the few last lines of the file. We can choose do read the "Tail" of a text file using ```Get-Content``` with the ```-Tail <Last_Number_of_Lines>``` parameter.

Here's the bit of code that enables this:
```powershell
PS> while ($true){
    cls
    (Get-Date).addhours(4);Get-Content -tail 3 C:\ExchangeSetupLogs
\ExchangeSetup.log;sleep 1
}
```

Note the following:
```powershell
(Get-Date).AddHours(4)
```

That's because the time stamps in the ExchangeSetup.Log file is in GMT time, and I wanted to dump the current GMT timeby adding the 4 hours of time shift between Canada East and Greenwitch.

At the time of the Exchange Setup, the Get Content of the 3 last lines gave me this output:

```
Sunday, August 25, 2019 2:59:01 AM
[08/25/2019 02:46:34.0217] [1] Updating performance counter strings for WorkloadPerformanceCounters.ini
[08/25/2019 02:46:34.0299] [1] Updating performance counter strings for WsPerformanceCounters.ini
[08/25/2019 02:46:34.0964] [1] Finished updating performance counter strings
```

I knew at that time that at 2:59:01 AM GMT time, the Exchange setup was stuck since 02:46:34, that's sroughly 13 minutes stuck after updating the performance counters strings...

That doesn't tell me much, but at least I knew it didn't stall on an error for now...

## If Exchange Setup is taking too long on Windows 2016, exclude the following directories from Windows Defender

Exchange binaries directory
Exchange Setup Log directory
Exchange Program Files directory

![Win Defender Exclusions](..\assets\media\Win_Defender_Exclusions_exchange_Setup.png)

## If you have to re-run setup because of a failed setup, check the following registry keys
If the setup fails - in my case it Dr Watsoned once on the Mailbox role install the first time it failed, and then I got successful install, but then the Get-ClientAccessService was not returning the server name => I checked the registry keys for the presence of *ACTION* and *WATERMARK* under each component.

My case was the same as Paul Cunningham's article for the second time <- this was told to me by the Setup, saying the FrontEndTransportRole was not correctly installed. For the first time, when the setup crashed on the Mailbox role installation, removing ACTION and WATERMARK was also the key to a successful second run of Setup.exe for the Mailbox Role.

Here's the link to Paul's article:
https://practical365.com/exchange-server/exchange-server-error-an-incomplete-installation-was-detected/

And a screenshot of a sample component where removing *ACTION* and *WATERMARK* enabled a successful Setup.exe second run:

![Registry Action and Watermark Keys](..\assets\media\exchange-registry-watermark-02.png)