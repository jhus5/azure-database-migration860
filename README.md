# azure-database-migration860

# Azure Database Migration: Milestone 1 - Setting Up the Production Environment

## Overview
In this milestone, we outline the process of setting up a production environment for our Azure Database Migration project. 
This includes creating a virtual machine on Microsoft Azure and installing Azure SQL Server. 

## Prerequisites
To follow this guide, make sure you have:
1. An active Microsoft Azure account.
2. Familiarity with the Azure Portal.
3. Basic understanding of SQL Server, Virtual machines, and T-SQL scripting.

## Steps to Create a Production Environment
### 1. Creating an Azure Virtual Machine

Firstly, from the Azure portal homepage select the Create a resource button from the top left corner of the dashboard. 
Virtual Machine will appear under Popular Azure services, but can also be found under Compute in the Categories panel on the left side.

 <img src = screenshots/1a.png>

Click on Create to start the process of creating your first virtual machine. To do so, you will need to complete the following details:

<img src = screenshots/1b.png>

### Project details
Here, you will first have to select the Subscription (your account will only have one subscription to select from). A subscription refers to a billing and management container that holds and organizes resources, services, and the associated billing information for an Azure customer.

In this section you can allocate the VM to a specific resource group. Let's create a new resource group for the VM, using the Create new button and give it a name, for example wvm-rg in my case.

### Instance details
The mandatory fields to complete here are:
- Virtual machine name
- Region
- Availibility Zone (Keeping this as 'Zone 1')
- Image
- Size


### Virtual machine name
Region: Pick the region geographically closest to you, we selected UK (south), as this is the closest to us.

Image: This represents the operating system of your virtual machine. In the example we are provisioning a Windows VM so we selected the Windows 11 Pro image. (We will learn later how to connect to Windows VMs). 
To provision a Windows VM you will need to select a Windows image such as Windows 11 Pro.

Size: Select the appropriate size of the virtual machine based on the resource requirements of your workload. To change the size you can click on See all sizes under the Size pane. For this example we have selected a Standard_B2MS general purpose server. 


### Administrator account

<img src = screenshots/1c.png>

When creating a virtual machine in Azure, you have two main options for authentication: password-based authentication and SSH-based authentication. For this project we will use password-based authentication as we are using a Windows  VM.

#### Password-based authentication
When creating a virtual machine, you can specify an administrator username and password that will be used to log in to the virtual machine's operating system. The password should be strong and secure to prevent unauthorized access to the virtual machine.

### Inbound port rules
Inbound port rules are a set of instructions that determine how inbound network traffic is handled by a firewall or network security group (NSG). An NSG is a virtual firewall that controls inbound and outbound traffic to a virtual network.

By default, Azure blocks all incoming traffic to virtual machines, so you need to explicitly allow traffic through the Azure NSG. Thus we will select 'Allow selected ports' 

### Enabling RDP Access for Windows VMs
For Windows VMs, Remote Desktop Protocol (RDP) is used to enable remote access. The Administrator username and password you were prompted to set up will be used for RDP access to the VM. To avoid any issues connecting to the VM later, ensure that the RDP port (port 3389) is allowed in the NSG inbound rules for the VM. In most cases, the default rule is automatically created, allowing traffic on port 3389. If not, you might need to add or adjust the rule to permit RDP traffic.

### Finalising provisioning a Windows VM
Now that we have completed all the necessary information you will first have to tick I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights. before proceeding with the Review + create button. Azure will now validate the VM's configuration, and once the validation has passed we will click Create.

### 2. Connecting to a Virtual Machine

<img src = screenshots/1d.png>

### Connecting to a Windows VM
To connect to a Windows VM you can simply follow the instructions presented on the resource page under the Connect panel for RDP connection:

You will simply have to download the RDP connection file using the Download RDP file button. You will use this file to connect to the Windows VM, but first, you need to make sure your local machine has an RDP client installed.


### Remote Desktop Client - Linux
You can use the Remote Desktop Client from microsoft on Windows (from MS store) or Mac OS (from app store). But as we are using a Ubuntu (Linux) we will be using Remmina. 

Remmina is a popular open-source remote desktop client for Linux. It provides a graphical interface to connect to various remote desktop protocols, including RDP (Remote Desktop Protocol). If you have Remmina installed on your Linux system, you can launch it from the application menu or by typing remmina in the terminal.

<img src = screenshots/1e.png>

You can then open the previously downloaded .rdp file from Azure. This should prompt you to the following window:

