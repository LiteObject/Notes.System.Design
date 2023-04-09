# Notes: System Design

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

## Content delivery network (CDN)
* A CDN is a network of geographically dispersed servers used to deliver static content. CDN
servers cache static content like images, videos, CSS, JavaScript files, etc.
* **Considerations of using a CDN**
  * Cost
  * Appropriate cache expiration
  * CDN fallback
  * Invalidating content
## Logging, mertics, monitoring
* Logs are important because they help to identify errors and problems
in the system
* Collecting different types of metrics help us to gain business insights and understand
the health status of the system
  * Host level metrics: CPU, Memory, disk I/O, etc.
  * Aggregated level metrics: For example, the performance of the entire database tier, cache
tier, etc.
  *  Key business metrics: daily active users, retention, revenue, etc.
