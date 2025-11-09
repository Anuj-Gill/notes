#systemDesign 

### Cache Invalidation & Update Strategies

1) **Time-based (TTL / Expiry)**
   - Each cache entry has a fixed lifetime (e.g., 5 minutes).  
   - After expiry, it’s automatically removed or refreshed on next access.  
   - Pros: Simple, auto-refreshes stale data  
   - Cons: May serve slightly stale data before expiry

2) **Key-based**
   - When a specific data item changes in the database:  
     - Either update that cache key’s value, or  
     - Delete it so it’s reloaded on the next cache miss.  
   - Pros: Granular and controlled  
   - Cons: Requires tracking which key corresponds to which data

3) **Write-through**
   - All writes go through the cache, updating both cache and database synchronously.  
   - Pros: High consistency  
   - Cons: Slower writes due to synchronous updates

4) **Write-behind (Write-back)**
   - Writes update the cache first; the database is updated asynchronously later.  
   - Pros: Very fast writes  
   - Cons: Temporary inconsistency (cache and DB may differ for a short time)

5) **Purge / Flush**
   - Clears the entire cache (e.g., during deployment or major schema change).  
   - Pros: Ensures 100% freshness  
   - Cons: Causes cold cache, increased load on the database

6) **Read-through**
   - The application always reads through the cache.  
     If the key is not found, the cache fetches the data from the database and stores it automatically.  
   - Pros: Simplifies application logic  
   - Cons: Slightly higher latency on first read (cache miss)

7) **Tag-based (Dependency-based)**
   - Cache items are grouped by tags (e.g., `product:55`), allowing invalidation of all related entries by tag.  
   - Pros: Efficient for related data (products, posts, users, etc.)  
   - Cons: Slightly higher metadata management overhead

8) **Lazy / On-demand Invalidation**
   - Cache entries are checked for freshness (via timestamp or version) when read.  
   - If stale, they are reloaded on-demand.  
   - Pros: Good for infrequent updates  
   - Cons: Adds overhead per read operation