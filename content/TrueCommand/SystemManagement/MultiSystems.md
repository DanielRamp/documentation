---
title: "Multiple Systems"
weight: 30
---

{{< toc >}}

TrueCommand has several multisystem management capabilities with more in development for future releases.

Cluster capability has been added with TrueCommand 2.0, as well as applying TrueNAS configurations to multiuple systems at once. 

## Config Management

TrueCommand can manage TrueNAS [Config files]({{< relref "truenasconfigmanage.md" >}}).
TrueCommand can also restore a single config file to multiple systems.

To apply a config to multiple systems, first create a config backup from the TrueNAS system you with the settings you wish to apply to other TrueNAS units.

Click on the system name for a TrueNAS server to open the Single System view.

![DashboardSingleSystemView](/images/TrueCommand/2.0/DashboardSingleSystemView.png "Dashboard Single System View")

Click **Config Backups** to open the config backup window.

The **Configuration Backup Window** shows a list of backups with the time and date of their creation.

Set the checkbox for the config to restore and click the <mat-icon role="img" class="mat-icon notranslate material-icons mat-icon-no-color" aria-hidden="true">restore</mat-icon> Restore Icon.

![RestoreDatabase](/images/TrueCommand/2.0/RestoreDatabase.png "RestoreDatabase")

Click **ADD SYSTEM** to select a system that will be restored by the config file.

![RestoreDatabaseAddSystem](/images/TrueCommand/2.0/RestoreDatabaseAddSystem.png "RestoreDatabaseAddSystem")

Additional servers can be added as needed.

![RestoreDatabaseSelectSystem](/images/TrueCommand/2.0/RestoreDatabaseSelectSystem.png "RestoreDatabaseSelectSystem")

Once the systems have been chosen, click **CONFIRM** to upload the config backup to the selected TrueNAS systems.

![RestoreDatabaseSystemSelected](/images/TrueCommand/2.0/RestoreDatabaseSystemSelected.png "RestoreDatabaseSystemSelected")

## System Inventory

To access the System Inventory page, click the *Gear* icon and Select **System Inventory**.

![SystemInventoryMenu](/images/TrueCommand/2.0/SystemInventoryMenu.png "Access the System Inventory Page")

{{< hint info >}}
To download a comma deliminated .CVS file for the current *Inventory* page, click **CVS** in the upper-right area of the screen.
{{< /hint >}}

There are three categories of inventory information:

**System** provides information on the *Manufacturer*, the *Serial* numbers of the controllers, the system's *Support Tier*, the Support Contract's *Expiration* date, the *Hostname* of the active controller, the *CPU*, the number of *CPU Cores*, the amount of *Physical Memory*, what *OS* the system is running, and the *Uptime*.

![SystemInventorySystem](/images/TrueCommand/2.0/SystemInventorySystem.png "System Information")

**Network** provides information about the *Interface* names, *Type*, *Link State* and *MAC* Address.

![SystemInventoryNetwork](/images/TrueCommand/2.0/SystemInventoryNetwork.png "System Network Information")

**Storage** provides information about the *Drives* such as *Name*, *Type* of drive, *Size*, *Model*, *Serial* Number, and location in the *Enclosure*.

![SystemInventoryStorage](/images/TrueCommand/2.0/SystemInventoryStorage.png "System Storage Information")

## iSCSI Management

When creating an iSCSI Volume with TrueCommand, the volume can be configured on multiple systems at the same time.
Refer to the [iSCSI section]({{< relref "iscsimanagement.md" >}}) for more information.

## Cluster Managemnet

By definition, clusters span across multiple systems.
TrueCommand instances that are connected to three or more TrueNAS SCALE systems can create clustered volumes.
Refer to the [Clustering section]({{< relref "TrueCommand/Clustering/_index.md" >}}) for more information.
