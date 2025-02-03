# Lab 02: Configure Microsoft Fabric Mirrored Database from Azure Cosmos DB

## Lab Scenario

In this lab, you will configure mirrored databases within Microsoft Fabric using Azure Cosmos DB. Your goal is to set up a multi-region, high-availability solution by replicating data across multiple Azure Cosmos DB instances. This will ensure low-latency access to data and provide disaster recovery capabilities. By the end of this lab, you will have successfully mirrored your Azure Cosmos DB databases in Microsoft Fabric, 
allowing for seamless data synchronization and improved availability.

## Lab objectives
In this lab, you will complete the following tasks:

- Task 01: Verify the Configuration of the Source Azure Cosmos DB Account
- Task 02: Set Up a Mirrored Database
- Task 03: Connect to the Source Database
- Task 04: Start the Mirroring Process and Monitor Fabric Mirroring
- Task 05: Query the Source Database from Fabric
- Task 06: Analyze the Target Mirrored Database

## Estimated time: 40 minutes

### Task 01: Verify the Configuration of the Source Azure Cosmos DB Account

In this task, you will confirm that the source Azure Cosmos DB account is correctly configured and ready for data mirroring in Microsoft Fabric.

1. In the Azure portal, type **"Azure Cosmos DB"** in the search bar at the top of the page, and select the **Azure Cosmos DB account** from the search results.

   ![](../media/Lab-02/azure-cosmosdb.png)

2. Select **cosmosdb-<inject key="DeploymentID" enableCopy="false"/>**.

3. From the left-hand panel, click on **Data Explorer (1)** and then select **Launch Quick Start (2)**.

   ![](../media/Lab-02/launch-quick.png)

4. Enter the Database ID as **OrderDB** and the Container ID as **Orderitems**. Leave all other settings at their default values, and click **OK** to proceed. Once the container is created, you will see the tables that were created.

   ![](../media/Lab-02/ordersdb-new.png)

5. Next, click on **Launch Quick Start** again and create another container with the ID **Orderstatus**. Select **Use Existing** and choose **OrderDB** from the dropdown menu. Then, click **OK**.

   ![](../media/Lab-02/orderdb-exist.png)

6. Ensure that the networking options in the **Networking** tab are set to **Public network access for all networks**.

7. In the left panel, select **Identity (1)**, enable the system-assigned status by switching it to **On (2)**, and then click **Save (3)**. When prompted, click **Yes**.

   ![](../media/Lab-02/democosmos.png)

8. Select **Keys** from the left-hand pane, then copy the **endpoint URL** and **primary key** and paste them in a notepad for use in the further steps.

      ![](../media/Lab-02/s10.png)

9. Now select **Backup & Restore (1)** from the left-hand pane.

     - Click **Change (2)** to modify the Backup policy mode.

     - Choose **Continuous (30 days) (3)**.

     - Click on **Save (4)**

        ![](../media/Lab-02/s11.png)

### Task 02: Set Up a Mirrored Database

In this task, you will configure a mirrored database in Microsoft Fabric, ensuring data replication from the source Azure Cosmos DB to a secondary instance for high availability and disaster recovery.

1. Go to the **Fabric portal** home page of powerbi.

   ![](../media/Lab-01/power-bi.png)

2. Open the existing workspace **fabric-<inject key="DeploymentID" enableCopy="false"/>**.

3. In the navigation menu, click on **+ New Item**.

   ![](../media/Lab-01/fabric-new.png)

4. From the options, choose **Mirrored Azure Cosmos DB (Preview)**.

   ![](../media/Lab-02/mirrored-1.png)


### Task 03: Connect to the Source Database

In this task, you will establish a connection to the source Azure Cosmos DB database, allowing you to manage and verify data replication settings within Microsoft Fabric.

1. In the **New Connection** section, choose **Azure Cosmos DB v2**.

   ![](../media/Lab-02/select-cosmos.png)

2. Enter the credentials for your Azure Cosmos DB for NoSQL account, including the following details:

     - **Azure Cosmos DB endpoint**: Paste the endpoint URL that you save in the notepad earlier.

     - **Connection name**: A unique name for this connection.

     - **Authentication kind**: Select
     **Account key**.

     - **Account Key**: Paste the account key that you saved in the notepad earlier.
     
     - Click **Connect**. 

       ![](../media/Lab-02/cosmos-db.png)  

3. Under the **Choose Data** section, click **Connect**.

    ![](../media/Lab-02/orderdb.png)
  
5. In the **Destination** section, enter the name **Mirrored-SampleDB** and click **Create mirrored database**.

     ![](../media/Lab-02/mirrored-db-1.png)

      > **Note**: All containers within the selected database will be mirrored.
    

### Task 04: Start the Mirroring Process and Monitor Fabric Mirroring

In this task, you will initiate the mirroring process between the source and mirrored databases, then monitor the replication progress and status within Microsoft Fabric to ensure data synchronization is successful.

