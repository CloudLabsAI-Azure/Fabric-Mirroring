# Configure Microsoft Fabric mirrored databases from Azure SQL Database 

## Enable SAMI of your Azure SQL logical server

1. In the azure portal , search for the SQL server
   
   ![](../media/Lab-01/sql-servers.png)

1. Select the SQL servers. 

   ![](../media/Lab-01/server-1.png)

1. To enable or verify the System Assigned Managed Identity (SAMI), navigate to your logical SQL Server in the Azure portal. Under the Security section in the resource menu, select Identity, and then set the Status to "On."

   ![](../media/Lab-01/sqldbserver01.png)

3. Go back to your query-editor, on query editor preview, login to sql server with server autentication 

   - Username : `adminserver`

   - Password : `Root@12345`

   ![](../media/Lab-01/query-editor.png)

2. The SAMI must be the primary identity. Verify the SAMI is the primary identity with the following T-SQL query: 

   ```
   SELECT * FROM sys.dm_server_managed_identities;
   ```

   ![](../media/Lab-01/query-editor-1-1.png)

    ![](../media/Lab-01/results-1.png)



## Connect to your Azure SQL logical server using SQL Server Management Studio (SSMS) or the mssql extension with Visual Studio Code. Connect to the master database.

1. Login to sql server , add the below creds :

   - Server name : 

   - Authentication : 

   - Login : 

   - Passoword : 

    ![](../media/Lab-01/sql-login.png)

1. Create a server login and assign the appropriate permissions.


1. Create a SQL Authenticated login named fabric_login. You can choose any name for this login. Provide your own strong password. Run the following T-SQL script in the master database:

  
     ```
     CREATE LOGIN fabric_login WITH PASSWORD = '<strong password>';
     ALTER SERVER ROLE [##MS_ServerStateReader##] ADD MEMBER fabric_login;
     ```

   ![](../media/Lab-01/sql-query-1.png)

1. Connect to the Azure SQL Database your plan to mirror to Microsoft Fabric, using the Azure portal query editor, SQL Server Management Studio (SSMS), Create a database user connected to the login: 

     ```
     CREATE USER fabric_user FOR LOGIN fabric_login;
     GRANT CONTROL TO fabric_user;
     ```

# Create a mirrored Azure SQL Database

1. Open the [Fabric portal](https://app.fabric.microsoft.com/home), You will be navigated to the **Fabric Home**.

    ![](../media/Lab-01/image10.png)

    To work with Fabric items, you will need a trial license and a workspace that has Fabric license. Let’s set this up.

1. On the top right corner of the screen, select the **user** **icon**.

1. Select **Free Trial**.

    ![](../media/Lab-01/image11.png)

1. Upgrade to a free Microsoft Fabric trial dialog opens. Select **Activate**.

    ![](../media/Lab-01/image12.png)

1. The "Successfully Upgraded to Microsoft Fabric" dialog will appear. Click on **Fabric Home Page**..  

   ![](../media/Lab-01/fabrichome_1.png)

1. You will be navigated back to the **Microsoft** **Fabric Home page**.

    ![](../media/Lab-01/image10.png)

1. Now let’s create a workspace with Fabric license. 

1.  Now, select **Workspaces** and click on **+ New workspace**:

     ![](../media/Lab-01/workspace.png)

2. Fill out the **Create a workspace** form with the following details:

   - **Name:** Enter **fabric-<inject key="DeploymentID" enableCopy="false"/>**.

      ![](../media/Lab-01/workspacename.png)
   
   >**Note**: The user ID will be unique for each user, and the workspace name must also be unique. Ensure that a green check mark with **"This name is available"** appears below the Name field.

1. If you would like, you can enter a **Description** for the workspace. This is an optional field.

1. Click on **Advanced** to expand the section.

    ![](../media/lab-01/RTI_username.png)

1. Under **License mode**, make sure **Trial** is selected. (It should be selected by default.)

    ![](../media/lab-01/imag017-1.png)

1. Select **Apply** to create a new workspace.

    ![](../media/lab-01/imag017-1.png)

    >**Note:** If the **Introducing task flows** dialog opens, click on **Got it**.

    ![](../media/lab-01/image28.png)

1. Navigate to the Create pane. Select the Create icon.

1. Scroll to the Data Warehouse section and then select Mirrored Azure SQL Database. Enter the name of your Azure SQL Database to be mirrored, then select Create.


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


