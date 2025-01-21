## Task01: Create an Open mirrored database 
1. Navigate to the **Fabric portal** home.

    ![](../media/Lab-01/image10.png)

2. Open an existing workspace **fabric-<inject key="DeploymentID" enableCopy="false"/>**

3. In the navigation menu, select **+New Item**.

    ![](../media/Lab-01/fabric-new.png)

1. Locate and select the **Mirrored Database** card.

    ![](../media/Lab-05/mirrored-database-1.png)

1. Enter a name for the new mirrored database as **Mirrored Database** and Select **Create**.

      ![](../media/Lab-05/mirrored-1.png)

1. Locate the **Landing zone URL** in the details section of the mirrored database home page.

     ![](../media/Lab-05/landing-zone-1.png)

## Task-02  Write Change Data into the Landing Zone(Read-Only)

1. Write Data into the Landing Zone.

1. Your application can now write both initial load and incremental change data into the landing zone.

1. Connecting to Microsoft OneLake to authorize and write to the mirrored database landing zone in OneLake.

1. Review the **Open mirroring landing zone requirements** and format specifications.

## Task -03: Start and Monitor the Replication Process

1. The **Configure mirroring** screen allows you to mirror all data in the database by default.

1. **Mirror all data** means that any new tables created after mirroring starts will also be mirrored.
    
1. Select **Mirror database**. The mirroring process begins.

    >Note: Wait for 2-5 minutes.Select **Monitor replication** to see the status.

 1. After a few minutes, the status should change to **Running**, which indicates that the tables are being synchronized.

 1. If you don't see the tables and the corresponding replication status, wait a few seconds and then refresh the panel.

1. Monitor Replication Status

2. Once mirroring is configured, you will be directed to the **Mirroring Status** page.


