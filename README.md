# Genetec Security Center and EXPRESSCLUSTER Quick Start Guide

1. Installing Security Center

   1. Execute `setup.exe`
   2. On setting *Database Server*

      Select `Install a new database server` (change from *Use an existing database server*).

   3. On setting *Database Server Authentication*

      Select *SQL Server and Windows authentication (mixed mode)* (change from *Windows authentication**).

      Input `sa` as *Username* and `Password-0` as *Password* for example.

2. Configuring SQL Server : enabling login property of *sa* user

   1. Open *SQL Server Management Studio*
   2. Connect to `localhost\SQLEXPRESS` with *Windows Authentication*
   3. Go to *Security/Logins*. Confirm if `sa` user is in Disabled status.
   4. If disabled, open **Login Properties** of *sa* user. Go to *Status*. Select `Enabled` for *Login* then click *OK*.

3. Configuring Security Center to add an RTSP video

   1. Open `http://localhost/Genetec/` and login to *Security Center Server Admin*
   2. Install the license.
   3. Enable the database
      1. Click the hostname in left pane.
      2. Select `SQL Server` as *Authentication* (change from *Windows*).
      3. Input `Password-0` as *Password* for example.
   4. Open *Genetec Config Tool*. `admin` with no password is used for first time login.
   5. Set admin password `Password-0` (example).
   6. Go to *Tasks*` > *Video*
   7. Click *+Video unit*, set followings then *Add and close*
  
      - *Manufacturer*: `Generic Stream`
      - *Product type*: `*RTSP(No ping)`
      - rtsp://`10.0.0.10/live` (assuming RTSP server runs on 10.0.0.10)
      - *Authentication*: `Default logon`

## Configuring Security Center Server for the cluster

You must make some changes to Security Center Server to be protected with failover by the cluster.

Before configuring, Stop Security Center services and set the *Startup Type* of them into *Manual startup*.

```bat
sc qc     GenetecServer
sc stop   GenetecServer
sc config GenetecServer   start= demand
sc qc     GenetecServer
sc qc     GenetecWatchdog
sc stop   GenetecWatchdog
sc config GenetecWatchdog start= demand
sc qc     GenetecWatchdog
```

**To configure Security Center Server for the cluster:**

1. Move the server's license file to a non-mirrored folder, and move the configuration files to a mirrored disk.
2. Stop the Genetec™ Watchdog service from restarting the Genetec™ Server service
   The cluster will monitor and control the Genetec™ Server service, so you do not want the Genetec™ Watchdog service trying to start or stop the server anymore.

### Moving Security Center server's license and configuration files

As part of configuring Security Center Server for a clustered environment, you must move the server's license file to a non-mirrored folder, and move the configuration files to a mirrored disk.

**To move Security Center server's license and configuration files:**

1. On both servers, stop the Genetec™ Watchdog service and the Genetec™ Server service.

   <!--
   1. Click **Start > Control Panel > Administrative Tools > Services**.
   2. Select the *Genetec™ Watchdog service* and click **Stop Service** on the toolbar at the top of the page.
   3. Select the Genetec Server service and click **Stop Service** on the toolbar at the top of the page.
   4. Minimize, but don't close the Services window.
   -->

   open *cmd.exe* then issue the following commands

   ```bat
   sc qc     GenetecServer
   sc stop   GenetecServer
   sc config GenetecServer start= demand
   sc qc     GenetecServer
   sc qc     GenetecWatchdog
   sc stop   GenetecWatchdog
   sc config GenetecWatchdog start= demand
   sc qc     GenetecWatchdog
   ```

2. Move the Genetec™ Server's license file (*license.gconfig*) from *C:\Program Files (x86)\Genetec Security Center 5.12\ConfigurationFiles* to the Security Center root folder (*C:\Program Files (x86)\Genetec Security Center 5.12*).
3. Using Notepad, paste the following text into an empty text file:

   ```xml
   <?xml version="1.0" encoding="utf-8" ?>
   <configurationPath path="N:\Genetec Security Center 5.12\ConfigurationFiles">
   <forceRoot name="License"/>
   </configurationPath>
   ```

   This file contains four lines. It will point to your mirrored data drive (“N:” is used in the example) as the location of your configuration files and your local root folder as the location of your license file. If your mirrored data drive is configured differently, adjust the file with your mirrored drive path.

4. Save the text file with the name *ConfigurationPath.gconfig* in your Security Center root folder (*C:\Program Files (x86)\Genetec Security Center 5.12*)
5. Repeat the steps on your standby server.

   ```bat
   robocopy 'C:\Program Files (x86)\Genetec Security Center 5.12\ConfigurationFiles' N:\Genetec Security Center 5.12\ConfigurationFiles
   ```

6. Copy the folder *ConfigurationFiles* in the Security Center root folder on the active server to the mirrored data drive.

A supplementary server configuration file called *ConfigurationPath.gconfig* is created. This file tells the server to look in an alternative path for your configuration files (the mirrored data drive) and an alternative path for
the server license.

### Stopping Genetec™ Watchdog from restarting Genetec™ Server

Once clustering is set up, the cluster will monitor and control the Genetec™ Server service, so you do not want
the Genetec™ Watchdog service trying to start or stop the Genetec™ Server service anymore.

**To stop the Genetec™ Watchdog service from restarting the Genetec™ Server service:**

1. Use Notepad to open the file GenetecWatchdog.gconfig found in the path `N:\Genetec Security Center 5.11\ConfigurationFiles`.
2. Edit the file to include the string: preventServiceRestart="true".
The third line now reads:

   ```xml
   <Watchdog serverPort="4534" registrationRetryDelay="00:00:15"
   responsivenessTimeout="00:02:00" emailFilterLevel="None"
   preventServiceRestart="true">
   ```

3. Repeat the steps on your standby server.
4. Start the Genetec™ Server service on both servers.

### Configuring SQL Server for the cluster

To configure your SQL database server for a clustered environment, you must move the database to your mirrored data partition so that the contents of the database are identical on the active server and the
standby server.

To configure the SQL Server for the cluster:

1. On both servers, stop the Genetec™ Watchdog service and the Genetec™ Server service as follows:
   a) Click Start > Control Panel > Administrative Tools > Services
   b) Select the Genetec™ Watchdog service and click Stop Service on the toolbar at the top of the page.
   c) Double-click the Genetec™ Watchdog service and set the Startup Type to Manual .
   d) Select the Genetec Server service and click Stop Service on the toolbar at the top of the page.
   e) Double-click the Genetec™ Server service and set the Startup Type to Manual.
2. In SQL Management Studio, you will need detach all Security Center databases before the database files can be moved to your mirrored data partition. Detaching a database removes it from the instance of the Microsoft SQL Server but leaves intact the database, with its data files and transaction log files.
   a) Open SQL Management Studio and connect to the Security Center database instance (by default the instance name is SQLEXPRESS)
   b) Right click on each one of the Security Center databases and select `Task` > `Detach`.
3. Move the Security Center \*.MDF and \*.LDF database files from their default folder (*C:/Program Files/Microsoft SQL Server/MSSQL10_50.SQLEXPRESS/MSSQL/DATA*) to the SQL folder on the mirror partition created earlier (*N:/MSSQL/DATA*).
4. Stop the SQL service on both servers.
5. Using SQL Server Configuration Manager, modify the SQL startup parameters for the master database on both servers.
   a) Open your SQL Server Configuration Manager.
   b) Select **SQL Server Services** in the pane on the left.
   c) Right-click on **SQL Server (SQLEXPRESS)** and select **Properties**.
   d) Select the **Advanced** tab in the **SQL Server (SQLEXPRESS) Properties** window.
   e) Modify the field **Startup Parameters** to point to the new master database path.

      In this example we have modified the startup parameters to: `-dN:/MSSQL/Data/master.mdf;-eN:/MSSQL/Data/Log/ERRORLOG;-lN:/MSSQL/mastlog.ldf`,
      whereby the field points to the new (mirrored) drive and folder path of our master database (*N:/MSSQL/Data*).

6. Move the master database (master.mdf and master.ldf) for the active server to the SQL folder on the
mirror partition created earlier (*N:/MSSQL/DATA**). Delete the master database (master.mdf and master.ldf)
on the standby server (it will be replicated from the active server).
7. Restart SQL service on both servers.
8. Reconnect SQL Management studio to SQL server for the active server.
9. Re-attach the Security Center databases.
10. When prompted to point to the database to be attached, use the Add button to browse to the new path of
your (moved) master database.

    **IMPORTANT**: This step needs to be performed on the standby server as well but it is not possible at this
    point. Once the cluster configuration has been completed, you will need to force a failover to the standby
    server, then connect SQL Management Studio to its SQL server to re-attach the same databases.

11. Make sure that all the databases that were previously detached are re-attached.

## Configuring NEC ExpressCluster

### Creating the cluster

## Cluster configuration tests

Copyright Miyamoto Kazuyuki 2025
