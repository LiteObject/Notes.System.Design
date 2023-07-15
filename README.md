# CHAPTER 1: SCALE FROM ZERO TO MILLIONS OF USERS

## Vertical scaling vs horizontal scaling
>Vertical scaling, referred to as “scale up”, means the process of adding more power (CPU,
RAM, etc.) to your servers. Horizontal scaling, referred to as “scale-out”, allows you to scale
by adding more servers into your pool of resources.

* Limitations of vertical scaling
  * Vertical scaling has a hard limit. It is impossible to add unlimited CPU and memory to a
single server
  * Vertical scaling does not have failover and redundancy. If one server goes down, the
website/app goes down with it completely
* Horizontal scaling is more desirable for large scale applications due to the limitations of
vertical scaling

## Database replication
>Database replication can be used in many database management
systems, usually with a master/slave relationship between the original (master) and the copies
(slaves)

* A master database generally only supports write operations. A slave database gets copies of
the data from the master database and only supports read operations

* Advantages of database replication:
  *  Better performance: In the master-slave model, all writes and updates happen in master
nodes; whereas, read operations are distributed across slave nodes. This model improves
performance because it allows more queries to be processed in parallel
  * Reliability: If one of your database servers is destroyed by a natural disaster, such as a
typhoon or an earthquake, data is still preserved. You do not need to worry about data loss
because data is replicated across multiple locations
  * High availability: By replicating data across different locations, your website remains in
operation even if a database is offline as you can access data stored in another database
server

## Cache
>A cache is a temporary storage area that stores the result of expensive responses or frequently
accessed data in memory so that subsequent requests are served more quickly
* Considerations for using cache
  *  Decide when to use cache. Consider using cache when data is read frequently but
modified infrequently
  * Expiration policy. It is a good practice to implement an expiration policy. Once cached
data is expired, it is removed from the cache. When there is no expiration policy, cached
data will be stored in the memory permanently. It is advisable not to make the expiration
date too short as this will cause the system to reload data from the database too frequently.
Meanwhile, it is advisable not to make the expiration date too long as the data can become
stale.
  * Consistency: This involves keeping the data store and the cache in sync. Inconsistency
can happen because data-modifying operations on the data store and cache are not in a
single transaction
  * Mitigating failures: A single cache server represents a potential single point of failure
(SPOF). As a result, multiple cache servers across different data centers are recommended to avoid SPOF
  * Eviction Policy: Once the cache is full, any requests to add items to the cache might
cause existing items to be removed. This is called cache eviction. Least-recently-used
(LRU) is the most popular cache eviction policy. Other eviction policies, such as the Least
Frequently Used (LFU) or First in First Out (FIFO), can be adopted to satisfy different use
cases  

## Content delivery network (CDN)
* A CDN is a network of geographically dispersed servers used to deliver static content. CDN
servers cache static content like images, videos, CSS, JavaScript files, etc.
* **Considerations of using a CDN**
  * Cost
  * Appropriate cache expiration
  * CDN fallback
  * Invalidating content
  
## Stateful vs Stateless Architecture

## geoDNS-routed, aka. geo-routed
* geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user
* Some technical challenges in multi-data center setup:
  * Traffic redirection: Effective tools are needed to direct traffic to the correct data center.
GeoDNS can be used to direct traffic to the nearest data center depending on where a user
is located
  * Data synchronization: Users from different regions could use different local databases or
caches. In failover cases, traffic might be routed to a data center where data is unavailable.
A common strategy is to replicate data across multiple data centers. A previous study
shows how Netflix implements asynchronous multi-data center replication
  * Test and deployment: With multi-data center setup, it is important to test your
website/application at different locations. Automated deployment tools are vital to keep
services consistent through all the data centers
---
### To further scale our system, we need to decouple different components of the system so they can be scaled independently. Messaging queue is a key strategy employed by many real-world distributed systems to solve this problem
---
## Message queue
* A message queue is a durable component, stored in memory, that supports asynchronous communication
* Decoupling makes the message queue a preferred architecture for building a scalable and reliable application

## Logging, mertics, monitoring
* Logs are important because they help to identify errors and problems
in the system
* Collecting different types of metrics help us to gain business insights and understand
the health status of the system
  * Host level metrics: CPU, Memory, disk I/O, etc.
  * Aggregated level metrics: For example, the performance of the entire database tier, cache
tier, etc.
  *  Key business metrics: daily active users, retention, revenue, etc.

##  Database scaling
There are two broad approaches for database scaling: vertical scaling and horizontal scaling.

### Vertical scaling
Vertical scaling, also known as scaling up, is the scaling by adding more power (CPU, RAM, DISK, etc.) to an existing machine.

Vertical scaling comes with some serious drawbacks:
- You can add more CPU, RAM, etc. to your database server, but there are hardware limits. If you have a large user base, a single server is not enough.
-  Greater risk of single point of failures.
- The overall cost of vertical scaling is high. Powerful servers are much more expensive.

### Horizontal scaling
Horizontal scaling, also known as sharding, is the practice of adding more servers.

Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.

Anytime you access data, a hash function is used to find the
corresponding shard.

The most important factor to consider when implementing a sharding strategy is the choice of the sharding key. Sharding key (known as a partition key) consists of one or more columns that determine how data is distributed.

A sharding key allows you to retrieve and modify data efficiently by routing database queries to the correct database.

When choosing a sharding key, one of the most important criteria is to choose a key that can evenly distributed data.

Sharding is a great technique to scale the database but it is far from a perfect solution. It introduces complexities and new challenges to the system:

- __Resharding data__: Resharding data is needed when 
  - 1: A single shard could no longer hold more data due to rapid growth. 
  - 2: Certain shards might experience shard exhaustion faster than others due to uneven data distribution. When shard exhaustion happens, it requiresupdating the sharding function and moving data around.

- __Celebrity problem__: This is also called a hotspot key problem. Excessive access to a specific shard could cause server overload.

- __Join and de-normalization__: Once a database has been sharded across multiple servers, it is hard to perform join operations across database shards. A common workaround is to de-normalize the database so that queries can be performed in a single table.

## Summary of how we scale system to support millions of users:
- Keep web tier stateless
- Build redundancy at every tier
- Cache data as much as you can
- Support multiple data centers
- Host static assets in CDN
- Scale your data tier by sharding
- Split tiers into individual services
- Monitor your system and use automation tools


# CHAPTER 2: BACK-OF-THE-ENVELOPE ESTIMATION






