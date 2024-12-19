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
   4. If disabled, open **Login Properties** of *sa* user. Go to *Status*. Select `Enabled` as *Login* then click *OK*.

3. Configuring Security Center to add an RTSP video

   1. Open `http://localhost/Genetec/` and login to *Security Center Server Admin*
   2. Enabling the license.
   3. Enabling the database
      1. Click the hostname in left pane.
      2. Select `SQL Server` as *Authentication* (change from *Windows*).
      3. Input `Password-0` as *Password* for example.
   4. Open *Genetec Config Tool*. `admin` with no password is used for first time login.
   5. Set admin password `Password-0` (example).
   6. Go to *Tasks*` > *Video*
   7. Click *+Video unit*, set followings then *Add and close*
  
      - *Manufacturer*: `Generic Stream`
      - *Product type*: `*RTSP(No ping)`
      - rtsp://`10.0.0.10/live` (assuming OBS runs RTSP on 10.0.0.10)
      - *Authentication*: `Default logon`

<div style="text-align: right;">Copyright Miyamoto Kazuyuki 2024</div>
