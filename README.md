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
5. Scaling your abstract design
- Vertical scaling
  - You scale by adding more power (CPU, RAM)
- Horizaontal scaling
  - You scale by adding more machines into your pool (such as, Load balacing)
- Caching
  - Vastly better use of the resources already have
  - Application Chacing: Explict integration in the Application code itself
  - Database cahcing: tends to be "free?". When you flip your database on, your're going to get some level of default configutation which will provide some degree of caching and performance.
  - In-memory chache: are most potent in therms of raw performance. This is beacuse they store their entire set of data in memory and accesses to RAM are orderes of magnitude faster than those to disk. (eg, memcached or redis)
  - eg, Precalcuating results. The number of visits from each reffering domain for the previous day
  - eg, pre-generating expensive indexes. Suggested stories based on a user's click history
  - eg, stroing copies of frequently accessed data in a faster backend. Memchace instead of PostrgreSQL
- Load Balancing
  - Public servers of a scalable web service are hidden behind a load balancer. This load balancer evenly distributes load (requests from your users) onto your group/cluster of application servers.
  - Type
    - Smart client: hard to get it perfect
    - Hardware load balancers: $$$ but reliable
    - Software load balancers: hybrid - works for most systems
- Database replication
  - frequent electroning copying data from a database in one computer or server to a another so that all users share the same level of information --> distributed database --> Users can access data relavant to their tasks without interfering with the work of others
  - Normalization: eliminating data ambigutiy, inconsistency among users
- Map-Reduce
- Platform Lyaer (services)

## Key Topics
1. Concurrency
  - Threads, Deadlock, Starvation
  - Parallelize Algorithms
  - Consistency and Coherence
2. Networking
  - IPC, TCP/IP
  - Throughput and Latency
3. Abstraction
  - OS
  - File System
  - Database
  - Various level of caching in a modern OS
4. Real-World Performance
  - performance of RAM, disk, SSD, and Network
5. Estimation
  - Back-of-them-envope calculation 
6. Avaiability & Reliability
  - How to design a system to cope with network failures
  - Durability?

## Web App System design considerations
- Security
- CDN: Content Delivery Network. System of Distributed Servers deliver webpages and web content based on the geographic locations. The closer the CDN server is the user geographically, the faster the content will be delivered to the user. Provide protection from large surges in traffic.
- Full Text Search: Using Sphinx/Lucene/Solr which achieve fast search responses because, instead of seraching the text directly, it seraches an index instead.
- Offine support / Progressive enhancement
- Web workers
- Server side rendering
- Asynchronous loading of assets (lazy load items)
- Minimizing network requests (Http2 + Bundling / Sprites etc)
- Developer productivity / Tooling
- Accessibility
- Internationalization
- Resopnsive design
- Browser compatibility

## Working Components of Front-End Architecture
- Code
  - HTML5/WAI-ARIA
  - CSS/Sass Code standards
  - Object-Oriented approach
  - JS framewroks/organization/performance optimization tech
  - Asset Delivery - Front end Ops
- Documentation
  - Onboarding Docs
  - Styleguide/Pattern Lib
  - Architecture Diagrams (code flow, tool chain)
- Testing
  - Performance Testing
  - Visual Regression
  - Unit Testing
  - End to End testing
- Process
  - Git workflow
  - Dependency Management (npm, bundler, bower)
  - Build Systems (Grunt / Gulp)
  - Deploy Process
  - Continuous Integration (Travis CI, Jenkins)
 

# Grokking System Design Interview

## Interview Process
### Scope the problem
- Don't make assumptions
- Ask clarifying questions to understand the constrints and use caes
- Steps
  - Requirements clarifications
  - System interface definition
  
### Sketch up an abstraction design
- Building blocks of the system
- Relationship between them
- Steps
  - Back-of-the-envelope estimation
  - Defining data model
  - High-level design

### Identify and address the bottlenecks
- Use the fundamental principles of scalable system design
- Steps
  - Detailed design
  - Identifying and resolving bottlenecks
<br><br><br><br>



## Distrbuted System Design Basics

### Key Characterics

#### Scalability
- ex) Scalable system for more user
- The capability of a system to grow and manage increased demand.
- Scalable system is which can continuously evolve to support growing
- Horizontal / Vertical scaling
  - Horizontal Scaling: by adding more servers into the pool of resources 
  - Vertical Scaling: by adding more resource (CPU, RAM, storage, etc) to an existing server. (downtime and an upper limit)

#### Reliability
ex) Reliable system even if server downs
- Reliability is the probability that a system will fail in a given period
- A distributed system is reliable if it keeps delivering its service even when one or multiple components fail
- Reliability is achieved through redundancy of components and data (remove every singple point of failure)

#### Availability
ex) Available system whenever user request
- Availability is the time a system remains operational to perform its required function in a specific period.
- Measured by the percentage of time that a system remians operational under normal condtions.
- Reliable system is available. But not vice-versa. It can be security hole.