<img src = screenshots/1g.png>

if you get a blank screen or face any issues, you can simply enter the vm's public IP address manually:

<img src = screenshots/1f.png>

To sign in you will have to use the username and password you have setup during the VM provisioning. You can leave the Domain field empty. Once you have successfully connected you will have to go through setting up your new Windows machine.

Once that is complete it should boot into the Windows VM.

<img src = screenshots/1h.png>

### 3. Installing SQL Server & SMSS on an Azure VM and Creating the Production database

#### Installing SQL Server & SMSS
The next step is to install the applications required on your VM, Here's a step-by-step guide on how to install SQL Server on an Azure VM:

1. Download SQL Server Installer: Download the SQL Server Developer installer from the Microsoft Download Center
2. Run SQL Server Installation Wizard: Locate the downloaded SQL Server installer on the Azure VM and run it to launch the SQL Server Installation Wizard
3. Choose Installation Type: Select the Basic option to begin a quick and simple SQL Server installation
4. Accept License Terms: Read and accept the Microsoft Software License Terms to proceed with the installation
5. Choose Installation Location: Specify the SQL Server install location then proceed by clicking Install
6. Installation Progress: The SQL Server installation process will commence, and you can monitor the progress as it installs the selected features
7. Download SQL Server Management Studio (SSMS): After the SQL Server installation is complete, proceed to install SQL Server Management Studio (SSMS) on the Azure VM using the Install SSMS button from the SQL Server Installation Wizard. This will redirect you to the Microsoft website. Download the latest version of SSMS from the Microsoft Download Center.
8. Install SQL Server Management Studio (SSMS): Run the SSMS installer on the Azure VM and follow the on-screen instructions to install the tool

### 4. Connecting to SQL Server Using SSMS

We will follow these steps to connect to SQL Server:

1. Launch SSMS : Open SQL Server Management Studio by searching for SSMS in the Windows Start Menu or by clicking its icon on your desktop.

<img src = screenshots/1m.png>

2. Connect to Server: After launching SSMS, you will be prompted to connect to a server. In the Connect to Server window, enter the server name or IP address of the SQL Server instance you want to connect to. This might be automatically filled up for you with your server name. 
<img src = screenshots/1n.png>

3. Connect: Click the Connect button to initiate the connection to the SQL Server instance using the specified credentials

4. Connected to SQL Server: Upon successful connection, you will see the Object Explorer window will open on the left side of the SSMS interface.

<img src = screenshots/1o.png>

### 5. Restoring Database in SQL Server
Now we are at the stage to create our production database. The database we utilised is the AdventureWorks sample databases, which can be found at: https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms

We then resotored the database to our SQL server using SSMS.

You can achieve this by follwing these steps (also described in the link above):

1. Download the appropriate .bak file from one of links provided (see above).

2. Move the .bak file to your SQL Server backup location. We did this to the defauly location on our VM:

C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup

3. Open SSMS and connect to your SQL Server instance.

4. Right-click Databases in Object Explorer > Restore Database... to launch the Restore Database wizard.
<img src = screenshots/1i.png>

5. Select Device and then select the ellipses (...) to choose a device.
<img src = screenshots/1j.png>
6. Select Add and then choose the .bak file you recently moved to the backup location.
<img src = screenshots/1k.png>
7. Select OK to confirm your database backup selection and close the Select backup devices window.

8. Check the Files tab to confirm the Restore as location and file names match your intended location and file names in the Restore Database wizard.

9. Select OK to restore your database.
<img src = screenshots/1l.png>

We are now ready to move onto migrating the database from our vm (on-premise) to the azure sql server (cloud).

## Conclusion
In this milestone, we've set up a production environment for our Azure Database Migration project by creating an Azure Virtual Machine, installing SQL Server and SSMS, and restoring a backup database. We're now ready to proceed with migrating this database to Azure SQL Database in the upcoming milestones. Stay tuned for more!

## Migrating database to Azure SQL Server
In order to migrate the databse to the new development environment we can utilise Azure Data Studio and acrry out the following steps:

- Creating an Azure SQL Database
- Install and connect to Azure Database Studio
- Schema Migration
- Data Migration

Let's begin!

### Azure SQL Database

Creating an Azure SQL Database, which will serve as the target for migrating your on-premise database.

1. Go to Azure Portal and search for 'SQL databases' or click on the icon
<img src = screenshots/2a.png>
2. To create a SQL database click on the Create button, and begin filling in the required information. 
<img src = screenshots/2b.png>

