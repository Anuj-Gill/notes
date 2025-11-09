#systemDesign 

### Cache Eviction Policies

Eviction policies define **which items are removed** from the cache when it reaches its capacity.

---

1) **LRU (Least Recently Used)**
   - Removes the item that hasn’t been accessed for the longest time.
   - Assumes recently used data will be used again soon.
   - Pros: Simple and effective for temporal locality.
   - Cons: Can perform poorly if data access patterns change suddenly.
   - Use Case: General-purpose caching (Redis default policy `volatile-lru`).

2) **LFU (Least Frequently Used)**
   - Removes the item accessed the fewest times.
   - Tracks access count for each key.
   - Pros: Works well for data with stable access frequency.
   - Cons: Higher overhead due to maintaining access counts.
   - Use Case: Recommendation systems, analytics, or APIs with hot items.

3) **MRU (Most Recently Used)**
   - Removes the most recently accessed item first.
   - Based on the idea that recently accessed items may not be reused soon.
   - Pros: Useful for one-time-use data patterns.
   - Cons: Not suitable for general workloads.
   - Use Case: Database page caching, when recently used pages are least likely to be reused soon.

4) **FIFO (First In, First Out)**
   - Removes the oldest cached item (based on insertion time).
   - Pros: Simple to implement.
   - Cons: Doesn’t consider how frequently or recently items are used.
   - Use Case: Streams, queues, or cyclic data where age matters more than access frequency.

5) **Random Replacement**
   - Randomly removes any item when space is needed.
   - Pros: Very fast, low overhead.
   - Cons: May evict useful items.
   - Use Case: High-throughput systems prioritizing speed over precision (e.g., small edge caches).

6) **TTL-based Eviction**
   - Each entry expires after its set TTL (time-to-live).
   - Eviction happens automatically based on expiry time.
   - Pros: Prevents stale data.
   - Cons: Does not adapt to cache pressure; relies on static TTL values.
   - Use Case: Config data, API responses, or temporary session data.

7) **ARC (Adaptive Replacement Cache)**
   - Maintains two lists: recent and frequent items, dynamically balancing between them.
   - Pros: Adapts to varying workloads; better hit ratio than LRU or LFU.
   - Cons: Complex to implement.
   - Use Case: File systems, databases (e.g., ZFS uses ARC).

8) **SLRU (Segmented LRU)**
   - Cache divided into segments (e.g., probationary and protected).  
     New items go into probationary; if accessed again, they move to protected.
   - Pros: Improves hit rate by filtering out transient data.
   - Cons: More complex metadata management.
   - Use Case: Web proxies, CDN edge servers.