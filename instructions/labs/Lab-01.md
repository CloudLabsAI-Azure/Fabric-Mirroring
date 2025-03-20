# Lab 01: Configure Microsoft Fabric mirrored databases from Azure SQL Database

## Lab Scenario

In this lab, you will configure mirrored databases within Microsoft Fabric using Azure SQL Database. You are tasked with setting up a high-availability solution for your organization’s critical data by enabling data replication across multiple regions. By the end of the lab, you will have mirrored your Azure SQL databases to ensure seamless data access, failover support, and disaster recovery capabilities within your Microsoft Fabric environment.

## Lab objectives
In this lab, you will complete the following tasks:

- Task 01:  Enable SAMI of your Azure SQL logical server
- Task 02: Create a mirrored Azure SQL Database
- Task 03: Initiate, Monitor, and Secure Microsoft Fabric Mirroring for Azure SQL Databases

## Estimated time: 40 minutes

### Task 01: Enable SAMI of your Azure SQL logical server

In this task, you will enable the **System assigned managed Identity (SAMI)** feature on your Azure SQL logical server to allow automatic scaling and management of your database instances within the Microsoft Fabric environment.

1. In the **Search resources, services, and docs** bar in Azure, search for **SQL servers (1)** and choose **SQL servers (2)**.
   
   ![](../media/Lab-01/sql-servers.png)

2. Select **sqlserver-<inject key="DeploymentID" enableCopy="false"/>**  

   ![](../media/Lab-01/fbdb-1.png)


3. In the resource menu, under the **Security** section, select **Identity (1)**. Change the status to **ON (2)** for System Assigned Managed Identity (SAMI), and **Save (3)** the changes.

   ![](../media/Lab-01/sami-on.png)

   >**Note**: **Please wait until SAMI is turned on; it might take some time.**

4. Under **Security**, Choose **Networking (1)** and **add a firewall rule (2)**.

   ![](../media/Lab-01/add_firewall.png)


5. Fill in the details as shown below:


   - Rule name : `Allowall` (1)

   - Start IPV4 address : `0.0.0.0` (2)

   - End IPV4 address : `255.255.255.255` (3)

   - Click on `OK` (4)

   - Now, Click on `Save` 

      ![](../media/Lab-01/add-firewall.png)
  
6. In the **Search resources, services, and docs** bar in Azure, search for **SQL database (1)** and **select it (2)**

   ![](../media/Lab-01/sqldb.png)


7. Select the **samplesqldb** database.

   ![](../media/Lab-01/sampledb-1.png)


8. From the left pane, select the **Query Editor (Preview)(1)**.

9. Log in to the SQL Server using server authentication with the provided details below, then click **OK (4)**.

   - **Username : <inject key="SQL Server Username" enableCopy="true"/>**

   - **Password : <inject key="SQL Server Password" enableCopy="true"/>**

     ![](../media/Lab-01/query-editor.png)

11. Ensure that the SAMI is set as the primary identity. Verify this by running the following T-SQL query:

   ```
   SELECT * FROM sys.dm_server_managed_identities;
   ```

   ![](../media/Lab-01/add-1.png)


11. In the Windows VM search bar, type **SSMS (1)** and select **SQL Server Management Studio 20 (2)** to open it.

      ![](../media/Lab-01/ssms.png)
 
12. In the Connect to Server pane, log in to the SQL Server using the credentials below, and click **Connect (5)** :

   - Server name : **sqlserver-<inject key="DeploymentID" enableCopy="false"/>.database.windows.net (1)**

   - Authentication : **SQL Server Authentication (2)**

   - Login : **<inject key="SQL Server Username" enableCopy="true"/> (3)**

   - Password : **<inject key="SQL Server Password" enableCopy="true"/> (4)**

     ![](../media/s1.png)

12. Click on **New Query** in the toolbar to run the query.
 
    ![](../media/Lab-01/s2.png)

13. Create a SQL-authenticated login named **fabric_login** with a strong password.
Run the T-SQL script in the **master** database by clicking **Execute**.

      > **Note:** Please use a password that you can remember and replace `<strong password>` in the script with your chosen password.

      ```sql
      CREATE LOGIN fabric_login WITH PASSWORD = '<strong password>';
      ALTER SERVER ROLE [##MS_ServerStateReader##] ADD MEMBER fabric_login;
      ```

      ![](../media/Lab-01/sql-query-1-1.png)


14. You will be able to see a **fabric_login** login account that's been created under **logins**. 

    ![](../media/Lab-01/fabric-login.png)

    >**Note :** If you are unable to see the fabric_login just refresh the pane.

    ![](../media/Lab-01/s3.png)


15. In the same query, paste the code and execute the highlighted part.  

      ```
      CREATE USER fabric_user FOR LOGIN fabric_login;
      ```

      ![](../media/Lab-01/create-user-1.png)

### Task 02: Create a mirrored Azure SQL Database
 
In this task, you will create a mirrored Azure SQL database by setting up replication between a source and target database, ensuring high availability and data redundancy within your Microsoft Fabric environment.

1. Open the ```https://app.fabric.microsoft.com```.

2. On the **Welcome to Fabric view** screen, click **Close**.

   ![](../media/Lab-01/cancel.png)

