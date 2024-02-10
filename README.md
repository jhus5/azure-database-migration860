# azure-database-migration860

# Azure Database Migration: Milestone 1 - Setting Up the Production Environment

## Overview
In this milestone, we outline the process of setting up a production environment for our Azure Database Migration project. 
This includes creating a virtual machine on Microsoft Azure and installing SQL Server 2019 Express LocalDB in production. 
Additionally, we'll create the production database.

## Prerequisites
To follow this guide, make sure you have:
1. An active Microsoft Azure account.
2. Familiarity with the Azure Portal.
3. Basic understanding of SQL Server, Virtual machines, and T-SQL scripting.

## Steps to Create a Production Environment
### 1. Creating an Azure Virtual Machine

Firstly, from the Azure portal homepage select the Create a resource button from the top left corner of the dashboard. Virtual Machine will appear under Popular Azure services, but can also be found under Compute in the Categories panel on the left side.

Click on Create to start the process of creating your first virtual machine. To do so, you will need to complete the following details:

### Project details
Here, you will first have to select the Subscription (your account will only have one subscription to select from). A subscription refers to a billing and management container that holds and organizes resources, services, and the associated billing information for an Azure customer.

In this section you can allocate the VM to a specific resource group. Let's create a new resource group for the VM, using the Create new button and call it my-vm-rg.

### Instance details
The mandatory fields to complete here are:

### Virtual machine name
Region: Pick the region geographically closest to you
Image: This represents the operating system of your virtual machine. In the example we are provisioning a Windows VM using the Windows 11 Pro image. Later we will learn how to connect to Windows VMs. To provision a Windows VM you will need to select a Windows image such as Windows 11 Pro.
Size: Select the appropriate size of the virtual machine based on the resource requirements of your workload. To change the size you can click on See all sizes under the Size pane. Find the B2ms general purpose VM, a VM that is good for many workloads, and then press Select

### Administrator account
When creating a virtual machine in Azure, you have two main options for authentication: password-based authentication and SSH-based authentication.

Password-based authentication
Password-based authentication is the most common method of authentication for Windows virtual machines in Azure. When creating a virtual machine, you can specify an administrator username and password that will be used to log in to the virtual machine's operating system. The password should be strong and secure to prevent unauthorized access to the virtual machine.

For example, when provisioning a Windows VM you will be met with a window like this under the Administrator account section:

### SSH-based authentication
SSH-based authentication is the most common method of authentication for Linux virtual machines in Azure. SSH (Secure Shell) is a network protocol that allows secure remote access to the virtual machine's command-line interface.

When creating a virtual machine, you can specify an SSH public key that will be used to authenticate access to the virtual machine's operating system. When provisioning a Linux VM you will have the following specifications for the Administrator account:


### Inbound port rules
Inbound port rules are a set of instructions that determine how inbound network traffic is handled by a firewall or network security group (NSG). An NSG is a virtual firewall that controls inbound and outbound traffic to a virtual network.

In the context of VMs, inbound port rules are used to control incoming network traffic to a virtual machine. By default, Azure blocks all incoming traffic to virtual machines, so you need to explicitly allow traffic through the Azure NSG.

### Enabling RDP Access for Windows VMs
For Windows VMs, Remote Desktop Protocol (RDP) is used to enable remote access. The Administrator username and password you were prompted to set up will be used for RDP access to the VM. To avoid any issues connecting to the VM later, ensure that the RDP port (port 3389) is allowed in the NSG inbound rules for the VM. In most cases, the default rule is automatically created, allowing traffic on port 3389. However, if it is missing or modified, you might need to add or adjust the rule to permit RDP traffic.

### Finalizing provisioning a Windows VM
Now that we have completed all the necessary information you will first have to tick I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights. before proceeding with the Review + create button. Azure will now validate the VM's configuration, and once the validation has passed we will click Create.

### 2. Connecting to a Virtual Machine
### Connecting to a Windows VM
To connect to a Windows VM you can simply follow the instructions presented on the resource page under the Connect panel for RDP connection:

You will simply have to download the RDP connection file using the Download RDP file button. You will use this file to connect to the Windows VM, but first, you need to make sure your local machine has an RDP client installed.


### Remote Desktop Client - Linux
=You can use the Remote Desktop Client from microsoft on Windows (from MS store) and Mac OS (from app store). But as I am on Ubuntu (Linux) I will be using Remmina. 

Remmina is a popular open-source remote desktop client for Linux. It provides a graphical interface to connect to various remote desktop protocols, including RDP (Remote Desktop Protocol). If you have Remmina installed on your Linux system, you can launch it from the application menu or by typing remmina in the terminal.

You can then open the previously downloaded .rdp file from Azure. This should prompt you to the following window:

