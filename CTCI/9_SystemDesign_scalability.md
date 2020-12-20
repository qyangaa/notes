# 9 System Design and Scalability

### Handling the questions

+ Communicate
+ Go broad first
+ Use whiteboard
+ Acknowledge interviewer concerns
+ Be careful about assumptions, and write down assumptions explicitly
+ Estimate when necessary
+ Drive

### Design

+ Scope the problem: 
  + major features and use cases (who, how)
  + constraints: 
    + traffic and data handling
    + frequency/ type of requests/ write/ read
    + special system requirements: multi-threading
+ Make assumptions: data size, number of users, frequency of use, allowance of time delay
+ Draw major components: with diagram. front-end server/ back-end data store/ server that crawl the Internet for data/ server that process analytics
+ Identify key issues: save time of access for frequently used data
+ Redesign for the key issues: be open about limitations

## Key Concepts

### Scaling

+ Horizontal/ vertical scaling:
  + Horizontal: add nodes (machines)
  + Vertical: increase resource of a node (higher memory/ CPU etc)

+ **Load Balancer:** 

### Data

+ Database Denormalization and NoSQL: 
  + Avoid: Joins in relational database, can be slow for large system.
  + Denormalization: add redundant (repeating) information to speed up reads
  + NoSQL:  does not support joins
+ Database partitioning (sharding): split data across machines
  + Vertical: by feature, issue: large feature cannot fit in one machine, needs to be partitioned further
  + Key-based (hash-based): `mod(key,n)`, issue: number of servers is fixed
  + Directory-based: maintain a lookup table. issue: table can give failure, accessing table impacts performance
+ Caching
  + Cache query & results, or object

### Asynchronous processing

+ Pre-process
+ Wait until process is done

+ 

#### Parallel Processing

+ **MapReduce**:  
  + `Map<key, value> map` `Reduce(map)`

### Networking

+ Bandwidth: max data transferred/ time (bits/second) 
+ Throughput: actual amount of data transferred/ time
+ Latency: delay between sending and receiving

### Considerations

+ Failures: plan for failure of different parts
+ Operational metrics
  + Availability: % time that system is operational
  + Reliability:  prob system is operational for certain unit of time
+ Read vs. write -heavy
  + Read heavy: cache
  + Write heavy: queuing up writes
+ Security


p149





p384