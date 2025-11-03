## Key Characteristics

- **High throughput, low storage**  
  Kafka is designed for **high-throughput, real-time data streaming** with low latency.  
  It can handle millions of messages per second efficiently.

- **Comparison with PostgreSQL**  
  - **Kafka** → High throughput, low storage (best for streaming)  
  - **PostgreSQL** → Low throughput, high storage (best for persistence)

**Example:**  
Used in systems like **Zomato** for tracking delivery **live location updates** in real time.

---

## Core Concepts

### Topics
- A **topic** is a **logical partition** of messages in Kafka.
- All data produced to Kafka is categorized under topics.
- **Producers** specify the **topic** to which data should be sent.

---

### Partitions
- Each topic is divided into **partitions** for parallelism and scalability.
- **A consumer can consume from multiple partitions**,  
  but **a single partition’s messages are consumed by only one consumer within a group**.
- Messages within a partition are **ordered**.
- **Each partition has an offset**, representing the position of messages within that partition.  
  Kafka tracks the last committed offset to know how much data a consumer has processed.

---

### Brokers and Clusters
- A **broker** is a single Kafka server that stores topic data.
- A group of brokers forms a **Kafka cluster**.
- **Partitions** of a topic can be distributed across multiple brokers for load balancing.
- Each partition has:
  - A **leader** replica: handles all reads and writes for that partition.
  - One or more **followers**: replicate data for fault tolerance.
- If the leader fails, one of the followers is promoted as the new leader.

---

### Producer Data Flow
- Messages consist of **key**, **value**, **topic**, and **partition**.
- **Partition assignment logic:**
  1. If a **key** is provided → partition is chosen based on **key hashing**.
  2. If **partition** is manually provided → message is sent to that partition.
  3. If neither is provided → Kafka uses **round-robin** partition selection.
- This ensures balanced data distribution and message order for the same key.

---

### Consumer Groups
- Each **consumer** belongs to a **consumer group**.
- Consumer groups allow **load balancing** across multiple consumers.  
  Kafka ensures each partition is read by **only one consumer** in a group.
- Multiple consumer groups can subscribe to the same topic independently.
- Offsets for each consumer group are stored to maintain message processing state.

---

### Fault Tolerance
- Kafka replicates topic partitions to ensure **data availability**.
- If a broker fails, **replicas** take over seamlessly.
- **Zookeeper** (in older versions) or **Kafka’s internal quorum controller** maintains metadata and offset states.

---

### Message Delivery Handling
- If a consumer fails to process a message after the **maximum retry attempts**,  
  the message is moved to a **Dead Letter Queue (DLQ)** for later analysis.

---

### Push vs Pull Model
- **Kafka → Pull-based**  
  Consumers periodically **poll** brokers to fetch new messages.
- **RabbitMQ → Push-based**  
  Messages are **pushed** directly to consumers when they arrive.

---

## Usage Models

- **Queue model:**  
  Each message is processed by only one consumer (within a group).

- **Publish–Subscribe (Pub/Sub) model:**  
  Multiple consumer groups can consume the same messages independently.

---

## Summary

| Concept | Description |
|----------|-------------|
| **Topic** | Logical grouping of messages |
| **Partition** | Subdivision of a topic for parallelism |
| **Broker** | Kafka server storing partitions |
| **Cluster** | Group of brokers working together |
| **Producer** | Sends messages to a topic |
| **Consumer** | Reads messages from topic partitions |
| **Consumer Group** | Set of consumers sharing partitions |
| **Offset** | Position of the message in a partition |
| **Leader/Follower** | Replication roles for fault tolerance |

---

**In essence:**  
Kafka combines the best of both **queue** and **pub-sub** systems, enabling scalable, fault-tolerant, and real-time data streaming pipelines.

![[Pasted image 20251101184403.png]]