1. Select **Monitor Replication**. Mirroring will now begin.

   ![](../media/Lab-02/monitor-replication.png)

   >**Note**: Wait for 2 to 5 minutes, then select **Monitor replication** to see the status of the replication action.

2. After a few minutes, the status should change to **Running**, indicating that the containers are being synchronized.

   ![](../media/Lab-02/mointor-replication.png)


   - **Tip**: If you can't find the containers and the corresponding replication status, refresh the pane after a few seconds. In rare cases, transient error messages may appear; you can safely ignore them and refresh.

     >**Note**: When the mirroring finishes the initial copying of the containers, a date will appear in the **Last Refresh** column. If data was successfully replicated, the **Total Rows** column will show the number of items replicated.


### Task 05: Query the Source Database from Fabric

In this task, you will execute queries on the source database through Microsoft Fabric to verify data accessibility and ensure the connection is properly established for mirroring.

1. Go to the mirrored database in the Fabric portal.

    ![](../media/Lab-02/mirrored-db02.png)

2. Click on **View**, then select **Source Database**. This will open the Azure Cosmos DB data explorer in a read-only mode for the source database.

    ![](../media/Lab-02/source-explorer-query.png)

  > **Note**: All read operations on the source database are routed to Azure and will consume Request Units (RUs) allocated to the account.


### Task 06: Analyze the Target Mirrored Database
 
In this task, you will examine the target mirrored database in Microsoft Fabric to ensure that data replication is functioning correctly and that the mirrored database is in sync with the source.

1. Switch from **Mirrored Azure Cosmos DB** to **SQL Analytics Endpoint**.

     ![](../media/Lab-02/sql-endpoint.png)

2. Each container from the source database will appear as a warehouse table in the SQL Analytics Endpoint.

3. Select **Orderstatus**, open the context menu, and click on **New SQL Query**, then choose **Select Top 100**. The query will run and return the top 100 records from the selected table.

4. Next, select **Orderitems**, click **New SQL Query**, and choose **Select Top 100**.

   - Run the following sample query:

        ```sql
         SELECT TOP (100)[_rid],
                           [id],
                   [categoryId],
                 [categoryName],
                          [sku],
                         [name],
                  [description],
                        [price],
                         [tags],
                          [_ts]
         FROM [OrderDB].[OrderDB].[Orderitems]
        ```

     ![](../media/Lab-02/results-1.png)

5. Select **New Visual Query** to open the query editor from the tool bar.    

      ![](../media/Lab-02/new-visual-query.png)

6. Drag and drop both the **Orderstatus** and **Orderitems** tables into the query editor.

     ![](../media/Lab-02/both-orders.png)

7. Click the **+** icon in the first query and choose **Merge query as new**.

     ![](../media/Lab-02/merge-as-view.png)

8. For the merge, select **Orderitems** from the right table and choose **id**, then click **OK**.

      ![](../media/Lab-02/merge-query-011.png)

9. Once merged, your visual query will appear as follows:

     ![](../media/Lab-02/order-final.png)

10. On the query editor pane , select **save as view**.

     ![](../media/Lab-02/save-as-view.png)

11. In the **Save As** window, select **OrderDB (1)** as the schema and name the view as **Merged_orders (2)**,then click **OK (3)**.

     ![](../media/Lab-02/save-as-1.png)

10. From the toolbar, go to the **Reporting** tab and click on **New Report**.

     ![](../media/Lab-02/new-report-1.png)

11. When the pop-up appears displaying all available data, click **Continue**.

      ![](../media/Lab-02/new-report.png)

12. When the pop-up appears, select **Try Free** to upgrade to a paid Power BI license.

      ![](../media/Lab-02/powerbi.png)

13. Click on **Got it**.

      ![](../media/Lab-02/powerbi0.png)

12. Expand the **Data Pane** and select the **Sum of _ts**, **categoryid**, and **Sum of price**.

     ![](../media/Lab-02/data.png)

13. In the **Visualization Pane**, select the **Clustered Column Chart**.

     ![](../media/Lab-02/choosevis.png)

14. Finally, the generated report for **OrderDB** will be displayed. Save the report as **Orders-reports**.

    ![](../media/Lab-02/final-report.png)

    ![](../media/Lab-02/saveas.png)

 # Summary:

In this lab, you have accomplished the following:

- **Verified the configuration** of the **Source Azure Cosmos DB Account** to ensure it is set up correctly.
- **Set up a mirrored database** for replication to enhance data availability.
- **Connected to the source database** to enable data synchronization and mirroring.
- **Started the mirroring process** and **monitored Fabric mirroring** to ensure successful data replication.
- **Queried the source database** from Fabric to verify data access and synchronization.
- **Analyzed the target mirrored database** to confirm data consistency and integrity across environments.

### Congratulations! You have successfully finished the lab. Click Next >> to Proceed to the next lab.
