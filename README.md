# System-Design-Preperation
## References
- https://www.educative.io/courses/grokking-the-system-design-interview
- https://gist.github.com/vasanthk/485d1c25737e8e72759f
- https://github.com/lei-hsia/grokking-system-design
<br><br><br><br>

# System Design Cheatsheet
> Picking the right architecture = right battles + managing trade-offs

## basic steps
1. Clarify and agree on the scope of the system
- User cases
  - Who is going to use?
  - How are they going to use?
- Constrains
  - Identify traffic and data handling constraints at scale
  - Scale of the system. Such as requests per second, requests types, data written per second, data read per second
  - Special system requirements. Such as multi-threading, read or write oriented
2. High level architecture design (Abstract design)
- Sketch the importatnt components and connections between. (No details)
  - Application service layer (serves the requests) (Load balancer)
  - List diffrent services (service partition)
  - Data storage layer (master/slave db cluster)
  - Cacheing System
3. Component Design
- Component + Specific APIs required
- Object Oriented Design for functionalities
  - Map features to modeuls: One scenario for one module
  - Consider the relationships among modules
    - Cerntain functions must have unique instance (singletons)
    - Core object can be made up of many other objects (compositions)
    - One object is another object )inheritance)
- Database schema design.
4. Understanding Bottlenecks
- a load balancer or other to handle user requests? downside?
- Distribute database on multiple machines for large data? downside?
- Mem cahcing for database performance? downside?







# Preview
## Scope the problem
- Steps
  - Requirements clarifications
  - System interface definition
- Ask clarifying questions to understand the constraints and use cases
- Don't make assumptions
  
## Sketch up an abstraction design
- Steps
  - Back-of-the-envelope estimation
  - Defining data model
  - High-level design
- Building blocks of the system
- Relationship between them

## Identify and address the bottlenecks
- Steps
  - Detailed design
  - Identifying and resolving bottlenecks
- Use the fundamental principles of scalable system design
<br><br><br><br>




# Distrbuted System Design Basics

## Key Characterics

### Scalability
- ex) Scalable system for more user
- The capability of a system to grow and manage increased demand.
- Scalable system is which can continuously evolve to support growing
- Horizontal / Vertical scaling
  - Horizontal Scaling: by adding more servers into the pool of resources 
  - Vertical Scaling: by adding more resource (CPU, RAM, storage, etc) to an existing server. (downtime and an upper limit)

### Reliability
ex) Reliable system even if server downs
- Reliability is the probability that a system will fail in a given period
- A distributed system is reliable if it keeps delivering its service even when one or multiple components fail
- Reliability is achieved through redundancy of components and data (remove every singple point of failure)

### Availability
ex) Available system whenever user request
- Availability is the time a system remains operational to perform its required function in a specific period.
- Measured by the percentage of time that a system remians operational under normal condtions.
- Reliable system is available. But not vice-versa. It can be security hole.

### Efficiency
ex) Fast response
- Latency: response time, the delay to obtain the first piece of data.
- Bandwidth: throughput, amount of data delivered in a given time.
<br><br><br>



## Load Balancer / Load Balancing (LB)
Help Scale Horizaontally across an ever-increasing number of servers.

### LB locations
- Between user and web server
- Between web server and an internal platform layer (application servers, cache servers)
- Between internal platform layer and DB

### Algorithm
- Round robin
- Weighted round robin
- IP Hash
- Least connection
- Least response time
- Least bandwidth
### Implementation
- Smart clients
- Hardware load balancers
- Software load balancers
<br><br><br>



## Caching
Take advantage of the locality of reference principle
- recently requested data is likely to be requested again.
Exist at all levels in architecture, but often found at the level nearest to the front end.

### Application server cache
- Cache placed on a request layer node
- When a request layer node is expanded to many nodes
  - Problem
    - LB randomly distrbutes requests across the nodes
    - The same request can go to diffrent nodes
    - Incrase cache misses
  - Solution
    - Global caches
    - Distrbuted caches

### Distributed cache
- Each request layer node owns part of the cached data
- Entire cache is divided up using a consistent hashing function
- Pros
  - Cache space can be increased easily by adding more nodes to the request pool
- Cons
  - A missing node leads to cache lost.

### Global Cache
- A server of file store that is faster than original store, and accessible by all request layer nodes.
- Two common forms
  - Cache server handles cache miss
    - Used by most applications
  - Request nodes handle cache miss
    - Have a large percentage of the hot data set in the cache.
    - An architecture where the files stored in the cache are static and shouldn't be evicted.
    - The application logic understadns the eviction strategy or hot spots better than the cache

