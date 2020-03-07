# System-Design-Preperation
## References
- https://www.educative.io/courses/grokking-the-system-design-interview
- https://github.com/lei-hsia/grokking-system-design

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




# Distrbuted System Design Basics

## Key Characterics

### Scalability
- The capability of a system to grow and manage increased demand.
- Scalable system is which can continuously evolve to support growing
- Horizontal / Vertical scaling
  - Horizontal Scaling: by adding more servers into the pool of resources 
  - Vertical Scaling: by adding more resource (CPU, RAM, storage, etc) to an existing server. (downtime and an upper limit)

### Reliability

### Availability

### Efficiency



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



##
