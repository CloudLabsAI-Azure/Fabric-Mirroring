# Create a Mirrored Database

In this section, we'll provide a brief overview of how to create a new mirrored database to use with your mirrored Snowflake data source.

1. **Select or Create a Workspace**

1. You can use an existing workspace 

1. From your workspace, navigate to the **Create hub**
.
1. After selecting the workspace you would like to use, click **Create**.


1. Scroll down and select the **Mirrored Snowflake** card.

1. Enter a name for the new database.

1. Click **Create**.

# Connect to Your Snowflake Instance in Any Cloud

1. Set up the Connection

   >**Note**: You might need to alter the firewall cloud to allow Mirroring to connect to the Snowflake instance.

1. Select **Snowflake** under **New connection** or select an existing connection if available.

4. **Configure Connection Settings**
   If you selected **New connection**, enter the following connection details:

   | **Connection Setting** | **Description** |
   |------------------------|-----------------|
   | **Server**             | You can find your server name by navigating to the accounts section in the Snowflake resource menu. Hover over the account name and copy the server name. Remove the `https://` prefix from the server name. |
   | **Warehouse**          | From the **Warehouses** section in the resource menu in Snowflake, select the **Snowflake Warehouse** (Compute) and not the database. |
   | **Connection**         | Create a new connection. |
   | **Connection name**    | This should be automatically filled out, but you can change it to a name that you would like to use. |
   | **Authentication kind** | Snowflake |
   | **Username**           | Your Snowflake username that you created to sign into Snowflake.com. |
   | **Password**           | Your Snowflake password that you created when you created your login information for Snowflake.com. |
   | **Database**           | Select your database from the dropdown list. |

5. Start the Mirroring Process
   
1. The **Configure mirroring** screen will allow you to mirror all data in the database by default.

2. Mirror all data** means that any new tables created after mirroring is started will be mirrored.

3. Optionally, you can disable the **Mirror all data** option and choose only certain objects to mirror by selecting individual tables from your database.

4. For this tutorial, we select the **Mirror all data** option.

5. Begin Mirroring

6. Click **Mirror database**. The mirroring process will begin.

