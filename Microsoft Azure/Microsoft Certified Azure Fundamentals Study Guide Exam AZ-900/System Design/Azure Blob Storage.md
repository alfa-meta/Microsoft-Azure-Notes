Here's what you need to know about Azure Blob Storage for the AZ-204. It sits within the "Develop for Azure storage" domain, which is 15–20% of the exam. The official study guide breaks it down into four skill areas:

---

**1. Set and Retrieve Properties and Metadata**

- Blob system properties (content type, ETag, last modified, etc.) vs. user-defined metadata (key-value pairs)
- Using the .NET SDK (`BlobClient`, `BlobContainerClient`) to get/set these
- Know the difference between properties and metadata — exams test this distinction

**2. Perform Operations Using the SDK**

- CRUD operations on blobs: `UploadAsync`, `DownloadAsync`, `DeleteAsync`, `CopyFromUriAsync`
- Working with containers: creating, listing, deleting
- `BlobServiceClient` → `BlobContainerClient` → `BlobClient` hierarchy
- Listing blobs with filters/prefixes
- Using `UploadFromFileAsync` for large file uploads
- AzCopy for automated, high-throughput, resumable bulk transfers (know when to use it vs. SDK vs. Storage Explorer)
- Event Grid integration for reacting to blob events (e.g., trigger a Function on blob upload)

**3. Implement Storage Policies and Data Lifecycle Management**

- Lifecycle management policies: rules to automatically tier or delete blobs based on age (days since last modified/accessed)
- Access tiers: Hot, Cool, Cold, Archive — costs and trade-offs of each
- **Critical caveat**: Premium block blob storage accounts do NOT support tiering to other tiers — only deletion via lifecycle policies
- Immutability policies: time-based retention and legal hold (WORM — write once, read many)
- Version-level immutability support

**4. Access Control**

- Shared Access Signatures (SAS): account SAS vs. service SAS vs. user delegation SAS
- SAS is the answer for time-limited, revocable access (not storage account keys)
- RBAC roles for more permanent access
- Know when to use each — this comes up repeatedly in scenario questions

---

**Other topics that appear frequently in exam questions:**

- **Storage account types**: General-purpose v2 and Blob Storage support Event Grid; v1 does not
- **Blob types**: Block blob (most common), Append blob, Page blob — tiering only works on block blobs
- **Change feed**: ordered, durable log of all changes to blobs — used for auditing
- **Point-in-time restore**: lets you restore containers to an earlier state
- **Soft delete**: recovers accidentally deleted blobs
- **Encryption**: server-side encryption is on by default; customer-managed keys via Key Vault is a separate topic

---

The exam focuses heavily on scenario-based questions — "which feature should you use to achieve X?" rather than pure recall. Focus on knowing _why_ you'd pick lifecycle management over snapshots, SAS over access keys, Event Grid over polling, etc. Hands-on practice with the SDK will solidify the API surface quickly.
