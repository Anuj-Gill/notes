#systemDesign 

Caching is a concept that involves **storing frequently accessed data in a location that is easily and quickly accessible.**
Improves performance and efficiency 

##### Types of cache

1) **Application Server cache**: An Application Server Cache is a storage layer within an application server that temporarily holds frequently accessed data![[Pasted image 20251031160308.png]]

2) **Distributed Cache**:  It pools together the random-access memory of multiple networked computers into a single in-memory data store used as  data cache to provide fast access to data![[Pasted image 20251031160936.png]]
	 It is of 2 types: Shard based caching(each node contains different portion of the cache. Horizontally scalable but if a node fails, the shard is lost if not replicated) and Replicated cache(each node has same cached data, poor scalability)

3) **Global Cache**: Will have a single cache space and all the nodes use this single space![[Pasted image 20251031161628.png]]

4) **CDN**: Group of servers that are strategically placed across the globe with the purpose of accelerating the delivery of web content. ![[Pasted image 20251031161743.png]]

### Cache Invalidation & Update Strategies

[[cacheInvalidation|Cache Invalidation]]

### Cache Eviction

[[cacheEvication|Cache Eviction]]

### Cache-aside Pattern (a.k.a. Lazy Loading)

In the cache-aside (or lazy-loading) pattern, **the application controls both reading and writing to the cache**.  
The cache starts empty and is **populated on demand** — only when data is first requested.
#### How it Works

1. Application first checks the cache for the data (cache lookup).
2. If **cache hit** → return data directly.  
3. If **cache miss** → fetch data from the database, return it, and store it in the cache for future requests.

For writes/updates:
- The application updates the **database first**,  
- Then **invalidates or updates** the corresponding cache entry.