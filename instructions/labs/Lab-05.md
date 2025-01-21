# Create a Mirrored Database

In this section, we provide a brief overview of how to create a new open mirrored database in the Fabric portal. Alternatively, you could use the **Create mirrored database REST API** together with the JSON definition example of open mirroring for creation.

1. Navigate to the **Fabric portal** home.

    ![](../media/Lab-01/image10.png)

2. Open an existing workspace **fabric-<inject key="DeploymentID" enableCopy="false"/>**

3. In the navigation menu, select **+New Item**.

    ![](../media/Lab-01/fabric-new.png)

1. Locate and select the **Mirrored Database** card.

    ![](../media/Lab-01/fabric-new.png)

1. Enter a name for the new mirrored database as Mirrored Database

1.Select **Create**.

1. Review the New Mirrored Database

1. Once an open mirrored database is created via the user interface, the mirroring process is ready.

1.Review the **Home page** for the new mirrored database item.

1. Locate the **Landing zone URL** in the details section of the mirrored database home page.

**Screenshot**: The Landing zone URL location can be found in the Home page of the mirrored database item.

# Write Change Data into the Landing Zone

1. Write Data into the Landing Zone.

1. Your application can now write both initial load and incremental change data into the landing zone.

1. Follow the steps in **Connecting to Microsoft OneLake** to authorize and write to the mirrored database landing zone in OneLake.

1. Review the **Open mirroring landing zone requirements** and format specifications.

# Start the Mirroring Process

1. Configure Mirroring

1. The **Configure mirroring** screen allows you to mirror all data in the database by default.

1. **Mirror all data** means that any new tables created after mirroring starts will also be mirrored.

1. Optionally, you can disable **Mirror all data** and select individual tables from your database.

   For this tutorial, we select the **Mirror all data** option.

## Begin the Mirroring Process
    
1. Select **Mirror database**. The mirroring process begins.

## Monitor the Replication Status

 1. Wait for 2-5 minutes.

 1. Select **Monitor replication** to see the status.

 1. After a few minutes, the status should change to **Running**, which indicates that the tables are being synchronized.

 1. If you don't see the tables and the corresponding replication status, wait a few seconds and then refresh the panel.

  ## Finish Initial Copying of Tables

1. When the initial copying of the tables is finished, a date will appear in the **Last refresh** column.

1. Once your data is up and running, there are various analytics scenarios available across all of Fabric.

# Monitor Fabric Mirroring

1. Monitor Replication Status

2. Once mirroring is configured, you will be directed to the **Mirroring Status** page.

3. Here, you can monitor the current state of replication.
