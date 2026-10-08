# Azure Storage
A Storage Account is the Azure resource. The storage services/data live within that account. Lets starts by identifying Azure Storage services, storage-account types, replication, access, and secure endpoints. 
```text 
| Service           | Think of it as       |
| ----------------- | -------------------- |
| **Blob Storage**  | Objects/files        |
| **Azure Files**   | Cloud file shares    |
| **Queue Storage** | Messages             |
| **Table Storage** | NoSQL key-value data |
```

### 8.1 What is Azure Storage?
Azure Storage is Microsoft's cloud storage platform for storing data at large scale. It provides services for blobs, files, queues, and tables.
### 8.2 Azure Storage Services
- Blob Storage
- Azure Files
- Queue Storage
- Table Storage

Understand what problem each one solves:
```text
| Service           | Think of it as        | Typical use                            |
| ----------------- | --------------------- | -------------------------------------- |
| **Blob Storage**  | Object storage        | Images, videos, documents, backups     |
| **Azure Files**   | Cloud file share      | Shared Windows/Linux files             |
| **Queue Storage** | Message queue         | Asynchronous application communication |
| **Table Storage** | NoSQL key/value store | Simple structured datasets             |
```
Microsoft describes Blob as object storage for unstructured data, Files as managed file shares, Queues as asynchronous messaging, and Tables as schemaless NoSQL storage.
### 8.3 Storage Accounts
A storage account is an Azure resource that provides a unique namespace for your storage data.
```text
Resource Group
      │
      ▼
Storage Account
      │
      ├── Blob Storage
      ├── Azure Files
      ├── Queue Storage
      └── Table Storage
```
### 8.4 Storage Account Types
Microsoft currently recommends these main account types:
```text
Storage Account Types
│
├── Standard general-purpose v2
│
├── Premium block blobs
│
├── Premium file shares
│
└── Premium page blobs
```
### General-purpose v2
```text 
GPv2
 │
 ├── Blob Storage
 ├── Azure Files
 ├── Queue Storage
 └── Table Storage
```
It's the standard account type for most Azure Storage scenarios.
### 8.5 Performance Tiers
- Standard  
- Premium  

Designed for workloads requiring low latency and/or high transaction rates. Premium storage uses SSD-based storage for supported account types.High-performance application, High transaction rate, Low-latency workload

For example, GPv2 is a storage account type that normally uses Standard performance, while premium account types provide specialized premium workloads.
### 8.6 Replication
- LRS
- ZRS
- GRS
- GZRS
- RA-GRS
- RA-GZRS

### 8.7 Storage Endpoints

### 8.8 Storage Security
- Access Keys
- Microsoft Entra ID
- SAS
- Firewalls
- Private Endpoints

### 8.9 Azure Blob Storage
- Containers
- Blobs
- Access tiers
- Lifecycle management
- Versioning
- Soft delete
- Snapshots
- Object replication

### 8.10 Azure Files
- File shares
- SMB
- NFS
- Snapshots
- Soft delete
- Identity-based access

### 8.11 Storage Tools
- Azure Storage Explorer
- AzCopy

### 8.12 Data Protection
- Encryption
- Soft delete
- Versioning
- Replication