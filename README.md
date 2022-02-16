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
         

#### **Instance Store**

- Persistent Disk

#### **Database Storage**

- SQL
- NoSQL
- Analytic