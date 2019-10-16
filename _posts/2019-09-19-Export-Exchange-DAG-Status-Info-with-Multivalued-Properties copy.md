---
layout: post
author: sammykrosoft
comments: true
categories: [PowerShell,DAG,Multivalue]
permalink: /How-To/Export-Exchange-Information-With-MultiValue-parameters.html
---

# Another note from my lab...

## Exporting PowerShellObject multivalued properties

Use the following synthax in the list of properties

```powershell
@{Name="Header name";Expression={$_.MultiValuedProperty -join ";"}}
```

For Example with e-mail addresses from an Exchange mailbox user:

```powershell
Get-Mailbox User001 | Ft Name,DisplayName,@{Name="E-mail addresses";Expression={$_.proxyaddresses -join ";"}}
```

Or to dump the same information in a CSV file, the same hashatable expression @{Name="Name";Expression={$_.MultivaluedProperty -join ";"}} can be used in a Select or Select-Object statement:

```powershell
Get-Mailbox User001 | Ft Name,DisplayName,@{Name="E-mail addresses";Expression={$_.proxyaddresses -join ";"}} | Export-CSV -NoTypeInformation c:\temp\MyMailboxAddresses.csv
```

## Example with exporting DAG information with many parameters

That includes some single-valued properties as well as multi-valued properties:

```powershell
Get-DatabaseAvailabilityGroup -Status *DAG0* | select Name, @{Name="Servers"; Expression={$_.Servers -join ";"}}, WitnessServer, WitnessDirectory, AlternateWitnessServer, AlternateWitnessDirectory, NetworkCompression, NetworkEncryption, ManualDagNetworkConfiguration, DatacenterActivationMode, @{Name="StoppedMailboxServers";Expression={$_.StoppedMailboxServers -join ";"}}, @{Name="StartedMailboxServers";Expression={$_.StartedMailboxServers -join ";"}}, @{Name="DatabaseAvailabilityGroupIpv4Addresses";Expression={$_.DatabaseAvailabilityGroupIpv4Addresses -join ";"}}, @{Name="DatabaseAvailabilityGroupIpAddresses";Expression={$_.DatabaseAvailabilityGroupIpAddresses -join ";"}}, AllowCrossSiteRpcClientAccess, ActivityState, FileSystem, @{NAme="OperationalServers";Expression={$_.DatabaseAvailabilityGroupIpAddresses -join ";"}}, PrimaryActiveManager, ServersInMaintenance, ThirdPartyReplication, ReplicationPort, @{Name="NetworkNames";Expression={$_.NetworkNames -join ";"}}, WitnessShareInUse, DatabaseAvailabilityGroupConfiguration, AutoDagSchemaVersion, AutoDagDatabaseCopiesPerDatabase, AutoDagDatabaseCopiesPerVolume, AutoDagTotalNumberOfDatabases, AutoDagTotalNumberOfServers, AutoDagDatabasesRootFolderPath, AutoDagVolumesRootFolderPath, AutoDagAllServersInstalled, AutoDagAutoReseedEnabled, AutoDagDiskReclaimerEnabled, AutoDagBitlockerEnabled, AutoDagFIPSCompliant, AutoDagAutoRedistributeEnabled, AutoDagSIPEnabled, ReplayLagManagerEnabled, MailboxLoadBalanceSellableStorage, MailboxLoadBalanceRelativeLoadCapacity, MailboxLoadBalanceComputeCapacity, MailboxLoadBalanceOverloadedThreshold, MailboxLoadBalanceUnderloadedThreshold, MailboxLoadBalanceEnabled, SiloName, DistributedStoreConfig, RequestedDistributedStoreConfig, DxStoreWitnessServers, DistributedStoreMembershipConfig, DistributedStoreMembershipConfigOverride, DxStoreSpareServers, PreferenceMoveFrequency, MetaCacheDatabaseVolumesPerServer, AdminDisplayName, ExchangeVersion, DistinguishedName, Identity, Guid, ObjectCategory, ObjectClass, WhenChanged, WhenCreated, WhenChangedUTC, WhenCreatedUTC, OrganizationId, Id, OriginatingServer, IsValid, ObjectState | export-csv -NoTypeInformation C:\temp\E2016DAGinfoStatus.csv

```
