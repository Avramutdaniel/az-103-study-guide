# AZ-103 STUDY GUIDE – PART 1 – MANAGE AZURE SUBSCRIPTIONS AND RESOURCES#

This AZ-103 Study Guide links to most relevant documentation on Microsoft Docs. 
It covers all objectives / skills measured in AZ-103 exam and is meant to be a 80% complete study guide. 20% should come from experience, watching video’s and tutorials. Exam AZ-103: Microsoft Azure Administrator grants the title Microsoft Certified: Azure Administrator Associate

![part1](https://user-images.githubusercontent.com/38914947/61790898-220de300-ae21-11e9-9c28-247e76696918.png)

## MANAGE AZURE SUBSCRIPTIONS AND RESOURCES (15-20%)
### MANAGE AZURE SUBSCRIPTIONS
### ASSIGN ADMINISTRATOR PERMISSIONS
https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal

https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles

To assign a role to a user, sign in to the Azure Portal, navigate to the user, select Directory Role and select Add Role. Only Global Administrators (sometimes referred as Company Administrators) or Privileged Role Administrators can delegate administrator roles.

Assign with Powershell:

https://docs.microsoft.com/en-us/office365/enterprise/powershell/assign-roles-to-user-accounts-with-office-365-powershell

For  example:

```
Get-AzureADDirectoryRole    #to list the roles that you can assign with PowerShell
$userName="sign-in name of the account"
$roleName="role name"
Add-AzureADDirectoryRoleMember -ObjectId (Get-AzureADDirectoryRole | Where {$_.DisplayName -eq $roleName}).ObjectID -RefObjectId (Get-AzureADUser | Where {$_.UserPrincipalName -eq $userName}).ObjectID
```

To increase security, you should minimize the number of people with administrative rights. Azure AD Privileged Identity Management (PIM) essentially helps you manage the who, what, when, where, and why for resources that you care about. You need Azure AD Premium P2 or Enterprise Mobility + Security (EMS) E5 for this functionality

https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/pim-configure

 

### CONFIGURE COST CENTER QUOTAS AND TAGGING

https://docs.microsoft.com/en-us/azure/billing/billing-getting-started

Microsoft Azure limits are sometimes called quotas.
Use tags to group billing data for supported services. For example, if you run several VMs for different teams, then you can use tags to categorize costs by cost center (HR, marketing, finance) or environment (production, pre-production, test).

To apply tags on resources:

https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags

You can add tags using

* Azure Policies
* Powershell:
```
	Set-AzResourceGroup -Name examplegroup -Tag @{ Dept=”IT”; Environment=”Test” }
```
* Azure CLI:
```
	az group update -n examplegroup –set tags.Environment=Test tags.Dept=IT
```
* Templates
* Azure Portal (GUI)

To review usage and costs, Microsoft recommends Cloudyn. Cloudyn shows you usage and costs so that you can track trends, detect inefficiencies, and create alerts

https://docs.microsoft.com/en-us/azure/cost-management/tutorial-review-usage

 

### CONFIGURE AZURE SUBSCRIPTION POLICIES AT AZURE SUBSCRIPTION LEVEL

https://docs.microsoft.com/en-us/azure/governance/policy/overview

Azure Policy begins with creating a policy definition. Every policy definition has conditions under which it’s enforced.  For example: only allow D1 VM’s to be created, or validate SQL Servers to have version 12 or later installed. To implement these policy definitions, you’ll need to assign them to a scope using Policy Assignments. A scope can be a Management group, Subscription or Resource group. You can assign policies through the Azure portal, PowerShell, or Azure CLI. Policy evaluation happens with several different actions, such as policy assignment or policy updates

Optional:

Policy parameters help simplify your policy management by reducing the number of policy definitions. Think of parameters like the fields on a form – name, address, city, state. These parameters always stay the same, however their values change based on the individual filling out the form. Parameters work the same way when building policies. By including parameters in a policy definition, you can reuse that policy for different scenarios by using different values.

An initiative definition is a collection of policy definitions. They simplify by grouping a set of policies as one single item.

 

## ANALYZE RESOURCE UTILIZATION AND CONSUMPTION
### CONFIGURE DIAGNOSTIC SETTINGS ON RESOURCES

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/diagnostic-logs-overview

Azure Monitor diagnostic logs (formerly named Log Analytics) are logs emitted by an Azure service that provide rich, frequent data about the operation of that service. These logs can come from tenant-level resources (like Azure Active Directory) or resources within the subscription (like a Network Security Group or a Storage Account)

To configure diagnostic settings , you need to set

Where diagnostic logs and metrics are sent (Storage Account, Event Hubs, and/or Azure Monitor).
Which log categories are sent and whether metric data is also sent.
How long each log category should be retained in a storage account
To enable storage of diagnostic logs in a storage account with
Powershell:
```
	Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```
Azure CLI:
```
az monitor diagnostic-settings create ……
```
 

### CREATE BASELINE FOR RESOURCES
https://docs.microsoft.com/en-us/azure/automation/automation-dsc-overview

This seems to be related to Configuration Management, and can be done by using DSC.

 

### CREATE AND REST ALERTS
https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-activity-log

Alerts only support two metrics signals, or one log search signal or one activity signal. To be alerted when event 4654 or event 3245 occurs, you have to create two alerts, because in each alert you can only add one condition.

 

### ANALYZE ALERTS ACROSS SUBSCRIPTION
https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview

Use the All Alerts page to view a list of alerts created within a selected time window.  Click through to Alert Detail Page to get more information, and to change its alert state. Use Smart Groups to filter noise in alerts.

Smart groups are automatically created by using machine learning algorithms to combine related alerts that represent a single issue. When an alert is created, the algorithm adds it to a new smart group or an existing smart group based on information such as historical patterns, similar properties, and similar structure. For example, if % CPU on several virtual machines in a subscription simultaneously spikes leading to many individual alerts, and if such alerts have occurred together anytime in the past, these alerts will likely be grouped into a single Smart Group, suggesting a potential common root cause.

 

### ANALYZE METRICS ACROSS SUBSCRIPTION
https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-collection

Metrics are collected every minute by default. They are stored for 93 days. Copy them to logs for long term trending.
 

### CREATE ACTION GROUPS

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/action-groups

An action may contain multiple actions (like Send SMS and execute Runbook).

 

### MONITOR FOR UNUSED RESOURCES

https://docs.microsoft.com/en-us/azure/cost-management/tutorial-review-usage

Use Cloudyn to improve efficiency. I.e. use the Optimizer to identify idle VM’s and unattached disks.

 

### MONITOR SPEND

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/usage-estimated-costs

https://azure.microsoft.com/en-us/pricing/details/cost-management/

 

### REPORT ON SPEND

https://azure.microsoft.com/en-us/pricing/details/cost-management/

Use Budgets to keep track on your expenses. Use Alerting to get notified on costs and usage budgets. Monthly budgets are evaluated every 4 hours. When an alert condition is met (i.e. costs have reached 80% of monthly budget), you can alert someone by email, but you can also trigger an action group. (i.e. at 80%, shutdown all non-critical VM’s)

https://docs.microsoft.com/en-us/azure/billing/billing-cost-management-budget-scenario

Cloudyn is also an option to report and alert on spending.

 

### UTILIZE LOG SEARCH QUERY FUNCTIONS

https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/get-started-queries

 

### VIEW ALERTS IN LOG ANALYTICS

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alert-management-solution

Logs Analytics repository can collect alerts from multiple sources:

* Log Analytics
* Nagios and Zabbix
* System Center Operations Manager

Then, add the Alert Management Solution to your Logs Analytics workspace to start analyzing.

 

## MANAGE RESOURCE GROUPS
### USE AZURE POLICIES FOR RESOURCE GROUPS

https://docs.microsoft.com/en-us/azure/governance/policy/tutorials/create-and-manage

### CONFIGURE RESOURCE LOCKS

https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-lock-resources

Locks can have 2 values: CanNotDelete and ReadOnly. All resources within a parent scope inherit the setting. The most restrictive setting take precedence.

Locks only apply to operations at management plane level. For example, a ReadOnly lock on a SQL Database prevents you from deleting or modifying the database, but it doesn’t prevent you from creating, updating, or deleting data in the database.

 

### CONFIGURE RESOURCE POLICIES

Not sure how this differs from 1.3.1, beside the scope (Resource vs Resource group)

 

### IDENTIFY AUDITING REQUIREMENTS (NEW SINCE AZ-100)

https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit

Not sure, since this objective is placed in “Manage Resource Groups”. “Identify” feels like a soft-skill.

 

### IMPLEMENT AND SET TAGGING ON RESOURCE GROUPS

https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags



 

### MOVE RESOURCES ACROSS RESOURCE GROUPS

https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-move-resources

The majority of services can be moved. However, there are still a lost of services that can’t.
Both the source group and the target group are locked during the move operation. Write and delete operations are blocked on the resource groups until the move completes. However, there is no downtime on the particular service.

 

### REMOVE RESOURCE GROUPS
 
https://docs.microsoft.com/en-us/azure/azure-resource-manager/manage-resources-portal#delete-resource-group-or-resources

 

## MANAGED ROLE BASED ACCESS CONTROL (RBAC) (NEW SINCE AZ-100)
### CREATE A CUSTOM ROLE

https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles

 

### CONFIGURE ACCESS TO AZURE RESOURCES BY ASSIGNING ROLES

https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-portal

In Powershell:
```
	New-AzRoleAssignment -SignInName <email, userprincipalname> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```
 

### CONFIGURE MANAGEMENT ACCESS TO AZURE, TROUBLESHOOT RBAC, IMPLEMENT RBAC POLICIES, ASSIGN RBAC ROLES
Generally, this means you need to know every basic action for managing RBAC. A good start would be

https://docs.microsoft.com/en-us/azure/role-based-access-control/overview

Configure Management Access to Azure:

https://docs.microsoft.com/en-us/azure/role-based-access-control/elevate-access-global-admin

Troubleshoot RBAC:

https://docs.microsoft.com/en-us/azure/role-based-access-control/troubleshooting

Implement RBAC policies (needs you to understand basics and best practices):

https://docs.microsoft.com/en-us/azure/role-based-access-control/overview

Assign RBAC Roles:

https://docs.microsoft.com/en-us/azure/role-based-access-control/quickstart-assign-role-user-portal

# AZ-103 STUDY GUIDE – PART 2 – IMPLEMENT AND MANAGE STORAGE #

![part2](https://user-images.githubusercontent.com/38914947/61790941-37830d00-ae21-11e9-8a87-b748fdf0abb0.png)

## IMPLEMENT AND MANAGE STORAGE (15-20%)
### CREATE AND CONFIGURE STORAGE ACCOUNTS

Before ticking off the objectives, I would highly recommend you to first start reading the introduction:

https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction

 

### CONFIGURE NETWORK ACCESS TO THE STORAGE ACCOUNT

https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security

Azure Storage firewall provides security for a storage account. Network rules apply to practically all network traffic, except: virtual machine disk traffic, and traffic within the same VNet

 

### CREATE AND CONFIGURE STORAGE ACCOUNT

https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal

To create a new Storage Account with Powershell:
```
New-AzStorageAccount -ResourceGroupName RG_itexperience `
  -Name "saitexperience" `
  -Location westus `
  -SkuName Standard_LRS `
  -Kind StorageV2
``` 

https://docs.microsoft.com/en-us/azure/storage/common/storage-account-upgrade

You can upgrade from General-purpose v1 to General purpose v2. This is permanent and cannot be undone.

In Powershell:

```
Set-AzStorageAccount -ResourceGroupName &lt;resource-group&gt; -AccountName &lt;storage-account&gt; -UpgradeToStorageV2
```

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers

Working with Blob Storage and General Purpose v2 accounts, Access tiers enable you to choose Hot, Cool, or Archive tier. Hot and Cool can be set at account level. Archive can be set only at object level.

 

### GENERATE SHARED ACCESS SIGNATURE

https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1

 

### INSTALL AND USE AZURE STORAGE EXPLORER

https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer

 

### MANAGE ACCESS KEYS

https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1

 

### MONITOR ACTIVITY LOG BY USING LOG ANALYTICS

https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-activity-logs



 

### IMPLEMENT AZURE STORAGE REPLICATION

https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy

Reminder: you can easily change from LRS to GRS to RA-GRS from within the Azure Portal. However, to change from or to ZRS, you need to copy manually, or request a live migration at Microsoft.

In addition, quickly review this site for better understanding:

https://insidemstech.com/2018/03/23/azure-storage-replication/

 

## IMPORT AND EXPORT DATA TO AZURE
### CREATE EXPORT FROM AZURE JOB

https://docs.microsoft.com/en-us/azure/storage/common/storage-import-export-data-from-blobs

 

### CREATE IMPORT INTO AZURE JOB

https://docs.microsoft.com/en-us/azure/storage/common/storage-import-export-data-to-blobs

https://docs.microsoft.com/en-us/azure/storage/common/storage-import-export-data-to-files

 

### USE AZURE DATA BOX

https://docs.microsoft.com/en-us/azure/databox/

 

### CONFIGURE AND USE AZURE BLOB STORAGE

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-storage-explorer

https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-powershell

List blobs in a container

```
Get-AzStorageBlob -Container $ContainerName -Context $ctx | select Name
``` 

### CONFIGURE AZURE CONTENT DELIVERY NETWORK (CDN) ENDPOINTS

https://docs.microsoft.com/en-us/azure/cdn/cdn-optimization-overview

 

## CONFIGURE AZURE FILES
### CREATE AZURE FILE SHARE

https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share

```
$storageContext = New-AzStorageContext itexperienceSN key-to-itexperience-net
$share = New-AzStorageShare logs -Context $storageContext
``` 

### CREATE AZURE FILE SYNC SERVICE

https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-planning

https://docs.microsoft.com/en-gb/azure/storage/files/storage-sync-files-deployment-guide

 

### CREATE AZURE SYNC GROUP

https://docs.microsoft.com/en-gb/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#create-a-sync-group-and-a-cloud-endpoint

 

### TROUBLESHOOT AZURE FILE SYNC

https://docs.microsoft.com/en-gb/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal

 

## IMPLEMENT AZURE BACKUP
### CONFIGURE AND REVIEW BACKUP REPORTS

https://docs.microsoft.com/en-us/azure/backup/backup-azure-configure-reports

 

### PERFORM BACKUP OPERATION

https://docs.microsoft.com/en-us/azure/backup/quick-backup-vm-portal

```
Enable-AzRecoveryServicesBackupProtection `
	-ResourceGroupName "myResourceGroup" `
	-Name "myVM" `
	-Policy $policy
``` 

### CREATE RECOVERY SERVICES VAULT

https://docs.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview

 

### CREATE AND CONFIGURE BACKUP POLICY

https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-vms-prepare

 

### PERFORM A RESTORE OPERATION

https://docs.microsoft.com/en-us/azure/backup/backup-azure-restore-files-from-vm


# AZ-103 STUDY GUIDE – PART 3 – DEPLOY AND MANAGE VIRTUAL MACHINES (VMS)#

Part 3 covers all objectives that are described in “Deploy and manage virtual machines (VMs)”.
15 to 20% of the exam questions are based on these subjects.

![part3](https://user-images.githubusercontent.com/38914947/61790945-394cd080-ae21-11e9-8830-065d3b25259c.png)

## DEPLOY AND MANAGE VIRTUAL MACHINES (VMS) (15-20%)
### CREATE AND CONFIGURE A VM FOR WINDOWS AND LINUX

Configure high availability:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability

Configure monitoring, networking, storage, and virtual machine size

Monitoring:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/monitor

Networking:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-common-network-ref

Storage:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq-for-disks

Virtual Machine Size:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm

Deploy and configure scale sets:

https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview

### AUTOMATE DEPLOYMENT OF VMS

Modify Azure Resource Manager (ARM) template:

https://docs.microsoft.com/en-us/azure/architecture/building-blocks/extending-templates/update-resource

Configure location of new VMs:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template
```
“location”: {
“type”: “string”,
“defaultValue”: “[resourceGroup().location]”,
“metadata”: {
“description”: “Location for all resources.”
}
}
```
Configure VHD template:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-specialized

Deploy from template:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/ps-template

Save a deployment as an ARM template:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/download-template

Deploy Windows and Linux VMs:

https://docs.microsoft.com/en-us/azure/virtual-machines/

### MANAGE AZURE VM
Add data disks:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-disk-ps

Add network interfaces

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-vm

Automate configuration management by using PowerShell Desired State Configuration (DSC) and VM Agent by using custom script extensions:

https://docs.microsoft.com/en-us/azure/automation/automation-dsc-overview

Manage VM sizes:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm

Move VMs from one resource group to another:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/move-vm

Redeploy VMs:

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/redeploy-to-new-node

### MANAGE VM BACKUPS
Configure VM backup:

https://docs.microsoft.com/en-us/azure/backup/quick-backup-vm-portal

Define backup policies:

https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-first-look-arm

Implement backup policies:

https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-vms-prepare

Terform VM restore:
https://docs.microsoft.com/en-us/azure/backup/backup-azure-restore-files-from-vm

Azure Site Recovery:
https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-overview

# AZ-103 STUDY GUIDE – PART 4 – CONFIGURE AND MANAGE VIRTUAL NETWORKS

Part 4 covers all objectives that are described in “Configure and manage virtual networks”. 
30-35% of the exam questions are based on these subjects.

![part4](https://user-images.githubusercontent.com/38914947/61790947-3b169400-ae21-11e9-8ea0-12a433fc8fa7.png)

## CONFIGURE AND MANAGE VIRTUAL NETWORKS (30-35%)
### CREATE CONNECTIVITY BETWEEN VIRTUAL NETWORKS

Create and configure VNET peering:

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-peering

Create and configure VNET to VNET:

https://docs.microsoft.com/en-us/azure/virtual-network/tutorial-connect-virtual-networks-portal

Verify virtual network connectivity:

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#troubleshoot

https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-connectivity-portal

Create virtual network gateway:

https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal

### IMPLEMENT AND MANAGE VIRTUAL NETWORKING

Configure private and public IP addresses, network routes, network interface, subnets, and virtual network:

https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal

### CONFIGURE NAME RESOLUTION

Configure Azure DNS:

https://docs.microsoft.com/en-us/azure/dns/dns-getstarted-portal

Configure custom DNS settings:

https://docs.microsoft.com/en-us/azure/dns/dns-custom-domain

Configure private and public DNS zones:

https://docs.microsoft.com/en-us/azure/dns/dns-operations-dnszones-portal

### CREATE AND CONFIGURE A NETWORK SECURITY GROUP (NSG)

Create security rules:

https://docs.microsoft.com/en-us/azure/virtual-network/manage-network-security-group

Associate NSG to a subnet or network interface


Subnet:

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-manage-subnet#change-subnet-settings

Network Interface:

https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface#associate-or-dissociate-a-network-security-group

Identify required ports:

https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Evaluate effective security rules:

https://docs.microsoft.com/en-us/azure/virtual-network/diagnose-network-traffic-filter-problem

### IMPLEMENT AZURE LOAD BALANCER

Configure internal load balancer, configure load balancing rules, configure public load balancer, troubleshoot load balancing

Configure internal load balancer:

https://docs.microsoft.com/en-us/azure/load-balancer/tutorial-load-balancer-basic-internal-portal

Configure load balancing rules:

https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-create-basic-load-balancer-portal#create-a-load-balancer-rule

Configure public load balancer:

https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-create-basic-load-balancer-portal

Troubleshoot load balancing:

https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-troubleshoot

### MONITOR AND TROUBLESHOOT VIRTUAL NETWORKING
Monitor on-premises connectivity, use Network resource monitoring, use Network Watcher, troubleshoot external networking, troubleshoot virtual network connectivity

Monitor on-premises connectivity:

https://docs.microsoft.com/en-us/azure/azure-monitor/overview

Use Network resource monitoring:

No direct Microsoft docs found

Use Network Watcher:

https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview

Troubleshoot external networking:

No direct Microsoft docs found, but try to be familiar with Express Route and VPN troubleshooting, like:

https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-troubleshoot-site-to-site-cannot-connect

Troubleshoot virtual network connectivity:

See Network Watcher. Like:

https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-connectivity-portal

### INTEGRATE ON PREMISES NETWORK WITH AZURE VIRTUAL NETWORK
Create and configure Azure VPN Gateway, create and configure site to site VPN, configure Express Route, verify on premises connectivity, troubleshoot on premises connectivity with Azure

Create and configure Azure VPN Gateway:

https://docs.microsoft.com/en-us/azure/vpn-gateway/create-routebased-vpn-gateway-portal

Create and configure site to site VPN:

https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal

Configure Express Route:

https://docs.microsoft.com/en-us/azure/expressroute/expressroute-howto-circuit-portal-resource-manager

Verify on premises connectivity:

https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-verify-connection-resource-manager

Troubleshoot on premises connectivity with Azure:

https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-troubleshoot-site-to-site-cannot-connect

# AZ-103 STUDY GUIDE – PART 5 – MANAGE IDENTITIES

Part 5 covers all objectives that are described in “Manage Identities”.
15-20% of the exam questions are based on these subjects.

![par5](https://user-images.githubusercontent.com/38914947/61790949-3ce05780-ae21-11e9-877a-5ce68761d215.png)

## MANAGE IDENTITIES (15-20%)
### MANAGE AZURE ACTIVE DIRECTORY (AD)
Add custom domains:

https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/domains-manage

Azure AD Join:

https://docs.microsoft.com/en-us/azure/active-directory/devices/azureadjoin-plan

Configure self-service password reset:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/quickstart-sspr

Manage multiple directories:

https://www.youtube.com/watch?v=0Jx_oftPCLY

### MANAGE AZURE AD OBJECTS (USERS, GROUPS, AND DEVICES)
Create users and groups:

https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal

Manage user and group properties:

https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-groups-settings-azure-portal

https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-users-profile-azure-portal

Manage device settings:

https://docs.microsoft.com/en-us/azure/active-directory/devices/overview

Perform bulk user updates:

https://docs.microsoft.com/en-us/powershell/azure/active-directory/importing-data?view=azureadps-2.0

Manage guest accounts:
https://docs.microsoft.com/en-us/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal

### IMPLEMENT AND MANAGE HYBRID IDENTITIES
Install Azure AD Connect, including password hash and pass-through synchronization:

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap

Use Azure AD Connect to configure federation with on-premises Active Directory Domain Services (AD DS):

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-custom

Manage Azure AD Connect:

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-post-installation

Manage password sync and password writeback:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-sspr-writeback

### IMPLEMENT MULTI-FACTOR AUTHENTICATION (MFA)
Configure user accounts for MFA, enable MFA by using bulk update, configure fraud alerts, configure bypass options, configure Trusted IPs, configure verification methods

Configure user accounts for MFA:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings

Enable MFA by using bulk update:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-getstarted#plan-conditional-access-policies

Configure fraud alerts:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#fraud-alert

Configure bypass options:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#one-time-bypass

Configure Trusted IPs, configure verification methods:

https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfa-mfasettings#trusted-ips

# OTHER USEFULL LINKS #

* Udemy Courses:
	https://www.udemy.com/70533-azure/
* Safaribook:
	https://learning.oreilly.com/videos/microsoft-az-103-azure/10009AZ100454545
* PluralSight:
	https://app.pluralsight.com/paths/certificate/microsoft-azure-administrator-az-103
* Microsoft learning github:
	https://github.com/MicrosoftLearning/AZ-103-MicrosoftAzureAdministrator
* Skylines academy:
	https://www.skylinesacademy.com/azure-study-resources?fbclid=IwAR2baQ0qEjShEV0TX6uTNc43ygjXyo6IM5zclQaoYeKhQD52JCMBGWLvT9k
* Microsoft Docs:
	https://docs.microsoft.com/en-us/learn/browse/?products=azure&roles=administrator&resource_type=learning%20path
* Microsoft youtube channel:
	https://www.youtube.com/user/windowsazure

	
# END OF AZ-103 STUDY GUIDE
