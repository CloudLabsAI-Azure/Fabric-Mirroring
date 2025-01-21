
##  Enable System Assigned Managed Identity (SAMI) of Your Azure SQL Managed Instance


 1. Navigate to your SQL Managed Instance in the Azure portal.

     ![](../media/Lab-03/identity-1.png)

 2. Under **Security** in the resource menu, select **Identity**.

      ![](../media/Lab-03/identity-1.png)

 3. Under **System assigned managed identity**, set **Status** to **On**.
 
 4. Ensure that SAMI is the primary identity. Verify this by running the following T-SQL query:
     ```sql
     SELECT * FROM sys.dm_server_managed_identities;
     ```

# Create a Database Principal for Fabric

## Create a Way for the Fabric Service to Connect to Your Azure SQL Managed Instance

1. To allow Fabric to connect, create a login and mapped database user using the principle of least privilege.

1. Only grant `CONTROL DATABASE` permission in the database to be mirrored.

## Create a Login and Mapped Database User

1. Connect to your Azure SQL Managed Instance using SQL Server Management Studio (SSMS) or Azure Data Studio and connect to the master database.

1. Create a server login and assign the appropriate permissions.

1. For SQL Authenticated Login:

  - Run the following T-SQL script in the master database:

     ```sql
     CREATE LOGIN <fabric_login> WITH PASSWORD = '<strong password>';
     ALTER SERVER ROLE [##MS_ServerStateReader##] ADD MEMBER <fabric_login>;
     ```

1. Switch Query Scope to the Database You Want to Mirror

   - Substitute `<mirroring_source_database>` with the name of your database and run the following T-SQL:
     ```sql
     USE [<mirroring_source_database>];
     ```

# Create a Database User
  
1. Create a database user connected to the login:

1. For SQL Authenticated Logins:
       ```sql
       CREATE USER <fabric_user> FOR LOGIN <fabric_login>;
       ```
# Create a Mirrored Azure SQL Managed Instance Database

1. Open the Fabric Portal

2. Use an existing workspace or create a new workspace.

3. Navigate to the Create Pane.

4. Select the **Create** icon.

5. Scroll to the **Data Warehouse** section and select **Mirrored Azure SQL Managed Instance (Preview).

# Connect to Your Azure SQL Managed Instance

1. Connect to the Azure SQL Managed Instance

2. Under **New sources**, select **Azure SQL Managed Instance**, or select an existing Azure SQL Managed Instance connection from the OneLake catalog.

   >**Note**: You can't use existing connections of type "SQL Server". Only connections of type "SQL Managed Instance" are supported for mirroring Azure SQL Managed Instance data.

3. Enter Connection Details

4. If you selected **New connection**, enter the following details:

     - **Server**: Find the Server name by navigating to the **Azure SQL Managed Instance Networking** page in the Azure portal under **Security**.
       - Example: `<managed_instance_name>.public.<dns_zone>.database.windows.net,3342`
     - **Database**: Enter the name of the database you wish to mirror.
     - **Connection**: Create a new connection.
     - **Connection name**: An automatic name is provided, but you can change it for easier identification.
     - **Authentication kind**: Choose from the following:
       - Basic (SQL Authentication)
       - Organization account (Microsoft Entra ID)
       - Tenant ID (Azure Service Principal)
   - Select **Connect**.

# Start the Mirroring Process

1. Configure Mirroring

1. The **Configure mirroring** screen will allow you to mirror all data in the database by default.

1. **Mirror all data**: Any new tables created after mirroring starts will also be mirrored.
    - Optionally, you can disable **Mirror all data** and select individual tables to mirror.
    - Tables that can't be mirrored will show an error icon, while tables with limitations will show a warning icon with an explanation.
    - For this tutorial, select **Mirror all data**.

1. **Create the Mirrored Database**
    - On the next screen, provide a name for the destination item and select **Create mirrored database**.
    - Wait 1-2 minutes for Fabric to provision everything.

1. **Monitor Replication**

1. After 2-5 minutes, select **Monitor replication** to see the replication status.

1. The status should change to **Running**, which means the tables are being synchronized.

1. If you don't see the tables and corresponding replication status, wait a few seconds and refresh the pane.

1. When the initial copying of the tables is finished, a date will appear in the **Last refresh** column.

    - **Important**: Any granular security settings in the source database must be re-configured in the mirrored database in Microsoft Fabric.

# Monitor Fabric Mirroring

1. Once mirroring is configured, you'll be directed to the **Mirroring Status** page, where you can monitor the current state of replication.

1. Replicating Status:
   
      - **Running** – Replication is currently running, bringing snapshot and change data into OneLake.
      - **Running with warning** – Replication is running with transient errors.
      - **Stopping/Stopped** – Replication is stopped.
      - **Error** – Fatal error in replication that can't be recovered.


 1. Table Level Monitoring:

      - **Running** – Data from the table is successfully being replicated into the warehouse.
      - **Running with warning** – Warning of non-fatal error with replication of the data from the table.
      - **Stopping/Stopped** – Replication has stopped.
      - **Error** – Fatal error in replication for that table.