#### Efficiency
ex) Fast response
- Latency: response time, the delay to obtain the first piece of data.
- Bandwidth: throughput, amount of data delivered in a given time.
<br><br><br>

#### Serviceability / Manageability
ex) Easy for System Update or Repaire
- Easiness to operate and maintain the system
- Simplicity and spend with which a system can be repaired or maintained


### Load Balancer / Load Balancing (LB)
Help Scale Horizaontally across an ever-increasing number of servers.

#### LB locations
- Between user and web server
- Between web server and an internal platform layer (application servers, cache servers)
- Between internal platform layer and DB

#### Algorithm
- Least connection
- Least response time
- Least bandwidth
- Round robin
- Weighted round robin
- IP Hash

#### Implementation
- Smart clients (Hard to be perfect)
- Hardware load balancers (Expensive)
- Software load balancers (Hybrid. Most use)
<br><br><br>



### Caching
Take advantage of the locality of reference principle
- recently requested data is likely to be requested again.
Exist at all levels in architecture, but often found at the level nearest to the front end.

#### Application server cache
- Cache placed on a request layer node
- When a request layer node is expanded to many nodes
  - Problem
    - LB randomly distrbutes requests across the nodes
    - The same request can go to diffrent nodes
    - Incrase cache misses
  - Solution
    - Global caches
    - Distrbuted caches

#### Distributed cache
- Each request layer node owns part of the cached data
- Entire cache is divided up using a consistent hashing function
- Pros
  - Cache space can be increased easily by adding more nodes to the request pool
- Cons
  - A missing node leads to cache lost.

#### Global Cache
- A server of file store that is faster than original store, and accessible by all request layer nodes.
- Two common forms
  - Cache server handles cache miss
    - Used by most applications
  - Request nodes handle cache miss
    - Have a large percentage of the hot data set in the cache.
    - An architecture where the files stored in the cache are static and shouldn't be evicted.
    - The application logic understadns the eviction strategy or hot spots better than the cache

#### Content distributed network (CDN)
- For sites serving large amounts of static media
- Process
  - A request first asks the CDN for a piece of static media
  - CDN serves that content if it has it locally available.
  - If content isn't available, CDN will query back-end servers for the file, cache it locallly and serve it to the requesting user.
- If the system is not large enough for CDN, it can be built like this:
  - Serving static media off a separate subdomain using lightweight HTTP server (e.g Nginx)
  - Cutover the DNS from this subdomain to a CDN later

#### Cache invalidation
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

#### Cache eviction policies
- FIFO: first in first out
- LIFO: last in first out
- LRU: least recently used
- MRU: most recently used
- LFU: least frequently used
- RR: random replacement
<br><br><br>



### Sharding & Data Partitioning

#### Partitioning methods
- Horizontal partitioning
  - Range based sharing

- Vertical partitioning
  - Divide data for a specific feature to their own server

- Directory-based partitioning
  - A lookup service that knows the partitioning scheme and abstracts it away from the database access code.

#### Partitioning criteria
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

#### Common problmes of sharding
Most of the constraints are due to the fact that operations across multiple tables or mlultiple rows in the same table will no longer run on the same server.
- Joins and denormalization
- Referential integrity
- Rebalancing
<br><br><br>



### Indexes
- Improve the performance of search queries
- Decrease the write performance. This performance degradation appiles to all insert, update, and delete operations.
<br><br><br>



### Proxies
A proxy server is an intermediary piece of hardware/software sitting between client and backend server
- Filter requests
- Log requests
- Transform requests (encryption, compression, etc)
- Cache
- Batch requests
  - Collapsed forwarding: enable multiple client requests for the same URI to be processed as one request to the backend server
  - Collpase requests for data that is spatially close together in the storage to minimize the reads
<br><br><br>



### Queues
Queues are used to effectively manage requests in a large-scale distributed sysetm, in which diffrent components of the system may need to work in an asynchronous way
- It is an abstraction between the client's request and the actual work performed to service it.
- Queues are implemente on the asynchronious comunication protocol. When a client submits a task to a queue they are no longer required to wait for the results
- Queue can provide protection from service outages and failures
<br><br><br>



### Redundancy
duplication of critical data or services withthe intension of increased reliability of the system
<br><br><br>


### SQL vs. NoSQL

#### NoSQL

##### Key-Value stores
- Redis, Vodemort, Dynamo

##### Document databases
- MongoDB, MongoDB

##### Wide-column / columnar databases
- Cassandra, HBase

##### Graph database
- Neo4J, InfiniteGraph

#### Diffrent between SQL and NoSQL
##### Storage
##### Schema
##### Querying
##### Scalability
##### ACID
- Atomicity, consistency, isolation, durability

#### Which one to use?

##### SQL
- Ensure ACID compliance
- Data is structured and unchanging

##### NoSQL
- Data has little or no structure.
- Make the most of cloud computing and storage
- Rapid development
<br><br><br>



### CAP Theorem
<br><br><br>

### Consistent Hashing
<br><br><br>

### Client Server Communication
<br><br><br><br>


## Sysetm Designs Examples