To sign in you will have to use the username and password you have setup during the VM provisioning. You can leave the Domain field empty. Once you have successfully connected you will have to go through setting up your new Windows machine.

### 3. Installing SQL Server on an Azure VM

Here's a step-by-step guide on how to install SQL Server on an Azure VM:

1. Create an Azure Virtual Machine: If you haven't created an Azure VM yet, create one before proceeding with the next steps
2. Connect to the Azure VM :Use Remote Desktop Protocol (RDP) or your preferred method to connect to the Azure VM. Ensure you have the necessary credentials to log in.
3. Download SQL Server Installer: Download the SQL Server Developer installer from the Microsoft Download Center
4. Run SQL Server Installation Wizard: Locate the downloaded SQL Server installer on the Azure VM and run it to launch the SQL Server Installation Wizard
5. Choose Installation Type: Select the Basic option to begin a quick and simple SQL Server installation
6. Accept License Terms: Read and accept the Microsoft Software License Terms to proceed with the installation
7. Choose Installation Location: Specify the SQL Server install location then proceed by clicking Install
8. Installation Progress: The SQL Server installation process will commence, and you can monitor the progress as it installs the selected features
9. Download SQL Server Management Studio (SSMS): After the SQL Server installation is complete, proceed to install SQL Server Management Studio (SSMS) on the Azure VM using the Install SSMS button from the SQL Server Installation Wizard. This will redirect you to the Microsoft website. Download the latest version of SSMS from the Microsoft Download Center.
10. Install SQL Server Management Studio (SSMS): Run the SSMS installer on the Azure VM and follow the on-screen instructions to install the tool
11. Verify SQL Server Installation: After installation, ensure that you can connect to SQL Server using SSMS or other client tools to confirm a successful setup

### 4. Connecting to SQL Server Using SSMS
Once you have installed SSMS on your local machine or an Azure Virtual Machine, you can follow these steps to connect to SQL Server:

1. Launch SQL Server Management Studio : Open SQL Server Management Studio by searching for SSMS in the Windows Start Menu or by clicking its icon on your desktop.
2. Connect to Server: After launching SSMS, you will be prompted to connect to a server. In the Connect to Server window, enter the server name or IP address of the SQL Server instance you want to connect to. This might be automatically filled up for you with your server name. 
3. Connect: Click the Connect button to initiate the connection to the SQL Server instance using the specified credentials
4. Connected to SQL Server: Upon successful connection, you will see the Object Explorer window will open on the left side of the SSMS interface.
5. 

### 5. Restoring Database in SQL Server

1. Identify the Backup Files
Before restoring a database, ensure that you have the necessary backup files available. Once you have this file you will have to copy it to C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup or the equivalent SQL Server folder on your local machine.

2. Connect to SQL Server
Open SQL Server Management Studio (SSMS) and connect to the SQL Server instance where you want to perform the database restoration.

3. Initiate Database Restore
Right-click on the Databases node in the Object Explorer, and then choose Restore Database...

4. Select the Source
In the Restore Database window, choose the Device option and click the ... button to select the backup files to restore. Click the Add button to navigate to the location of the backup files, select the appropriate full backup, and if needed, the differential and transaction log backups.

5. Choose the Restore Options
On the General page, ensure that the destination database name is correct. Optionally, specify a new database name if you want to restore with a different name. Click OK to perform the restoration. Once the restore has been completed you will be met with a message as the following: Database restored successfully.

## Conclusion
In this milestone, we've set up a production environment for our Azure Database Migration project by creating an Azure Virtual Machine, installing SQL Server and SSMS, and restoring a backup database. We're now ready to proceed with migrating this database to Azure SQL Database in the upcoming milestones. Stay tuned for more!

# Azure Database Migration: Milestone 4 - Data Backup and Restoration

In this milestone, we'll outline the process of backing up our production database, securely storing backups in Azure Blob Storage, restoring the backups to a development environment, and setting up automated backups using Maintenance Plans in SQL Server Management Studio (SSMS).

## Backing Up the Production Database

Backing up your production database is a crucial step to ensure data protection and availability. We'll use SQL Server Management Studio (SSMS) to create a full backup:

1. Connect to your production database in SSMS.
2. Right-click on the database in Object Explorer and select `Tasks > Backup`.
3. In the Back Up Database dialog, choose a descriptive name for your backup file, such as `ProductionDatabase_YYYYMMDD_HHMMSS`.
4. Choose the backup type as `Full`, and select your backup destination as a local file. Ensure the backup file is saved in a secure location, such as a dedicated backup folder.
5. Click `OK` to start the backup process.
6. Monitor the progress of the backup in the Output window and wait for it to complete.

## Securely Storing Backups in Azure Blob Storage

