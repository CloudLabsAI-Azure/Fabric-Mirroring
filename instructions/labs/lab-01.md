# Configure Microsoft Fabric mirrored databases from Azure SQL Database 

## Enable SAMI of your Azure SQL logical server

1. In the azure portal , search for the SQL server
   
   ![](../media/Lab-01/sql-servers.png)

1. Select the SQL servers. 

   ![](../media/Lab-01/server-1.png)

1. To configure or verify that the SAMI is enabled, go to your logical SQL Server in the Azure portal. Under Security in the resource menu, select Identity.

   ![](../media/Lab-01/sqldbserver01.png)

2. Under System assigned managed identity, select Status to On.

3. The SAMI must be the primary identity. Verify the SAMI is the primary identity with the following T-SQL query: 

   ```
   SELECT * FROM sys.dm_server_managed_identities;
   ```

## Connect to your Azure SQL logical server using SQL Server Management Studio (SSMS) or the mssql extension with Visual Studio Code. Connect to the master database.

1. Create a server login and assign the appropriate permissions.

2. Create a SQL Authenticated login named fabric_login. You can choose any name for this login. Provide your own strong password. Run the following T-SQL script in the master database:

  
     ```
     CREATE LOGIN fabric_login WITH PASSWORD = '<strong password>';
     ALTER SERVER ROLE [##MS_ServerStateReader##] ADD MEMBER fabric_login;
     ```

3. Connect to the Azure SQL Database your plan to mirror to Microsoft Fabric, using the Azure portal query editor, SQL Server Management Studio (SSMS), Create a database user connected to the login: 

     ```
     CREATE USER fabric_user FOR LOGIN fabric_login;
     GRANT CONTROL TO fabric_user;
     ```

# Create a mirrored Azure SQL Database

1. Open the [Fabric portal](https://app.fabric.microsoft.com/home).

2. create a new workspace named Fabric_workspace

3. Navigate to the Create pane. Select the Create icon.

4. Scroll to the Data Warehouse section and then select Mirrored Azure SQL Database. Enter the name of your Azure SQL Database to be mirrored, then select Create.


# Connect to your Azure SQL Database

1. Under New sources, select Azure SQL Database. Or, select an existing Azure SQL Database connection from the OneLake hub.


2. If you selected New connection, enter the connection details to the Azure SQL Database.

  - Server: You can find the Server name by navigating to the Azure SQL Database Overview page in the Azure portal. For example, server-name.database.windows.net.
  - Database: Enter the name of your Azure SQL Database.
  - Connection: Create new connection.
  - Connection name: An automatic name is provided. You can change it.
  - Authentication kind:
    - Basic (SQL Authentication)
  - Select Connect.


 # Start mirroring process


1.  The Configure mirroring screen allows you to mirror all data in the database, by default.

      - Mirror all data means that any new tables created after Mirroring is started will be mirrored.

      - Optionally, choose only certain objects to mirror. Disable the Mirror all data option, then select individual tables from your database.

2. Select Mirror database. Mirroring begins.

3. Wait for 2-5 minutes. Then, select Monitor replication to see the status.

4. After a few minutes, the status should change to Running, which means the tables are being synchronized.

 >Note: If you don't see the tables and the corresponding replication status, wait a few seconds and then refresh the panel.

5. When they have finished the initial copying of the tables, a date appears in the Last refresh column.


