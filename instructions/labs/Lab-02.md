# Configure your Azure Cosmos DB account

First, ensure that the source Azure Cosmos DB account is correctly configured to use with Fabric mirroring.

Navigate to your Azure Cosmos DB account in the Azure portal.

Ensure that continuous backup is enabled. If not enabled, follow the guide at migrate an existing Azure Cosmos DB account to continuous backup to enable continuous backup. This feature might not be available in some scenarios. For more information, see database and account limitations.

Ensure that the networking options are set to public network access for all networks. If not, follow the guide at configure network access to an Azure Cosmos DB account.

# Create a mirrored database

Now, create a mirrored database that is the target of the replicated data. For more information, see What to expect from mirroring.

Navigate to the Fabric portal home.

Open an existing workspace or create a new workspace.

In the navigation menu, select Create.

Select Create, locate the Data Warehouse section, and then select Mirrored Azure Cosmos DB (Preview).

Provide a name for the mirrored database and then select Create.

# Connect to the source database
Next, connect the source database to the mirrored database.

In the New connection section, select Azure Cosmos DB for NoSQL.

Provide credentials for the Azure Cosmos DB for NoSQL account including these items:

Value
Azure Cosmos DB endpoint	URL endpoint for the source account.
Connection name	Unique name for the connection.
Authentication kind	Select Account key.
Account Key	Read-write key for the source account.
Screenshot of the new connection dialog with credentials for an Azure Cosmos DB for NoSQL account.

Select Connect. Then, select a database to mirror.

 Note

All containers in the database are mirrored.

Start mirroring process
Select Mirror database. Mirroring now begins.

Wait two to five minutes. Then, select Monitor replication to see the status of the replication action.

After a few minutes, the status should change to Running, which indicates that the containers are being synchronized.

 Tip

If you can't find the containers and the corresponding replication status, wait a few seconds and then refresh the pane. In rare cases, you might receive transient error messages. You can safely ignore them and continue to refresh.

When mirroring finishes the initial copying of the containers, a date appears in the last refresh column. If data was successfully replicated, the total rows column would contain the number of items replicated.

Monitor Fabric Mirroring
Now that your data is up and running, there are various analytics scenarios available across all of Fabric.

Once Fabric Mirroring is configured, you're automatically navigated to the Replication Status pane.

Here, monitor the current state of replication. For more information and details on the replication states, see Monitor Fabric mirrored database replication.

Query the source database from Fabric
Use the Fabric portal to explore the data that already exists in your Azure Cosmos DB account, querying your source Cosmos DB database.

Navigate to the mirrored database in the Fabric portal.

Select View, then Source database. This action opens the Azure Cosmos DB data explorer with a read-only view of the source database.

Screenshot of the data explorer with a read-only view of NoSQL data in the Azure Cosmos DB account.

Select a container, then open the context menu and select New SQL query.

Run any query. For example, use SELECT COUNT(1) FROM container to count the number of items in the container.

 Note

All the reads on source database are routed to Azure and will consume Request Units (RUs) allocated on the account.

Analyze the target mirrored database
Now, use T-SQL to query your NoSQL data that is now stored in Fabric OneLake.

Navigate to the mirrored database in the Fabric portal.

Switch from Mirrored Azure Cosmos DB to SQL analytics endpoint.

Screenshot of the selector to switch between items in the Fabric portal.

Each container in the source database should be represented in the SQL analytics endpoint as a warehouse table.

Select any table, open the context menu, then select New SQL Query, and finally select Select Top 100.

The query executes and returns 100 records in the selected table.

Open the context menu for the same table and select New SQL Query. Write an example query that use aggregates like SUM, COUNT, MIN, or MAX. Join multiple tables in the warehouse to execute the query across multiple containers.

 Note

For example, this query would execute across multiple containers:

SQL

Copy
SELECT
    d.[product_category_name],
    t.[order_status],
    c.[customer_country],
    s.[seller_state],
    p.[payment_type],
    sum(o.[price]) as price,
    sum(o.[freight_value]) freight_value 
FROM
    [dbo].[products] p 
INNER JOIN
    [dbo].[OrdersDB_order_payments] p 
        on o.[order_id] = p.[order_id] 
INNER JOIN
    [dbo].[OrdersDB_order_status] t 
        ON o.[order_id] = t.[order_id] 
INNER JOIN
    [dbo].[OrdersDB_customers] c 
        on t.[customer_id] = c.[customer_id] 
INNER JOIN
    [dbo].[OrdersDB_productdirectory] d 
        ON o.product_id = d.product_id 
INNER JOIN
    [dbo].[OrdersDB_sellers] s 
        on o.seller_id = s.seller_id 
GROUP BY
    d.[product_category_name],
    t.[order_status],
    c.[customer_country],
    s.[seller_state],
    p.[payment_type]
This example assumes the name of your table and columns. Use your own table and columns when writing your SQL query.

Select the query and then select Save as view. Give the view a unique name. You can access this view at any time from the Fabric portal.

Return back to the mirrored database in the Fabric portal.

Select New visual query. Use the query editor to build complex queries.

Screenshot of the query editor for both text-based and visual queries in Fabric.


Build BI reports on the SQL queries or views
Select the query or view and then select Explore this data (preview). This action explores the query in Power BI directly using Direct Lake on OneLake mirrored data.
Edit the charts as needed and save the report.
