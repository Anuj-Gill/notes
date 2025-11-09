#systemDesign 

##### What is Sharding?
- Sharding is a method to **horizontally scale** a database. It involves splitting the main database into multiple smaller partitions, where each partition contains a unique subset of the data.
- Each unique partition is known as a **Shard**.
- Sharding is generally considered a **last-resort scaling option**. Before implementing it, databases are usually scaled using simpler approaches such as **replication**, **vertical scaling (adding more CPU/RAM)**, and **caching** to handle increased load.
##### Sharding Methods

1. **Key-Based Sharding**
   - A specific column (like `user_id`, `id`, `email`, etc.) is chosen as the sharding key.  
   - We apply a **hash function** on that key, then take the result modulo the total number of shards to get the target shard index.
   - The focus is on using a **fast and evenly distributed** hash function (not cryptographically secure).  
     Commonly used functions include **MurmurHash**, **xxHash**, or **CityHash**.
   - Instead of directly dividing by the number of shards, large systems often use **consistent hashing** (a circular hash ring) to minimize data reshuffling when adding or removing shards.

2. **Range-Based Sharding**
   - Data is divided based on predefined **ranges** of a column.
   - Example: users with IDs `1–100` in Shard 1, `101–200` in Shard 2, and so on.
   - Simple to implement but can lead to **hotspots** if certain ranges get more traffic or inserts.

3. **Vertical Sharding**
   - Instead of dividing rows, the **columns** of a table are split across different databases.
   - Example: separating user profile info and authentication info into different databases.
   - Useful when different parts of the data are accessed independently.

4. **Directory-Based Sharding**
   - A **lookup service or mapping table** is maintained that records which shard contains which data.
   - The application first queries this directory to find the correct shard for a given key, then routes the request to that shard.
   - Offers flexibility in shard placement but adds an extra lookup step and potential single point of failure if not handled properly.
