# Lab 05: Configure Microsoft Fabric open mirrored databases

In this lab, you will learn how to configure Microsoft Fabric open mirrored databases. You will start by creating an open mirrored database within the Fabric environment. Then, you'll write change data into the landing zone to ensure synchronization. After that, you will initiate the mirroring process to enable data replication. Finally, you’ll monitor the mirroring process to ensure data consistency and resolve any issues that arise during synchronization.

## Task 01: Create an Open mirrored database 

1. Navigate to the **Fabric portal** home.

    ![](../media/Lab-01/power-bi.png)

2. Open an existing workspace **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)**

3. In the navigation menu, select **+ New Item (2)**.

    ![](../media/Lab-01/fabric-new.png)

1. Locate and select the **Mirrored Database (preview)** card.

    ![](../media/Lab-05/mirrored-database-1.png)

1. Enter a name for the new mirrored database as **Mirrored Database_<inject key="DeploymentID" enableCopy="false"/>** and Select **Create**.

      ![](../media/Lab-05/mirrored-1.png)

1. Locate the **Landing zone URL** in the details section of the mirrored database home page and copy it 

     ![](../media/Lab-05/landing-zone-1.png)

## Task 02: Write Change Data to the Landing Zone and Monitor the Replication Process.

1. In the Windows search bar, type **Azure Storage Explorer** and open the application.

   ![](../media/Lab-05/storage-explorer.png)

2. Click on **Sign-in with azure** .

   ![](../media/Lab-05/sign-in.png)

3. In the **Which Azure environment would you like to sign in to?** window, select **Azure** and click on next

    ![](../media/Lab-05/azure-signin.png)


4. On the **sign-in** page, select the account **<inject key="AzureAdUserEmail"></inject>**. A pop-up will prompt you to close the window. After logging in, Once you're logged in, close the pop-up and return to **Azure Storage Explorer**.


     ![](../media/Lab-05/odl-1.png)

5. You'll be able to view the resources, resource groups, and subscriptions.

    ![](../media/Lab-05/connect-00-1.png)

6. Click on **Connect** in the left-hand pane.

    ![](../media/Lab-05/connect-00.png)

7. In the **Select resources** window, choose the **ADLS Gen2 container or directory**.

    ![](../media/Lab-05/connect-adls-storage.png)

1. In the **Select connection method** window, select **Sign in using OAuth**, then click **Next**.

   ![](../media/Lab-05/select-cn.png)

1. On the **Select Account & Tenant** window, choose the account **<inject key="AzureAdUserEmail"></inject>** and select **next**.

    ![](../media/Lab-05/select-odluser-1.png)

2.  In the **Enter Connection Info** window,name it as **Fabric_mirroring (1)** paste the **landing URL (2)** that you copied in Task 1 and click on **Next (3)**

    ![](../media/Lab-05/fabric-mirroring.png)

1. In the **Summary** pane, review the settings and click **Connect**.

     
     ![](../media/Lab-05/connect-1.png)

1. Click on **Files(1)**, then select the **LandingZone(2)** folder.

     ![](../media/Lab-05/landing_zone.png)

1. From the toolbar, select **+New Folder (1)** and name it **source_employee (2)** and Click **Ok**.

     ![](../media/Lab-05/landing_zone.png)

2. Now, you will be able to see the newly created folder **source_employee**.

     ![](../media/Lab-05/source_empoyee-fold.png)

1. Select **Upload (1)**, then from the drop-down menu, choose **Upload files (2)** to upload the files to the folder.

    ![](../media/Lab-05/upload-1-1.png)

1. Now, in the **Upload Files** pane, **select the "..."** (ellipsis) that's highlighted. As it helps to browse and select files from the File Explorer.

   ![](../media/Lab-05/upload-1.png)

1. Go to the path **This PC/Downloads**, **select** the **`metadata.json`(1)** file, and **open(2)** it to upload.

    ![](../media/Lab-05/metadata-1.png)

1. Navigate Back to **Mirrored Database_<inject key="DeploymentID" enableCopy="false"/>** in fabric portal .

1. In the **Monitor Replication** blade, **click on Refresh**. You will be able to see the **source_employee** folder that has been created but is yet to succeed. You can ignore the warning.

    ![](../media/Lab-05/source-employee1.png)

> **Note:** The process may take 2-5 minutes. Use Monitor replication to track the status.

> **Note:** If the tables and replication status are not visible immediately, wait a few seconds and refresh the panel.

1. Go back to the Storage explorer and Select **Upload (1)**, then from the drop-down menu, choose **Upload files (2)** to upload the files to the folder.

    ![](../media/Lab-05/upload-1-1.png)

1. In the **Upload Files** pane, **select** the **"..."** (ellipsis) to browse for files. Then, go to **C:\Downloads\Labfiles**, **select** the **`00000000....1.parquet`** file, and **open** it to upload.
   
     ![](../media/Lab-05/001-ipload.png)

1. Leave all the settings as default in the **Upload Files** pane and click **Upload**.

     ![](../media/Lab-05/last-upload.png)

1. In Azure Storage Explorer, you will see a **success message** once the upload is complete.

   ![](../media/Lab-05/uploaded.png)

1. In the **Monitor Replication** blade of fabric, **click on Refresh**. You will see the **source_employee** folder that has been created, with the count displayed as **3**.

     ![](../media/Lab-05/source-employee3.png)


1. Now return to **Storage Explorer**, click **Upload** again to upload the **0000000....2.parquet** file. After the upload is complete  go to the **Monitor Replication** in Fabric, click **Refresh** and you should see the count increase. 

    ![](../media/Lab-05/monitor.png)

1. Choose **Query in T-SQL** from the **Monitor Replication** blade.

    ![](../media/Lab-03/query-1.png)

1. Click on **Refresh**, then expand **Schema** > **dbo** > **Tables**. You will see the **source_employee** table. From the three ellipses, choose **New Query**, and then select **TOP 100**.

     ![](../media/Lab-05/source-employee.png)

1. In the **Results** pane, you will see the results displaying the **Employee ID** and **Location**.

     ![](../media/Lab-05/result-of-1.png)

1. Now, return to **Storage Explorer**, click **Upload** again to upload the **0000000....3.parquet** file. Once the upload is complete, go to the **Monitor Replication** in Fabric, click **Refresh**, and you should see the count increase. 

    ![](../media/Lab-05/source_employee-last.png)

1. Select **Query in T-SQL** again from the **Monitor Replication** blade.

    ![](../media/Lab-03/query-1.png)

1. Run the query again that's already open. You will see only two employee IDs and their details because the last parquet file you uploaded is intended to delete the row.

    ![](../media/Lab-05/lab-5-last.png)


## Review

In this lab, you learned how to configure Microsoft Fabric open mirrored databases. You started by creating an open mirrored database within the Fabric environment. Next, you wrote change data into the landing zone to ensure proper synchronization. You then initiated the mirroring process to enable data replication across systems. Finally, you monitored the mirroring process to maintain data consistency and resolve any issues encountered during synchronization.