3. You will be directed to the **Fabric Home** page, where you can select **Power BI** from the bottom left.

   ![](../media/Lab-01/power-bi.png)


4. Now, select **Workspaces (1)** and click on **+ New workspace (2)** 

   ![](../media/Lab-01/workspace-1.png)

5. Fill out the **Create a workspace** form with the following details:

   - **Name:** Enter **fabric-<inject key="DeploymentID" enableCopy="false"/>**.

      ![](../media/Lab-01/workspacename.png)
   
      >**Note**: The user ID will be unique for each user, and the workspace name must also be unique. Ensure that a green check mark with **"This name is available"** appears below the Name field.

6. If you would like, you can enter a **Description** for the workspace. This is an optional field.

7. Click on **Advanced** to expand the section and Under **License mode**, select **Fabric capacity (1)**, Under **Capacity** Select available **fabric<inject key="DeploymentID" enableCopy="false"/> EAST US (2)** and click on **Apply (3)** to create and open the workspace.

    ![](../media/Lab-01/fab-1.png)

    >**Note:** If the **Introducing task flows** dialog opens, click on **Got it**.

    ![](../media/Lab-01/image28.png)


8. Navigate to the **fabric<inject key="DeploymentID" enableCopy="false"/> (1)** workspace. Select the **+ New item (2)** icon.

    ![](../media/Lab-01/add-item.png)

9. Search for **Mirrored Azure SQL Database (2)** in **Filter by item type (1)** bar and select it.


   ![](../media/Lab-01/s5-1.png)

10. Select a Azure SQL Database under **choose a database connection to get started**.

    ![](../media/Lab-01/sql-db.png)

11. Select New connection, enter the connection details to the Azure SQL Database.

   - Server : **sqlserver-<inject key="DeploymentID" enableCopy="false"/>.database.windows.net (1)**

   - Database : **samplesqldb (2)**

   - Connection: Create new connection.
   
   - Connection name: **sqlserver-<inject key="DeploymentID" enableCopy="false"/>.database.windows.net;samplesqldb (3)**

   - Authentication kind: **Basic (SQL Authentication) (4)**

   - Username : **<inject key="SQL Server Username" enableCopy="false"/> (5)**

   - Password : **<inject key="SQL Server Password" enableCopy="false"/> (6)**
   
   - Select **Connect (7)**.

     ![](../media/Lab-01/s6.png)


 12. On the **Choose Data (1)** pane, verify that all checkboxes are selected by default. Once confirmed, click on **Connect (2)**.

      > **NOTE:** Some tables might not be selected even if you try to choose them, Please ignore those tables.
 
     ![](../media/Lab-01/select-db.png)

 13. On the Destination pane, ensure **samplesqldb (1)** database is present and click on **Create mirrored database (2)**.

     ![](../media/Lab-01/sql-1-1.png)

### Task 03: Initiate, Monitor, and Secure Microsoft Fabric Mirroring for Azure SQL Databases

In this task, you will start the mirroring process for Azure SQL databases, monitor the replication status to ensure synchronization, and apply security measures to protect data during the mirroring process within Microsoft Fabric.

1. Creating your mirrored database.

   ![](../media/Lab-01/mirrored-1.png)

   >**Note**: Please wait for 2 to 5 minutes. 

2. After a few moments, the status will change to Running, indicating that the tables are being synchronized.

   ![](../media/Lab-01/sales-lt.png)

   >**Note:** Click on Refresh to see the synchronized tables


3. Navigate back to the SQL Server Management Studio (SSMS) that is already connected to the database, to run the query.

4. Right-click on your database and select **New Query**. Paste the following code and execute it by clicking the **Execute** button.

  
   ![](../media/Lab-01/s8.png)
  
   ```
   CREATE TABLE [dbo].[New-table1] (
    [Id] INT IDENTITY(1,1) PRIMARY KEY, -- Auto-increment primary key
    [Column1] NVARCHAR(100) NOT NULL,  -- Example column
    [Column2] INT NULL,                -- Example column
    [Column3] DATETIME DEFAULT GETDATE() -- Example column with default value
    );
    ```

    ![](../media/Lab-01/create-table-1.png)

5. The newly created table will appear in the list of tables in the Database Explorer.

    ![](../media/Lab-01/new-table-1.png)

    > **NOTE:** If the database name is not showing up, just refresh the pane.

    ![](../media/Lab-01/s3.png)

6. Go back to the Fabric environment, navigate to the mirrored database, and refresh the view. The newly created table should now appear.

   ![](../media/Lab-01/new-table-2.png)
     
   >**Note**: If the tables and replication status do not appear immediately, wait a few seconds and refresh the panel again.

7. After the initial table copy is complete, a date will appear in the **Last Completed** column.

## Summary

In this lab, you have accomplished the following:

- **Enabled SAMI** on your **Azure SQL logical server** for secure authentication and access to Azure resources.
- **Created a mirrored Azure SQL Database** to ensure high availability and data replication.
- **Initiated, monitored, and secured Microsoft Fabric mirroring** for Azure SQL Databases to ensure data consistency and real-time synchronization.


## Congratulations! You have successfully finished the lab. Click Next >> to Proceed to the next lab.