### Content distributed network (CDN)
- For sites serving large amounts of static media
- Process
  - A request first asks the CDN for a piece of static media
  - CDN serves that content if it has it locally available.
  - If content isn't available, CDN will query back-end servers for the file, cache it locallly and serve it to the requesting user.
- If the system is not large enough for CDN, it can be built like this:
  - Serving static media off a separate subdomain using lightweight HTTP server (e.g Nginx)
  - Cutover the DNS from this subdomain to a CDN later

### Cache invalidation
- keep cache coherent with the source of truth. Invalidate cache when source of truth has changed.
- Write-through cache
  - Data is written into the cache and permanent storage at the same time.
  - Pro
    - Fast Retrieval, complete data consistency, robust to system distruptions
  - Con
    - Higher latency for write operations
- Write-around cache
  - Data is written to permanent storage, not cache
  - Pro
    - Reduce the cache that is no used.
  - Con
    - Query for recently written data creates a cache miss and higher latency.
- Write-back cache
  - Data is only written to cache.
  - Write to the permanent storage is done later on
  - Pro
    - Low latency, high throughput for write-intensive applications
  - Con
    - Risk of data loss in case of system disruptions.

### Cache eviction policies
- FIFO: first in first out
- LIFO: last in first out
- LRU: least recently used
- MRU: most recently used
- LFU: least frequently used
- RR: random replacement
<br><br><br>



## Sharding & Data Partitioning

### Partitioning methods
- Horizontal partitioning
  - Range based sharing

- Vertical partitioning
  - Divide data for a specific feature to their own server

- Directory-based partitioning
  - A lookup service that knows the partitioning scheme and abstracts it away from the database access code.

### Partitioning criteria
- Key or hash-based partitioning
  - Apply a hash function to some key attribute of the entry to get the partition number
  
- List partitioining
  - Each partition is assigned a list of values

- Round-robin partitioning
  - With n partitions, the i tuple is assigned to partition i % n

- Composite partitioning
  - Combine any of above partitioning schemes to devise a new scheme.
  - Consistent hashing is a composite of hash and list partitioning.
    - Key -> reduced key space through hash -> list -> partition.

### Common problmes of sharding
Most of the constraints are due to the fact that operations across multiple tables or mlultiple rows in the same table will no longer run on the same server.
- Joins and denormalization
- Referential integrity
- Rebalancing
<br><br><br>



## Indexes
- Improve the performance of search queries
- Decrease the write performance. This performance degradation appiles to all insert, update, and delete operations.
<br><br><br>



## Proxies
A proxy server is an intermediary piece of hardware/software sitting between client and backend server
- Filter requests
- Log requests
- Transform requests (encryption, compression, etc)
- Cache
- Batch requests
  - Collapsed forwarding: enable multiple client requests for the same URI to be processed as one request to the backend server
  - Collpase requests for data that is spatially close together in the storage to minimize the reads
<br><br><br>



## Queues
Queues are used to effectively manage requests in a large-scale distributed sysetm, in which diffrent components of the system may need to work in an asynchronous way
- It is an abstraction between the client's request and the actual work performed to service it.
- Queues are implemente on the asynchronious comunication protocol. When a client submits a task to a queue they are no longer required to wait for the results
- Queue can provide protection from service outages and failures
<br><br><br>



## Redundancy
duplication of critical data or services withthe intension of increased reliability of the system
<br><br><br>


## SQL vs. NoSQL

### NoSQL

#### Key-Value stores
- Redis, Vodemort, Dynamo

#### Document databases
- MongoDB, MongoDB

#### Wide-column / columnar databases
- Cassandra, HBase

#### Graph database
- Neo4J, InfiniteGraph

### Diffrent between SQL and NoSQL
#### Storage
#### Schema
#### Querying
#### Scalability
#### ACID
- Atomicity, consistency, isolation, durability

### Which one to use?

#### SQL
- Ensure ACID compliance
- Data is structured and unchanging

#### NoSQL
- Data has little or no structure.
- Make the most of cloud computing and storage
- Rapid development
<br><br><br>



## CAP Theorem
<br><br><br>

## Consistent Hashing
<br><br><br>

## Client Server Communication
<br><br><br><br>


# Sysetm Designs Examples
