```powershell
Move-ActiveMailboxDatabase Database_Name\Server_Name -SkipClientExperienceChecks -MountDialOverRide GoodAvailability 
```
And checked database status

Get-MailboxDatabase -Status | Ft Name,MountedOnServer, Mounted

Where we should see the MountedOnServer one of the B-EX-MAIL servers, and Mounted status to “$True”.