To improve data security, we'll move our backups to Azure Blob Storage:

1. Sign in to the [Azure Portal](https://portal.azure.com).
2. Click on `Storage accounts` in the left sidebar under `Storage`.
3. Create a new storage account or use an existing one. Make sure to note down the storage account name and access key for future reference.
4. In the left sidebar, click on `Blob services` under your storage account.
5. Create a new container named `backups`. Set appropriate access policies and permissions as needed.
6. Install the Azure Storage Explorer to manage your blob storage easily.

## Seamless Restoration onto the Development Environment

To restore a backup to a development environment, follow these steps:

1. Connect to your development database in SSMS.
2. Right-click on the `Databases` node and select `Restore Database`.
3. In the Restore Database dialog, choose a descriptive name for your restored database, such as `ProductionDatabase_Dev_YYYYMMDD_HHMMSS`.
4. Select the backup file you created earlier from your local file system or Azure Blob Storage as the source.
5. Choose `ProductionDatabase` as the destination database.
6. Set the restore type to `Overwrite the existing database`.
7. Click `OK` to start the restoration process.
8. Monitor the progress of the restoration in the Output window and wait for it to complete.

## Executing Automated Backups via Maintenance Plans in SSMS

To automate backups, we'll create a maintenance plan in SQL Server Management Studio:

1. Connect to your production database in SSMS.
2. Right-click on the `Management` node and select `Maintenance Plans`.
3. Click on `New Maintenance Plan`.
4. Give your maintenance plan a descriptive name, such as `ProductionDatabase_Backup`.
5. Add a new task to the maintenance plan by clicking on `Tasks > Backup > Full Database`.
6. Configure your backup settings, such as the destination type (local file or Azure Blob Storage), naming conventions, and frequency.
7. Add any necessary steps for cleaning up old backups, such as deleting them after a certain retention period.
8. Save and run your maintenance plan to start automating regular backups of your production database.


## Milestone 5: Data recovery simulation (Refer to odt file for screenshots)
As a test to restore database, I selected to delete the following three tables from the database.

AdventureWorks22 > Tables and then proceeded to delete the first three tables:
(a) AWBuildVersion
(b) dbo.Database.log
(c) dbo.ErrorLog


2. You can see some errors already from the assessment summary: 




and the three tables are missing from a quick inspection of the database from azure data studio:




before backup:







restoring database:



also verified successful backup in azure data studio:


Carried out the same exercise from azure cloud services. This time I restored data back back a few hours, at 10AM this morning, before mimicking data loss:

 

Deployment completed.








## Milestone 6: Geo replication and failover (Refer to odt file for screenshots)
 
### Geo replication of the same server in Azure SQL datbase:


now we have two: primary and geo replica:



### Test Failover and Tailback

Initiating a Test Failover to the Secondary Region

Create your failover group and add your single database to it using the Azure portal.
    1. Select Azure SQL in the left-hand menu of the Azure portal. If Azure SQL isn't in the list, select All services, then type Azure SQL in the search box. (Optional) Select the star next to Azure SQL to favorite it and add it as an item in the left-hand navigation.
    2. Select the database you want to add to the failover group.
    3. Select the name of the server under Server name to open the settings for the server.
       
    4. Select Failover groups under the Settings pane, and then select Add group to create a new failover group.
       
    5. On the Failover Group page, enter or select the required values, and then select Create. Either create a new secondary server, or select an existing secondary server. The secondary server in the failover group must be in a different region than the primary server.
        ◦ Databases within the group: Choose the database you want to add to your failover group. Adding the database to the failover group will automatically start the geo-replication process.
       




going to resource:




Access fail-over groups:




which should now indicate two servers in the fail-over group:


select failover and yes:



in a few moments you will see the servers swap roles where the replica is now the primary:

Select Failover again to fail the servers back to their original roles

By following these steps, you can safely perform a test failover to verify your disaster recovery environment's functionality. and then perform a tailback to revert to the primary region. 


Result of tailback:






## Milestone 7: MS Entra id directory integration

make sure you click ‘save’

then disconnect from current connection and manage connection. Change to ‘Microsoft entra ID’


you will be unable to connect vis azure data studio if you fail to save previous action, as I found out:



Thus I had to return to the previous screen on azure services (on browser window) and click save. This allowed me to log back in. 



### Creating a DB Reader User:


Test the DB Reader's User Access:




you will be …. prompted to change the password on first login:

















Depending on your companies setup, you may be asked to setup MFA for the new user account. If this is the case, have your preferred MFA app at hand so you can follow the steps to authenticate the new user. 


You can then proceed to connecting via azure data studio with the new credentials:
After logging you should check that you have setup the user, testing reading the database with success:


testing access to modify to database which should fail, as this user should correctly only have read access.


