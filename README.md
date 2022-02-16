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
  

#### **Instance Store**

- Persistent Disk

