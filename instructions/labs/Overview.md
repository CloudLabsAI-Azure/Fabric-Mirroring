## Overview of Microsoft Fabric Mirroring

Mirroring in Fabric is a low-cost and low-latency solution to bring data from various systems together into a single analytics platform. You can continuously replicate your existing data estate directly into Fabric's OneLake from a variety of Azure databases and external data sources.

With the most up-to-date data in a queryable format in OneLake, you can now use all the different services in Fabric, such as running analytics with Spark, executing notebooks, data engineering, visualizing through Power BI Reports, and more.

Mirroring in Fabric allows users to enjoy a highly integrated, end-to-end, and easy-to-use product that is designed to simplify your analytics needs. Built for openness and collaboration between Microsoft, and technology solutions that can read the open-source Delta Lake table format, Mirroring is a low-cost and low-latency turnkey solution that allows you to create a replica of your data in OneLake which can be used for all your analytical needs.

## Why use Mirroring in fabric

Accessing and working with this data today requires complex ETL (Extract Transform Load) pipelines, business processes, and decision silos, creating:

- Restricted and limited access to important, ever changing, data.
- Friction between people, process, and technology
- Long wait times to create data pipelines and processes to critically important data
- No freedom to use the tools you need to analyze and share insights comfortably
- Lack of a proper foundation for folks to share and collaborate on data
- No common, open data formats for all analytical scenarios - BI, AI, Integration, Engineering, and even Apps

## Types of Mirroring 

1. **Database Mirroring**:

Database Mirroring in Microsoft Fabric allows you to replicate entire databases or tables from external systems into OneLake (the unified data lake within Microsoft Fabric). The primary focus here is data replication, meaning the actual data from different sources is physically copied and made available for analysis within Fabric.

You can bring in data from external systems such as SQL databases, NoSQL stores, or other data warehouses.
The entire database or individual tables can be mirrored into Fabric, making it available for analysis and reporting within the platform.
The data is physically replicated, so you’ll have a copy of the data in Fabric, but it’s synchronized so changes in the source can be mirrored into Fabric.

**Use cases**:

 - Centralized Analytics: If you want to consolidate data from multiple sources into a single platform (like a data warehouse or lake) for reporting, querying, or machine learning.
 - Backup and Recovery: You can replicate databases for disaster recovery or high-availability purposes.


2. **Metadata Mirroring**:

Metadata Mirroring is a more lightweight approach compared to database mirroring. Instead of physically moving the data, metadata mirroring only synchronizes the metadata of the datasets—things like catalog names, schemas, and table structures.

This approach does not move the actual data; instead, it syncs metadata (the structure or "schema" of the data).
You create shortcuts to the original data, allowing you to access it from within Microsoft Fabric without having to replicate it physically.
The original data remains in its source location (e.g., in a different database or cloud storage), but Fabric can access it in real time, making the data feel local.

**Use cases**:

- Cost-efficient access: If your data is large or stored in external sources (e.g., cloud storage or third-party databases), you can access it without duplicating it, which helps save storage and reduce costs.
- Real-time updates: Because you're working directly with the metadata, any changes in the source data are reflected immediately.


3. **Open Mirroring**
   
  Open Mirroring in Microsoft Fabric is designed to work with open Delta Lake table formats. Delta Lake is an open-source storage format built on top of Apache Parquet, which provides ACID transactions and scalable metadata handling for large datasets. With Open Mirroring, developers can write change data (data changes) directly into a mirrored database item in Fabric.

Open mirroring extends traditional mirroring by allowing data to be changed directly in Fabric.
Using Delta Lake format (which is widely supported across platforms), you can create open APIs for applications to write incremental data updates (like new records or changes) directly into the mirrored data structure in Fabric.

**Use cases**:

- Change Data Capture (CDC): If your application generates frequent updates (like transactional systems), you can mirror and capture these changes directly into Fabric, which allows for up-to-date analytics.
- Open-source integration: Open mirroring leverages open formats like Delta Lake, allowing it to work across various ecosystems and giving you more flexibility in how data is integrated.


### Conclusion  

Microsoft Fabric's **mirroring** capabilities provide flexible data integration options to suit different needs. **Database mirroring** replicates entire databases for full data availability, while **metadata mirroring** syncs only schema information, reducing storage costs. **Open mirroring** supports real-time change data capture, enabling continuous data synchronization. These approaches help businesses centralize data, reduce duplication, and maintain up-to-date analytics. Ultimately, Fabric’s mirroring options empower seamless, scalable, and efficient data management across diverse systems.
