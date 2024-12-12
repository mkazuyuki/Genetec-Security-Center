# Genetec Security Center and EXPRESSCLUSTER Quick Start Guide

1. Installing Security Center

	1. Execute `setup.exe`

	2. On setting **Database Server**

	   Select `Install a new database server` (change from *Use an existing database server*).

	3. On setting **Database Server Authentication**

	   Select `SQL Server and Windows authentication (mixed mode)` (change from *Windows authentication**).

	   Input `sa` as Username and `Password777` as Password for example.

2. Configuring SQL Server : login property of *sa* user

	1. Open **SQL Server Management Studio**
	2. Connect to `localhost\SQLEXPRESS` with *Windows Authentication*
	3. Go to `Security/Logins`. *sa* user is in Disabled status. Open **Login Properties** of *sa* user. Go to
 *Status*. Select `Enabled` as *Login* then click *OK*.


 3. Configuring Security Center

	1. Open http://localhost/Genetec/ and login to *Security Center Server Admin* 
	1. Click the hostname in left pane.
	1. Select `SQL Server` as *Authentication* (change from *Windows*).
	1. Input `Password777` as *Password* for example.

4. Running Genetec Config Tool

	1. set admin password Cluster-0


<div style="text-align: right;">Copyright Miyamoto Kazuyuki 2024</div>

