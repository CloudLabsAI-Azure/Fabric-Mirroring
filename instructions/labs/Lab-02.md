# Configure Your Azure Cosmos DB Account

## Ensure the Source Azure Cosmos DB Account is Correctly Configured

1. Navigate to your Azure Cosmos DB account in the Azure portal.

2. Ensure that continuous backup is enabled. 

If not enabled, follow the guide at [Migrate an Existing Azure Cosmos DB Account to Continuous Backup](https://learn.microsoft.com/en-us/azure/cosmos-db/migrate-to-continuous-backup) to enable continuous backup. Note that this feature might not be available in all scenarios. For more information, see [Database and Account Limitations](https://learn.microsoft.com/en-us/azure/cosmos-db/database-account-limitations).

## Configure Networking Options

1. Ensure that the networking options are set to **Public network access for all networks**.
2. If not configured, follow the guide at [Configure Network Access to an Azure Cosmos DB Account](https://learn.microsoft.com/en-us/azure/cosmos-db/network-access).

## Create a Mirrored Database

1. Navigate to the **Fabric portal** home.
2. Open an existing workspace or create a new workspace.
3. In the navigation menu, select **Create**.
4. Under the **Data Warehouse** section, select **Mirrored Azure Cosmos DB (Preview)**.
5. Provide a name for the mirrored database and select **Create**.

## Connect to the Source Database

1. In the **New Connection** section, select **Azure Cosmos DB for NoSQL**.
2. Provide credentials for the Azure Cosmos DB for NoSQL account including:
     - **Azure Cosmos DB endpoint**: URL endpoint for the source account.
     - **Connection name**: Unique name for the connection.
     - **Authentication kind**: Select **Account key**.
     - **Account Key**: Read-write key for the source account.
   - Select **Connect**. Then, select a database to mirror.
   - **Note**: All containers in the database will be mirrored.

# Start the Mirroring Process

5. **Begin Mirroring**
   - Select **Mirror database**. Mirroring will now begin.
   - Wait for 2 to 5 minutes, then select **Monitor replication** to see the status of the replication action.
   - After a few minutes, the status should change to **Running**, indicating that the containers are being synchronized.
   - **Tip**: If you can't find the containers and the corresponding replication status, refresh the pane after a few seconds. In rare cases, transient error messages may appear; you can safely ignore them and refresh.
   - When the mirroring finishes the initial copying of the containers, a date will appear in the **Last Refresh** column. If data was successfully replicated, the **Total Rows** column will show the number of items replicated.

# Monitor Fabric Mirroring

6. **Monitor the Current State of Replication**
   - Once Fabric Mirroring is configured, you're automatically navigated to the **Replication Status** pane.
   - Here, monitor the current state of replication. For more details on the replication states, see [Monitor Fabric Mirrored Database Replication](https://learn.microsoft.com/en-us/azure/fabric/monitor-replication).

# Query the Source Database from Fabric

7. **Query the Source Database**
   - Navigate to the mirrored database in the Fabric portal.
   - Select **View**, then **Source database**. This action opens the Azure Cosmos DB data explorer with a read-only view of the source database.
   - Select a container, then open the context menu and select **New SQL query**.
   - Run any query. For example, use:
     ```sql
     SELECT COUNT(1) FROM container
     ```
     to count the number of items in the container.
   - **Note**: All the reads on the source database are routed to Azure and will consume Request Units (RUs) allocated on the account.

# Analyze the Target Mirrored Database

1. **Query NoSQL Data Stored in Fabric OneLake**
   - Navigate to the mirrored database in the Fabric portal.
   - Switch from **Mirrored Azure Cosmos DB** to **SQL Analytics endpoint**.
   - Each container in the source database should be represented in the SQL analytics endpoint as a warehouse table.
   - Select any table, open the context menu, then select **New SQL Query**, and select **Select Top 100**.
   - The query will execute and return 100 records in the selected table.
   - Open the context menu for the same table, select **New SQL Query**, and write an example query that uses aggregates like **SUM**, **COUNT**, **MIN**, or **MAX**. You can also join multiple tables to execute the query across multiple containers.
   
   Example SQL query:
   ```sql
   SELECT
       d.[product_category_name],
       t.[order_status],
       c.[customer_country],
       s.[seller_state],
       p.[payment_type],
       SUM(o.[price]) AS price,
       SUM(o.[freight_value]) AS freight_value
   FROM
       [dbo].[products] p
   INNER JOIN
       [dbo].[OrdersDB_order_payments] o ON o.[order_id] = p.[order_id]
   INNER JOIN
       [dbo].[OrdersDB_order_status] t ON o.[order_id] = t.[order_id]
   INNER JOIN
       [dbo].[OrdersDB_customers] c ON t.[customer_id] = c.[customer_id]
   INNER JOIN
       [dbo].[OrdersDB_productdirectory] d ON o.product_id = d.product_id
   INNER JOIN
       [dbo].[OrdersDB_sellers] s ON o.seller_id = s.seller_id
   GROUP BY
       d.[product_category_name],
       t.[order_status],
       c.[customer_country],
       s.[seller_state],
       p.[payment_type]
   ```

1. Select the query and then select Save as view. Give the view a unique name. You can access this view at any time from the Fabric portal.

1. Return back to the mirrored database in the Fabric portal.

1. Select New visual query. Use the query editor to build complex queries.

1. Screenshot of the query editor for both text-based and visual queries in Fabric.


Build BI reports on the SQL queries or views
Select the query or view and then select Explore this data (preview). This action explores the query in Power BI directly using Direct Lake on OneLake mirrored data.
Edit the charts as needed and save the report.
