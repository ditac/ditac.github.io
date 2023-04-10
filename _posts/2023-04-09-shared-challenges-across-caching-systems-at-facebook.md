---
title: Shared Challenges across caching systems
tags: [caching, Facebook, workload]
style: fill
color: primary
description: Shared Challenges across caching systems
---

Caching systems play a critical role in improving the performance of large-scale applications by reducing the amount of time needed to access frequently used data. However, designing and managing caching systems for massive data sets can be challenging due to issues such as object size variability, short bursts of activity, computationally costly queries for empty results, efficient cache updates and frequent restarts. In this article, we will discuss the shared challenges that caching systems face when dealing with massive data sets.

### Massive data sets
Dealing with massive data sets is hard because the more data there is, the larger the cache needed to store popular data. Popularity of data can change rapidly, making it difficult to keep track of what is popular and what isn't. The high amount of churn in popular data complicates caching policies that rely on past access patterns. Additionally, the popularity distribution tends to be highly skewed, with a small number of objects receiving a large portion of requests. All these factors make it challenging to efficiently manage and cache massive working sets.

### Small objects
The size of objects in a cache system can greatly affect its performance. Different use cases have different size distributions, with some having mainly small objects and others having larger chunks. This can make it challenging to design a caching system that efficiently uses memory and storage space. Existing systems that store only one object per cache line may waste space for systems with many small objects. Additionally, in-memory data structures used as an index for objects on flash can take up a lot of memory, which is especially problematic for systems with many small objects. Overall, it is important to handle object size variability while minimizing memory and storage overhead in caching systems.
The size of objects in a cache system can greatly affect its performance. Different use cases have different size distributions, with some having mainly small objects and others having larger chunks. This can make it challenging to design a caching system that efficiently uses memory and storage space. Existing systems that store only one object per cache line may waste space for systems with many small objects. Additionally, in-memory data structures used as an index for objects on flash can take up a lot of memory, which is especially problematic for systems with many small objects. Overall, it is important to handle object size variability while minimizing memory and storage overhead in caching systems.

### Short bursts
Facebook's traffic is not consistent and has sharp bursts of activity, making it challenging to provision caching systems with enough resources to maintain low latencies. To be efficient, caching systems should use all available resources without exceeding them, especially DRAM. However, as the workload changes, memory available for caching can vary widely. That's why CacheLib dynamically allocates and frees memory used by the cache to avoid crashes caused by memory over-commitment or the kernel's out-of-memory killer, while still using all available resources efficiently.

### Empty results
Another challenge that Facebook faces with caching is the issue of computationally costly queries for empty results. This occurs frequently in database systems that track associations between users, where a user might query for the set of acquaintances they have in common with another user. Such queries are typically computationally costly for the backend database, and if empty results are not cached, it would significantly lower the cache hit ratio.

### Efficient cache updates
Updating cached data structures is also a challenge that Facebook faces with caching. Caches should efficiently support structured data, especially for in-process caches that directly interact with application data structures. Applications often want the ability to update specific fields in a cached data structure without deserializing, updating, and re-serializing the object.

### Frequent restarts
Finally, frequent restarts pose a challenge for production caches at Facebook. Most caching systems are transient, meaning that their content is lost upon application restart. Transience is problematic because large caches take a long time to warm up. It would take hours or even days for cache hit ratios to stabilize following the restart of a transient cache at Facebook. Prior work has suggested the alternative of warming a cache after restart, but this requires an explicit warm-up phase as part of routing or requires slow deployments.

In conclusion, caching systems are essential in improving the performance of large-scale applications by reducing the time needed to access frequently used data. However, designing and managing caching systems for massive data sets can be challenging due to issues such as object size variability, short bursts of activity, computationally costly queries for empty results, efficient cache updates, and frequent restarts. Addressing these shared challenges is crucial for building efficient and reliable caching systems that can handle the demands of modern applications. By finding solutions to these challenges, developers can create caching systems that provide fast access to critical data while optimizing resources and minimizing downtime.


## References
[CacheLib](https://www.usenix.org/conference/osdi20/presentation/berg)
