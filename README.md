# GCP Professional Data Engineer

Repository to store study notes for the GCP Professional Data Engineer certification.

Source: [Dan Sullivan course on Udemy](https://www.udemy.com/course/google-cloud-professional-data-engineer-get-certified/)

## Storage in GCP

#### **Object storage: Cloud Storage**

- Unstructured data (image, video, text).
- Archived data.
- Temporary storage between services.
- Global access, web accessible. 
- Logical Organization.
  - Global namespace.
  - Buckets.
  - Folders.
  - Objects.


- **Storage Classes**
   - High Peformanec Obj Storage.
      - Frequent access.
      - Optimized for performance.
   - Backup and Archival.
      - Nearline.
      - Coldline.
      - Archive.


- **Redundancy**
    - Regional.
      - Data stored in single region.
      - Replicated across zones. 
    - Dual-region.
      - Data replicated across specific pair of region.
      - Auto-failover.
    - Multi-region.
      - Data replicated across US, EU or Asia.
      - Auto-failover.


- **Object lifecycle management**
    - Automatically transition to lower cost storage class. Specified by lifecycle policy. Useful for archiving.
     - Retation policies specifies minimum retention period.
     - Object holds prevent deeletion.

- **Security**
     - Automatic encryption: Googe Managed, Customer Managed - using Cloud Key Management Service, Customer supplied - using key created and managed by customer.
     - Bucket level access using object access control lists (ACLs).
     - Activity logs and data access logs.

- **Loading data**
  - Small amounts of data: console, command line, API.
  - Bulk data loads: Cloud Storage Transfer, Transfer Appliance.
  - Cloud Storage Transfer: 
    - Move data from onpremises or other cloud providers.
    - Between cloud storage buckets.
    - One time or recurring transfers.
    - Onpremises transfer:
      - Less than 1TB - use gustil.
      - Bigger than 1 TB - use Cloud Storage Transfer Service.
  - Transfer Appliance:
    - High capacity storage server.
    - Shipped to your data center.
    - Physically attach to your network to copy data.
    - Ship back to Google Cloud.
    - Data copied to Cloud Storage.
    - Capacity: 100 TB, 400 TB.

- **Access Controls for Cloud Storage**
  - Uniform bucket level access:
    - Recomended
    - Uses Cloud Identity and Access Management (IAM).
    - Applies permissions to objects in bucket or group of objects with common prefix.
  - Fine-grained:
    - Legacy access control method.
    - Use access control lists.
    - Apply permissions at both bucket and object level. 
  - Signed URLs:
    - Time limited read or write access to an object.
    - Generated URL.
    - Allow access to those without IAM authorizations.
  - Signed Policy Document:
    - Specify what can be uploaded to a bucket.
    - Control
      - Size
      - Type
      - Other upload characteristics
    
- **Life Cycle Policy Management**
  - Rules applied to buckets and contents.
  - Actions executed when condition applies.
    - Change storage class based on age.
    - Delete object based on date.
    - Purge number of versions of an object.
  - Actions:
    - Delete:
      - Delete object.
      - If versioned, deleting live version creates non-current version.
      - If versioned, deleting non-current version deletes obj permanently.
    - Change storage class:
      - Multi-region - Nearline, Coldline, Archive.
      - Standard - Nearline, Coldline, Archive.
      - Nearline - Coldline, Archive.
      - Coldline - Archive.
  - Conditions:
    - Age.
    - Created before.
    - Is live.
    - Matches Storage Class.
    - Number of newer versions.

#### **Database Storage**

- SQL
- NoSQL
- Analytical

###### **Relational Databases: Cloud SQL**
- When to use Relational Databases:
  - Structured data.
  - Predefined schema.
  - Data structures:
    - Tables
    - Index
    - Views
    - Constraints
    - Partitions
  - ACID transactions.
  - Strong consistancy.

- Defining databases:
  - SQL (Structed Query Language).
  - DDL (Data Definition Language), DML (Data Manipulation Language) etc.
  - Tables store data.
  - Index can improve access time.
  - Partitions shard data.

- When not to use Relational Databases:
  - Data warehousing - analytical.
  - Unstructured data - Object storage.
  - Semi-structured data - Document or Wide-Column.
  - High volume, low latency writes - Wide-Column.

- When to use Cloud SQL:
  - Geography:
    - Regional data store.
    - Multiple zones for high availability.
    - Multi-region for backups only.
  - Up to 30 TB per database.
  - Using one of:
    - PostgreSQL
    - MySQL
    - SQL Server

###### **Relational Databases: Cloud Spanner**

- When to use Cloud Spanner:
  - Structured data.
  - Predefined schema.
  - Data structures.
  - ACID transactions.
  - Strong consistancy.
  - Globally distributed (multi-region).
  - High available.
  - Up to 2 TB per node.
  - Managed:
    - Automatic replication.
    - No planned downtime.
  - Hot Spotting:
    - Hot spots occour when many read and write ops happen on the same node.
    - Can happen with sequential primary key:
      - Auto-increment values.
      - Timestamps.
    - Consider using:
      - Hash value of primary.
      - Bit reverse sequential values. 
      - Promote high cardinality attributes in multi-attribute primary key.
  - Intervealed Tables:
    - Parent-child relationship.
      - Order to order line.
      - Person to addresses. 
    - Row from parent table stored with rows from child table.
    - More efficient when retriving data from both.

###### **NoSQL Databases: Cloud Firestore**

- Cloud Firestore (datastore) and Document databases:
  - Managed NoSQL database.
  - Uses the document model:
    - Key-value pairs.
    - Hierarchical.
  - Two modes:
    - Native mode.
    - Datastore mode.

- When to use Document Databases:
  - Semi-structured data.
    - Schema is not fixed.
    - May change over time.
    - Different attributes across entities.
    - Query on multiple attributes. 
    - Bounded ingestion volumes.

- Choosing a mode:
  - Datastore mode:
    - Backend for server applications.
    - Does not require synching with mobile device.
  - Native mode:
    - Used with mobile & web applications.
    - Supports large number of connections.
    - Mobile and web clients. 

- Entities:
  - Entities describes or represent a thing.
  - A single entity is analogous to a row in a relational table.
  - A set of entities with similar attributes is somewhat analogous to a table in a relational database. 
  - Entities have properties.
  - Related entities knows as Kinds.
  - Similar to json struct.
  - Atomic values:
    - Integers.
    - Floating points numbers.
    - Strings. 
    - Dates.
  - Arrays.
  - Entities.

- Index
  - Indexes rerquired for all queries.
  - no scanning.
  - Two types:
    - Single field.
    - Composite.
  - Automatically create some index.
    - Atomic values, ascending and descending.
    - Maps and arrays also.
  - Composite Indeex:
    - Index with multiple fields.
    - Not automatically created. 
    - Used when querying and filtering using multiple attribute
  - Limitaions:
    - Queries with range filters on different fields are not supported.
    - Some limits on OR queries.

- Transactions:
  - Set of one or more operations on one or more entities.
  - Guaranteed to be atomic.
    - Never partially applied.
    - All operations succeed or fail.
  - Optional for database operations.
  - Serializable operations:
    - Data read by a transaction cannot be concurrently modified.
    - Transaction sees consistent snapshot of the database.
    - Writes within a transaction not available to remaining operations in transaction.
    - When no writes in a transaction, use read-only transacion to improve performance. 
  - Limitations:
    - Time:
      - Maximum duration of 60 seconds.
      - 10 seconds idle expiration after 30 seconds.
    - Modify up to 500 entities in a single transaction.
    - Transactions can fail due to:
      - Too many concurrent modifications.
      - Exceeds resource limits.
      - Internal error.

###### **NoSQL Databases: Big Table**

- Wide column database.
  - Relational databases build on the table abstraction.
  - Document database build on hierarchical key-value pairs.
  - Wide column database build on the sparse multidimensional array.

- Use cases of Wide Column databases:
  -  Petabyte scale data.
  - Low latency writes.
  - Key based reads.
  - Analytics.
  - Time series applications: IoT, Finance, Monitoring.
  - Open source wide columns databases: HBase (Hadoop), Cassandra.

- Big Table:
  - Managed service.
  - Specify cluster config: number of nodes, SSD or HDD, Region/Zone.
  - Scales linearly.
  - Multi-region high availability with cluster replication.
  - HBase API.

- Row key:
  - Unique identifier for a row of data stored in big table.
  - Analogous to a primary key in relational databases.
  - But...
    - Bigtable does not support joins, so no concept of foreign keys.
    - Rows keys determine where data is writen.
  - Need to consider how structure of row-key affects performance.
  - Row key and data location:
    - Big Table writes data to multiple serves (nodes).
    - Each node handles a subset of workload.
    - Within nodes there are multiple sorted-string tables (SSTables).
    - Data is sharded into blocks of contiguous rows, called tablets.
    - Tablets are stored in Colossus file system.
    - IMPORTANT:
      - Data is never stored on a node.
      - Only metadata stored on node.
      - Data stored in Colossus.
  - Designing Row keys.
    - Row keys determine node and SSTable where data is written.
    - Ideally, reads and writes distributed evenly across nodes.
    - Need row key evenly distributed across nodes.
    - Avoid:
      - Linearly incrementing row keys.
      - Low cardinality.

- Query Patterns and Denormalization:
  - Each table has one index based on row key.
  - Rows are sorted by row-key.
  - Columns are grouped into families.
  - All operations are atomic at the row level.
  - Related entities should be stored in adjacent rows to improve read efficiency.
  - Empty columns do not take any space.
  - Column Families:
    - Set of related columns in a table.
    - Frequently access together:
      - Street address, city, state, country.
      - Product name, type, size. 
    - Supports up to 100 columns families efficiently.
  - Tables:
    - Limit of 1000 tables per instance.
    - Better store data in one table than many.
    - Small tables problematic.
      - Sending requests to many tables increases connection overhead and latency.
      - More dificult to load balance. 

- Time Series data. 
  - Data that includes a time and measurements.
  - Basic design patterns:
    - Row and time buckets.
      - New columns for new events.
      - New rows for new events.  
    - Rows represent sigle events.
      - Serialized column data.
      - Unserialized column data. 
    - Row with time buckets: 
      - Each row represents a bucket of time such as an hour or day.
      - Row key includes non-timestamp identifier such as 'hour123'.
      - Row size limited to 100 MB, upper limit on size of buckets. 
      - Advantages:
        - Better read and write performance than row per event.
        - Compression more efficient. 
      - Disadvantage: more complex application code. 
    - Row with time buckets - Column per Event:
      - Write new column for each event.  
      - Value stored in column qualifier/name rather than in cell.
      - Send column family, column qualifier, timestamp.
      - Row store the vallues for one metric.
      - Use this pattern when:
         - Do not need to measure changes in time series.
         - Save storage space by using column qualifiers as data.  
    - Row with time buckets - Cell per Event:
      - Write new cell for each event.
      - Save multiple timestamped events in a single column of a row.
      - Each row contains all metrics of an event.
      - Use whe you want to measure chanes in measurements over time. 
    - Single Timestamp Rows - Serialized:
      - Write new row for each event.
      - Row key uses timestamp value as suffix. 
      - Known as 'Tall and Narrow' pattern.
      - Values stored in serialized format such as probufs. 
      - Advantage:
        - Storage efficiency.
        - Speed.
      - Disadvantages:
        - Can not retrieve a single column.
        - Need to deserialize data in application.
      - Use when:
        - Query patterns fluxuate.
        - Cost concerns. 
        - Events may exceed 100 MB otherwise.
    - Single Timestamp Rows - UnSerialized:
      - Store events in a new row.
      - Do not serialize de data.
      - Advantages: Easier ti implement than time buckets pattern.
      - Disadvantages:
        - Less performant.
        - Not as efficiently compressed. 
        - Potencial for hotspotting. 
      - Use when:
        - Retrieve all columns for a time range.
        - Do not want to serialize.

- Others considerations:
  - Store data in multiple tables with different row keys if needed for different query patterns. 
  - Combine features of different patterns, such as serializing data in time buckets. 

###### **Analytical Databases: Big Query Data Management**

- Serveless data warehouse.
- Petabyte scale.
- Uses SQL but is not a relational database. 
- Analytical database.
- Other features: BQ ML, BQ BI Engine, BQ GIS.
- Datasets and tables: 
  - Dataset: 
    - Collection of tables and viws. 
    - Access control set at dataset level.
  - Tables:
    - Supports scalar and nested structures.
    - Stored in columnar format.
    - Partitioning.
  - Views:
    - Projection of one or more tables.
    - Tables can be joined.
    - Views can be materialized.
  - Array type:
    - Arrays:
      - Ordered list of zero or more elements of non-arrays values. 
      - Arrays os arrys are not allowed.
      - Arrays of struct are allowed.
  - Struct type:
    - Container of ordered fields each with a type and name. 

- Data Access Controls:;
  - Access control applied at:
    - organization or project level.
    - dataset level. 
    - table or view. 
  - Predefined IAM Roles:
    - BQ Admin.
    - BQ Owner/Editor/Viewer.
    - BQ Resource Admin/Editor/Viewer.
    - BQ User.
    - BQ Job User.
  - Column level security.
    - Restrict access to sensitive info in a table.
    - Taxonomy of tags in data catalog.
    - Tags assigned to columns in BQ.
    - Use IAM roles to restrict access to each policy tag.
  - Basic roles:
    - Exist prior to IAM.

- Partitioned tables:
  - Table is divided into segments called partitions.
  - Improves query performance.
  - Lowers cost.
  - Partition by ingestion time:
    - Loads data into daily, date-based partitions.
    - Automatically creates new partitions.
    - Uses ingestion time to determine partition.
    - Create pseudo-column_PARTITIONTIME
      - Date-based timestamp.
      - Used in queries to limit the number of partitions scanned.
  - Date/Timestamp partitioning:
    - Partition based on date or timestamp column.
    - Each partition holds one time unit of data where time unit is hour, day, month etc.
  - Special partitions:
    - __ NULL __ when nulls in partition column.
    - __ UNPARTITIONED __ when values in column outside allowed range. 
  - Integer Range Partitioning:
    - Use separate a column with integer value.
    - Specify start and end of the range.
    - Interval of each partition in the range. 
    - Values outside the range to the UNPARTITIONED partition.
  - Date/Timestamp partitioning vs Sharding:
    - Use separate tables for each day.
    - TABLE_NAME_PREFIX_YYMMDD.
    - Use UNION in queries to scan multiple tables. 
  - Partitioning is preferred over sharding.
    - Less metadata to maintain.
    - Less permission checking overhead.
    - Better performance.

- Clustered Tables:
  - Data sorted based on values in one or more columns. 
  - Can improve performance of aggregate queries.
  - Can reduce scanning when cluster columns used in WHERE clause.
  - Use clustering when:
    - Table is partitioned.
    - Unpartitioned tables cannot be clustered.
    - Commonly use filters or aggregations against the cluster columns. 
  - BQ automatically re-clusters in the background.

- Load Data Sources
  - Cloud Storage.
  - Other Google services.
  - Local Machine.
  - Streaming inserts.
  - DML bulk loads.
  - Cloud Dataflow.
  - BQ command line, API and GUI. 
  - Avro is preferred format.

#### **Instance Store**

- Persistent Disk

###### **Migrating a Data Warehouse**

Read: https://cloud.google.com/architecture/dw2bq/dw-bq-migration-overview

###### **Caching Data and Cloud Memorystore**

- Cloud Memorystore:
  - Managed Redis and Memcached.
  - Caching data.
  - Basic Tier:
    - Cahe with no replication. 
    - No cross zone replication.
    - No automatic failover.
  - Standar Tier:
    - Cache with replication.
    - Cross zone replication.
    - Automatic failover.

###### **Workflow and ETL/ELT**

- Cloud Composer:
  - Airflow managed service. 
  - Execute workflows in term os DAGS. 
  - Console or cli. 
  - Workflow: collection of tasks. 
  - DAGS are stored in Cloud Stored. 
  - Log associate with sigle DAG task.

- Cloud Data  Fusion:
  - Managed service based on open source project CDAP. 
  - Code-free ETL/ELT tool.
  - Grag and drop ETL tool. 
  - Cloud Data Instance deployed as an instance. 
  - Two editions:
    - Basic.
    - Enterprese. 
    