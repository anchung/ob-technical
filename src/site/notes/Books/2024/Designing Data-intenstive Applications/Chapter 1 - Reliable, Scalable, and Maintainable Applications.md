---
{"dg-publish":true,"permalink":"/books/2024/designing-data-intenstive-applications/chapter-1-reliable-scalable-and-maintainable-applications/","tags":["Data"]}
---

- Store data so that they, or other applications, can find it again later (*DataBase*)
- Store data so that they, or other applications, can find it again later (*DataBase*)
- Remember the result of an expensive operation, to speed up reads (*Caches*)
- Allow users to search data by keyword or filter in various ways (*search indexes*)
- Send a message to another process, to be handled asynchronously (*stream processing*)
- Periodically crunch a large amount of accumulated data (*batch processing*)

Many tools are optimized for a variety of different use cases, and they no long neatly fit into traditional categories (database/caches/queues):
> **Redis** - datastores  / message queues
> **Apache Kafka** - message queues with durability guaranteed

Different tools are being stitched together by application code to handle wide-ranging requirements and demands.
> Developing applications to handle caches and indexes held by *Memcache* and *Elasticsearch*, for example

![Pasted image 20240709143832.png](/img/user/Pasted%20image%2020240709143832.png)

## Reliability
- Continuously "Work Correctly"
- When adversity happens (*Fault*), a system has to be **Fault-tolerant** or **resilient**.
* Prevent *faults* from causing *failures*

### Hardware Faults
- Redundancy to individual components
	- Disk - RAID configuration
	- Server - Dual PSUs; hot-swappable CPUs
- Services/Components on AWS, for example, are designed to prioritize flexibility and elasticity over single-machine reliability
### Software Errors
- Quick, easy recovery
- Set up detailed and clear monitoring: performance metrics and error rates - **telemetry**
- Good management practices and training
## Scalability
- A term that is used to describe a system's ability to cope with increased **load**.
- **Load** can be described by **Load Parameters** and it's based on the architecture of a system
- Twitter solutions (page 12)
- Describe **Performance**
	- When increasing a *load parameter* and keep the system resources unchanged, how is the performance of a system affected
	- When increasing a *load  parameter*, how much do we need to increase the system resources in order to keep the performance unchanged
- **Latency** and **Response Time**
- **Median** and **Percentile**
- Algorithms to calculate a good approximation of percentiles at minimal CPU and memory cost - *[[forward decay\|forward decay]]*, *[[t-digest\|t-digest]]*, or *[[HdrHistogram\|HdrHistogram]]*

## Maintainability
Three design principles for software systems:
- Operability
- Simplicity
- Evolvability - also known as *extensibility*, *modifiability*, or *plasticity*