For Subscription select the subscription associated with the account, and for Resource group create a new one, here we called it 'AdventureWorks2022'. 
<img src = screenshots/2c.png>

We need to create a server at this point. Select 'Create new', and add details of the server name, location and authentication method (SQL authentication) on the following screen
<img src = screenshots/2i.png>

We changed the compute tier to basic for this exercise.

<img src = screenshots/2d.png>

Click 'Create' and wait for the Azure Database to be created. This can take a few minutes.

<img src = screenshots/2e.png>

By default, the Public Network Access setting is set to Deny, therefore we need to change this.

In order to do this you need to select the new SQL database and select 'Configure' from the overview page:

<img src = screenshots/2f.png>

Then select 'Selected networks' under Public network access and click on save. This step is important.

<img src = screenshots/2g.png>

This then allows you to add IP address. However, if you're environments are on the Azure cloud then you can simply check 'Allow azure services and resources to access this server' and you should be good. Then select save.

<img src = screenshots/2h.png>


### Azure Database Studio

1. Install and configure Azure Data Studio (download page: https://learn.microsoft.com/en-us/azure-data-studio/download-azure-data-studio) on your production Windows VM. 

2. Use Azure Data Studio to establish a connection to the existing on-premise database.
Click server icon and select new connection
<img src = screenshots/2j.png>
and enter connection details such as server name (in this case localhost), Windows Authentication, and login credentials; and enable Trust server certificate.
<img src = screenshots/2k.png>
Then click connect to establish a connection to the (local) server.

3. Repeat the above steps to connect to the Azure SQL Database.

This time use the server name and SQL Server authentication and login details from the Azure SQL Server that you created, in the previous step. If you have any connection issues go back to Azure SQL Database to ensure that you have enabled 'selected networks' and 'Allow azure services and resources to access this server'. 


### Schema Migration
After establishing connections to both databases, proceed to: 
1. install the SQL Server Schema Compare extension within Azure Data Studio.
- click on the Extensions icon in the Activity Bar. Search for and install the SQL Server Schema Compare extension.
<img src = screenshots/2l.png>

2. Leverage this extension to compare and subsequently migrate the schema from the on-premise database to the Azure SQL Database.

- Once the SQL Server Schema Compare extension is installed, click on the Server icon in the Activity Bar. Select your local SQL Server database from the list of connected servers.
<img src = screenshots/2m.png>
- Right-click on the database and choose Schema Compare to create a new migration project
<img src = screenshots/2n.png>
- In the Schema Compare dialog, configure the source connection to your local SQL Server database and the target connection to your Azure SQL Database.

<img src = screenshots/2o.png>

- Click Compare at the top of the Schema Compare page in Azure Data Studio.
<img src = screenshots/2p.png>
- Select all desired schema changes and click Apply at the top of the page. Confirm "Yes" when prompted to update the target.
- The local database schema will now match that of the Azure SQL Database after applying selected schema changes. This foundation prepares you for further steps in the migration process.
- Upon successful application of the schema changes, you will see "Apply schema compare changes succeeded." Refresh the Tables node in your Azure SQL connection to view the updated schema.
<img src = screenshots/2p.png>

Note this does not mean any data has been migrated yet. We shall do this in the next stage.


### Data Migration and validation
1. Data migration. With the successful execution of the schema migration, you are now prepared to move forward with the data migration phase. Begin by installing the Azure SQL Migration extension within Azure Data Studio.
- In Azure Data Studio, go to the Extensions view and search for the Azure SQL Migration extension and add this extension to your environment.
<img src = screenshots/2r.png>
- Click on the Server icon and select your desired your local SQL Server database from the list of connected servers

- Right-click on the server and select Manage. In the following window, under General click on Azure SQL Migration and then click on the Migrate to Azure SQL button
<img src = screenshots/2s.png>

<img src = screenshots/2t.png>

- This will open the Migration Wizard. In the first step you will be asked to select the database you want to migrate (in this case sales_database).

<img src = screenshots/2u.png>

- In the following step the Wizard will assess if the database can be migrated to Azure and will provide you with potential Azure SQL targets. Then click 'Next'. we will select 'Azure SQL Database'.

<img src = screenshots/2v.png>

- For Step 3 of the Migration Wizard, you will have to configure the Azure SQL target. Start by selecting your desired subscription, resource group and SQL Database Server. Then you will have to provide the login credentials to connect to this server. Once successfully connected you will be able to select the desired target database.


<img src = screenshots/2w.png>

we then need to download and install integration runtime, then use the generated Key(s) to register the integration runtime.
<img src = screenshots/2x.png>

In the next step we need to first provide the password for Windows authentication to the local SQL Server and select the tables that we want to migrate from source to target. 
<img src = screenshots/2y.png>

Then run validation. Once this is successful we can proceed tgo start migration. 

<img src = screenshots/2z.png>

2. To confirm the success of the database migration process, you can carry out a check by running sql queries for comparisons.
You can do so by selecting a table from within the Azure Server > right-click and 'Select top 1000'.
<img src = screenshots/2aa.png>

You should now see entries in these databses:

<img src = screenshots/2ab.png>


# Azure Database Migration: Milestone 4 - Data Backup and Restoration

In this milestone, we'll outline the process of backing up our production database, securely storing backups in Azure Blob Storage, restoring the backups to a development environment, and setting up automated backups using Maintenance Plans in SQL Server Management Studio (SSMS).

## Backing Up the Production Database

Backing up your production database is a crucial step to ensure data protection and availability. We'll use SQL Server Management Studio (SSMS) to create a full backup:

1. Connect to your production database in SSMS.
2. Right-click on the database in Object Explorer and select `Tasks > Backup`.

<img src = screenshots/4a.png>

3. In the Back Up Database dialog, choose a descriptive name for your backup file, such as `ProductionDatabase_YYYYMMDD_HHMMSS`.
4. Choose the backup type as `Full`, and select your backup destination as a local file. Ensure the backup file is saved in a secure location, such as a dedicated backup folder.

<img src = screenshots/4b.png>

5. Click `OK` to start the backup process.
6. Monitor the progress of the backup in the Output window and wait for it to complete.

## Securely Storing Backups in Azure Blob Storage

To improve data security, we'll move our backups to Azure Blob Storage:

1. Sign in to the [Azure Portal](https://portal.azure.com).
2. Click on `Storage accounts` in the left sidebar under `Storage`.
3. Create a new storage account or use an existing one. Make sure to note down the storage account name and access key for future reference.
In our case we created a new storage account by selecting 'create' and proceeding to complete the fields as below:
<img src = screenshots/4c.png>.
In the next step just confirm, agree and create. Then proceed onto the next step.
<img src = screenshots/4d.png>
4. In the left sidebar, click on `Blob services` under your storage account.
<img src = screenshots/4e.png>
5. Create a new container and provide it with a name such as `backups` in our case we will it 'aw22-container'. Set appropriate access policies and permissions as needed.
<img src = screenshots/4f.png>
<img src = screenshots/4g.png>
6. Optional, you can install the Azure Storage Explorer to manage your blob storage easily.

You can then access the container and #Upload' any file. In our case we uploaded the backup file we previously created to safely store in on the cloud.

<img src = screenshots/4h.png>


## Creating a Development environment. 
Now that we have our production environment up and running, we will create a development environment, where users can create, test, and modify the database without affecting the production environment.

In order to do this please go back and follow the above steps and provide a new name, in or case we named it 'dev-env-wvm' to the previous name.

<img src = screenshots/4i.png>


## Executing Automated Backups via Maintenance Plans in SSMS

To automate backups, we'll create a maintenance plan in SQL Server Management Studio:

1. Connect to your production database in SSMS.
<img src = screenshots/4i.png>
2. Right-click on the `Management` node and select `Maintenance Plans`.
3. Click on `New Maintenance Plan`.
<img src = screenshots/4j.png>
4. Give your maintenance plan a descriptive name, such as `Weekley_Backup`.
<img src = screenshots/4ka.png>
<img src = screenshots/4l.png>
5. Add a new task to the maintenance plan by clicking on `Tasks > Backup > Full Database`.
<img src = screenshots/4n.png>
<img src = screenshots/4n.png>
6. Configure your backup settings, such as the destination type (local file or Azure Blob Storage), naming conventions, and frequency.
<img src = screenshots/4n.png>
7. Save and run your maintenance plan to start automating regular backups of your production database.


## Milestone 5: Disaster Recovery Simulation
As a test to restore database, I selected to delete the following three tables from the database.

AdventureWorks22 > Tables and then proceeded to delete the first three tables:
(a) AWBuildVersion
(b) dbo.Database.log
(c) dbo.ErrorLog

<img src = screenshots/5a.png>

2. You can see some errors already from the assessment summary: 

 <img src = screenshots/5b.png>


and the three tables are missing from a quick inspection of the database from azure data studio:

 <img src = screenshots/5c.png>


before backup:

 <img src = screenshots/5d.png>

restoring database:

 <img src = screenshots/5e.png>
 <img src = screenshots/5f.png>
also verified successful backup in azure data studio:

 <img src = screenshots/5g.png>
 
Carried out the same exercise from azure cloud services. This time I restored data back back a few hours, at 10AM this morning, before mimicking data loss:

 <img src = screenshots/5h.png>

Deployment completed.

 <img src = screenshots/5i.png>


## Milestone 6: Geo replication and failover (Refer to odt file for screenshots)
 
### Geo replication of the same server in Azure SQL datbase:

<img src = screenshots/6a.png>

now we have two: primary and geo replica:

 <img src = screenshots/6b.png>


### Test Failover and Tailback

Initiating a Test Failover to the Secondary Region

Create your failover group and add your single database to it using the Azure portal.
1. Select Azure SQL in the left-hand menu of the Azure portal. If Azure SQL isn't in the list, select All services, then type Azure SQL in the search box. (Optional) Select the star next to Azure SQL to favorite it and add it as an item in the left-hand navigation.
2. Select the database you want to add to the failover group.
3. Select the name of the server under Server name to open the settings for the server.
  
4. Select Failover groups under the Settings pane, and then select Add group to create a new failover group.
       
5. On the Failover Group page, enter or select the required values, and then select Create. Either create a new secondary server, or select an existing secondary server. The secondary server in the failover group must be in a different region than the primary server.
        ◦ Databases within the group: Choose the database you want to add to your failover group. Adding the database to the failover group will automatically start the geo-replication process.
       

 <img src = screenshots/6f.png>

going to resource:
 <img src = screenshots/6g.png>

Access fail-over groups:
 <img src = screenshots/6h.png>

which should now indicate two servers in the fail-over group:
 <img src = screenshots/6i.png>

select failover and yes:
 <img src = screenshots/6j.png>


in a few moments you will see the servers swap roles where the replica is now the primary:
 <img src = screenshots/6k.png>

Select Failover again to fail the servers back to their original roles

By following these steps, you can safely perform a test failover to verify your disaster recovery environment's functionality. and then perform a tailback to revert to the primary region. 
 <img src = screenshots/6l.png>

Result of tailback:
 <img src = screenshots/6m.png>


## Milestone 7: MS Entra id directory integration

Make sure you click ‘save’ then disconnect from current connection and manage connection. Change to ‘Microsoft entra ID’

 <img src = screenshots/7a.png>

We will be unable to connect vis azure data studio if you fail to save previous action, as we experienced:

 <img src = screenshots/7b.png>

Thus, we had to return to the previous screen on azure services (on browser window) and click save. This allowed us to log back in. 



### Creating a DB Reader User:

 <img src = screenshots/7c.png>

Test the DB Reader's User Access:

 <img src = screenshots/7d.png>


you will be …. prompted to change the password on first login:

 <img src = screenshots/7e.png>
 
 <img src = screenshots/7f.png>
 
 <img src = screenshots/7g.png>

Depending on your companies setup, you may be asked to setup MFA for the new user account. If this is the case, have your preferred MFA app at hand so you can follow the steps to authenticate the new user. 

 <img src = screenshots/7h.png>

You can then proceed to connecting via azure data studio with the new credentials:
After logging you should check that you have setup the user, testing reading the database with success:

 <img src = screenshots/7i.png>

testing access to modify to database which should fail, as this user should correctly only have read access.

 <img src = screenshots/7j.png>
 
 ## This completes the project. We have now:

- setup a production (on-prem) database with a cloud backup on a weekly schedule
 <img src = screenshots/8a.png>
 - installed utilities like Azure Datbase Studio (with extensions) and SSMS to backup and migrate data
 <img src = screenshots/8b.png>
 - setup a development environement, mimicking the production environment, for developers to work on setup azure storage services so we can back up and restore dev environements
  <img src = screenshots/8c.png>
 - setup a geo-replca of the production database as a failover in any data loss event on our primary server
   <img src = screenshots/8d.png>
 - and finally we controlled access to the production environment by using MS Entra ID to create sepearte Admina dn User (read-only) accounts.
   <img src = screenshots/8e.png>

The overall picture of the project on azure cloud can be seen on the following uml diagram:
    <img src = screenshots/8f.png>

