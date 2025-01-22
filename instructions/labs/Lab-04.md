# Lab-04 Mirroring Snowflake in Microsoft Fabric

In this lab, you’ll create a mirrored database in Microsoft Fabric and connect to your Snowflake instance on any cloud platform. You’ll start the mirroring process to replicate the Snowflake database into Fabric. Finally, you’ll monitor and view the status of the Fabric Snowflake mirroring process to ensure successful replication and synchronization.

## Task-01: Create a mirrored database in Microsoft Fabric 

In this section, we'll provide a brief overview of how to create a new mirrored database to use with your mirrored Snowflake data source.

1. Navigate to the **Fabric portal** home.

    ![](../media/Lab-01/image10.png)

2. Open an existing workspace **fabric-<inject key="DeploymentID" enableCopy="false"/>**

3. In the navigation menu, select **+ New Item**.

   ![](../media/Lab-01/fabric-new.png)

4. **Scroll down** and select the **Mirrored Snowflake (preview)** card.

     ![](../media/Lab-04/mirrored-db.png)

1. Enter a name as **Mirrored_<inject key="DeploymentID" enableCopy="false"/>** and Click on create **Create**

     ![](../media/Lab-04/md-1.png)


## Task-02 Connect to your Snowflake instance in any cloud 

1. Navigate back to your workspaace and create a new item.

1. Select **Mirrored Snowflake** under get data.

   ![](../media/Lab-04/s14.png)

1. Select **Snowflake** under **Choose a database connection**. 

2. **Configure Connection Settings**
   If you selected **New connection**, enter the following connection details and click on **Connect (6)**

   | Connection Setting | Description |
   |------------------------|-----------------|
   | **Server (1)**             | dlhdzca-bab11165.snowflakecomputing.com|
   | **Warehouse (2)**          | FABRICMIRRORING |
   | **Connection**         | Create new connection |
   | **Connection name**    | Default |
   | **Authentication kind (3)** | Snowflake |
   | **Username (4)**           | <inject key="SnowFlake Username" enableCopy="false"/> |
   | **Password (5)**           | <inject key="SnowFlake Password" enableCopy="false"/> |
   | **Database**           | FABRIC_MIRRORING|

      ![](../media/Lab-04/s16.png)

1. Select **FABRIC_MIRRORING** database from dropdown list and click on **Connect**.

      ![](../media/Lab-04/fabric-mirroring.png)

1. View the tablets that's selected and then click on **Connect**.

      ![](../media/Lab-04/choose-data.png)
   
1. The **Configure mirroring** screen will allow you to mirror all data in the database by default.
     
     ![](../media/Lab-04/choose-data.png)

1. In Destination pane ensure **FABRIC_MIRRORING** is selected and click on **Create mirrored database**.
      
## Task-03 Start mirroring process for Snowflake database 

1. Click Mirror database to start the mirroring process.

1. After configuring mirroring, you'll be redirected to the Mirroring Status page, where you can monitor the replication status.

   > **Note:** Allow 2-5 minutes, then click Monitor replication to check the status.

1. Within a few minutes, the status should update to Running, indicating that the tables are being synchronized.

1. If the tables and replication status are not immediately visible, wait a few seconds and refresh the panel.

1. Once the initial table copying is complete, a date will appear in the Last refresh column.

## Task 4 : Monitor & View of the Fabric Snowflake Mirroring 

1. Observe the replication process and view the active Fabric operations.

   ![](../media/Lab-04/public-demo.png)

1. Table Level Monitoring:

      - **Running** – Data from the table is successfully being replicated into the warehouse.
      - **Running with warning** – Warning of non-fatal error with replication of the data from the table.
      - **Stopping/Stopped** – Replication has stopped.
      - **Error** – Fatal error in replication for that table.

   ## Review
   
   In this lab, you have completed creating a mirrored database in Microsoft Fabric and connected it to your Snowflake instance. You successfully initiated the mirroring process to replicate the Snowflake database into Fabric. Additionally, you monitored and reviewed the status of the Fabric Snowflake mirroring process. This ensured that the database replication and synchronization were performed correctly.
