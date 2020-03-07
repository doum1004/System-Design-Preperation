# System-Design-Preperation
## References
- https://www.educative.io/courses/grokking-the-system-design-interview
- https://github.com/lei-hsia/grokking-system-design
<br><br><br><br>

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


### Global Cache

### Content distributed network (CDN)

### Cache invalidation

### Cache eviction policies
