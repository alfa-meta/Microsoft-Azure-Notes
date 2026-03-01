Here are the 20 most important Azure CLI commands for **Cosmos DB** in the AZ-204 exam:

---

## Account Creation & Management

1. `az cosmosdb create --name <account> --resource-group <rg> --locations regionName=uksouth failoverPriority=0 isZoneRedundant=false` Create a Cosmos DB account. Know that `--locations` with `failoverPriority=0` sets the primary write region. Adding multiple `--locations` entries enables multi-region distribution.
    
2. `az cosmosdb create --name <account> --resource-group <rg> --kind MongoDB --locations regionName=uksouth failoverPriority=0` Create a Cosmos DB account with a specific API. Know `--kind` values: `GlobalDocumentDB` (default, SQL API), `MongoDB`, `Parse`. For Cassandra, Gremlin, and Table APIs, `--kind` is `GlobalDocumentDB` and `--capabilities` sets the API type.
    
3. `az cosmosdb create --name <account> --resource-group <rg> --enable-automatic-failover true` Enable automatic failover. Know that automatic failover only applies when you have multiple regions configured — it promotes the next highest priority region when the primary goes down.
    
4. `az cosmosdb create --name <account> --resource-group <rg> --default-consistency-level Session` Create an account with a specific consistency level. **Heavily tested.** Know all five levels in order of strongest to weakest: `Strong` → `BoundedStaleness` → `Session` → `ConsistentPrefix` → `Eventual`. `Session` is the default.
    
5. `az cosmosdb update --name <account> --resource-group <rg> --default-consistency-level Eventual` Update the consistency level on an existing account. Know you can relax consistency freely but strengthening it has restrictions — you cannot move to `Strong` on a multi-region account with multiple write regions.
    
6. `az cosmosdb show --name <account> --resource-group <rg>` Show account details including endpoint URI, consistency level, and replication regions. Used to retrieve the document endpoint for SDK configuration.
    
7. `az cosmosdb keys list --name <account> --resource-group <rg>` List primary and secondary read-write and read-only keys. Know that Cosmos DB provides four keys: primary key, secondary key, primary read-only key, secondary read-only key.
    
8. `az cosmosdb keys list --name <account> --resource-group <rg> --type connection-strings` List connection strings directly. Commonly used when configuring `CosmosClient` in application settings.
    

---

## Databases

9. `az cosmosdb sql database create --account-name <account> --resource-group <rg> --name <db>` Create a SQL API database. No throughput set here means throughput must be set at the container level.
    
10. `az cosmosdb sql database create --account-name <account> --resource-group <rg> --name <db> --throughput 400` Create a SQL API database with **shared throughput** (RU/s). Know that throughput set at the database level is shared across all containers in that database that do not have their own dedicated throughput. Minimum is 400 RU/s.
    
11. `az cosmosdb sql database create --account-name <account> --resource-group <rg> --name <db> --max-throughput 4000` Create a database with **autoscale throughput**. Know the difference: manual throughput (`--throughput`) is fixed; autoscale (`--max-throughput`) scales between 10% of max and the max value automatically based on load.
    

---

## Containers

12. `az cosmosdb sql container create --account-name <account> --resource-group <rg> --database-name <db> --name <container> --partition-key-path /category` Create a container with a partition key. **The most tested Cosmos DB command.** Know that partition key choice is critical — it should have high cardinality, be included in most queries, and distribute data evenly. Cannot be changed after creation.
    
13. `az cosmosdb sql container create --account-name <account> --resource-group <rg> --database-name <db> --name <container> --partition-key-path /id --throughput 400` Create a container with dedicated throughput. Use this when a container needs guaranteed RU/s independent of other containers in the database.
    
14. `az cosmosdb sql container create --account-name <account> --resource-group <rg> --database-name <db> --name <container> --partition-key-path /id --max-throughput 4000` Create a container with autoscale throughput. Know the autoscale minimum is always 10% of the max — so `--max-throughput 4000` means the container scales between 400 and 4000 RU/s.
    
15. `az cosmosdb sql container create --account-name <account> --resource-group <rg> --database-name <db> --name <container> --partition-key-path /id --ttl 3600` Create a container with a default **Time to Live (TTL)** in seconds. Know TTL behaviour: `-1` means items inherit the container default but don't expire unless the item has a `ttl` property; a positive value deletes items automatically after that many seconds. TTL must be enabled at the container level first.
    
16. `az cosmosdb sql container update --account-name <account> --resource-group <rg> --database-name <db> --name <container> --ttl -1` Enable TTL on an existing container without setting a default expiry. Individual items can then set their own `ttl` property.
    
17. `az cosmosdb sql container show --account-name <account> --resource-group <rg> --database-name <db> --name <container>` Show container details including partition key, indexing policy, throughput, and TTL settings.
    

---

## Throughput Management

18. `az cosmosdb sql container throughput update --account-name <account> --resource-group <rg> --database-name <db> --name <container> --throughput 1000` Update manual throughput on a container. Know that you can scale RU/s up at any time but scaling down is limited — you can only decrease to the minimum recommended RU/s based on data stored.
    
19. `az cosmosdb sql container throughput migrate --account-name <account> --resource-group <rg> --database-name <db> --name <container> --throughput-type autoscale` Migrate a container from manual to autoscale throughput. Also know `--throughput-type manual` to go the other direction. This is tested in scenarios where workloads become unpredictable.
    

---

## Regions & Failover

20. `az cosmosdb failover-priority-change --name <account> --resource-group <rg> --failover-policies uksouth=0 ukwest=1` Change failover priority across regions. Know that `failoverPriority=0` is always the write region. In a manual failover scenario (automatic failover disabled), you use this command to promote a read region to write region during planned maintenance.

---

## Key Concepts the Exam Tests Beyond Commands

**Consistency levels** are the single most tested Cosmos DB topic in AZ-204. Memorise this:

|Level|Guarantee|Trade-off|
|---|---|---|
|Strong|Always reads latest write|Highest latency, not available multi-region multi-write|
|Bounded Staleness|Reads lag by K versions or T time|Good for global apps needing near-strong|
|Session|Consistent within a client session|Default — best balance for most apps|
|Consistent Prefix|Reads never see out-of-order writes|Lower latency, eventual-like|
|Eventual|No ordering guarantees|Lowest latency, highest availability|

**Partition keys** — know that a logical partition is defined by partition key value, a physical partition holds multiple logical partitions, and cross-partition queries are more expensive. Synthetic partition keys (concatenating fields) are used when no single field has sufficient cardinality.

**Request Units (RUs)** — all operations cost RUs. A read of a 1KB item costs 1 RU. Writes cost more. If RU/s is exceeded, Cosmos DB returns a `429 Too Many Requests` response and the SDK handles retries automatically with exponential backoff.

**Change Feed** — Cosmos DB automatically records all inserts and updates (not deletes by default) in order per partition key. Commonly tested as a trigger source for Azure Functions using the Cosmos DB trigger binding.

**Indexing** — all properties are indexed by default (opt-out model). You can customise the indexing policy to exclude paths and reduce RU consumption on write-heavy workloads.