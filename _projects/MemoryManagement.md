---
name: Memory Management
tools: [aws, elasticsearch]
description: Manage memory more efficiently in Elasticsearch
---

I lead a project to improve memory management in Elasticsearch and make
clusters more stable across the board. This is currently a rough draft of some
of the work that I did on the project.
 
### Memory
By default Elasticsearch uses the CMS garbage collector and is tuned for
low-latency.

#### Why fixed heap size?
When configuring the JVM to run Elasticsearch, the default sets the min and max
heap size to the same value. This avoids having to request memory from the
operating system on-demand and improves tail latency. The garbage collector
often resorts to a full GC cycle when resizing the heap and a fixed heap size
avoids this all together. On the flip side, this forces a fixed memory
allocation between the operating system and the JVM. This is fine on small
instances but on large instances with 100+GB of main memory we need to be more
intelligent on how to share memory between the operating system and the JVM. 

The JVM has a memory optimization below 32GB that allows it to use smaller
sized pointers when the heap size is below 32GB. On larger heaps, the JVM uses
a bigger pointer size to be able to address all pages. From our experiments we
found that we dont really get any real benefits by increasing the heap size
from 32GB to 40GB. As Lucene is optimized to use the Linux page cache for most
caching needs, keeping a small heap was beneficial.

One of the first things that I did as part of this project was to mentor
a summer in Roy Elzur to work on off-heap memory allocation in Lucene. One of
my colleagues, Min Zhou had come up with a prototype based on off-heap
allocation in Spark. Roy polished these changes and also built a controller
that would move allocations between on-heap and off-heap data structures based
on memory utilization. 

Apache Lucene uses memory-mapped files to offload memory from the JVM heap to
the operating system page cache. I had an idea to utilize memory-mapped files
instead of the off-heap data structures that Min had used in his prototype. Roy
reached out to Mike Mccandless who is very knowledgeable about Lucene and he
really liked the idea. But, we ran out of time and couldnt implement the
changes. In the winter of 2018 one of my teammates Ankit Jain, reached out to
me for some cool ideas to work on during the Christmas break. I told him about
the memory mapped files and he was able to implement it within 2 weeks. This
was a major feature in Lucene/Elasticsearch as it helped reduce memory
utilization of large indexes by a big factor!


### Detection
- JMX metrics around number of GC cycles and the memory released.

### Remediation
- Dynamically increase heap size to 128GB when required.

### Small instances
- By default the CMS collector allocates 64MB per core. On instance types like
  r5.large you have 2 cores but 16GB of memory. Out of the 16GB, 8GB was left
for the page cache and 8GB was allocated to JVM. A 128MB young gen on 8GB heap
was inefficient and moving to a larger 1GB young gen improved performance by
more than 30%.

### Top consumers
In addition to detecting that we are running out of heap space, we needed a way
to take action. The easiest way to do that was to find the top consumers and
see if we can reduce memory utilization in any of them. We investigated heap
profiles of mulitple production clusters that were running into memory pressure
and came up with the following data structures that could be tweaked.

#### Caches
Elasticsearch came with a bunch of on-heap caches to speed up query access.
Each of these caches had interesting behavior that I will talk about in the
following section.

#### Field data cache
The field data cache is used by Elasticsearch to store document to term
mappings in memory. This can be used for certain aggregations. By default this
cache is unlimited and was one of the main reasons for high memory pressure in
Elasticsearch. The circuit breakers are configured would stop expensive queries
but did not prevent slow accumulation of the cache. Once loaded, elements in
the cache are never released regardless of whether they are used again or not.
Dropping the field data cache in case of high utilization and letting customers
know which of their queries was resulting in high cache utilization can help
improve Elasticsearch stability. Since the field data cache is per field, we
can easily drop unused fields when under memory pressure.

#### Shard Request cache
The shard request cache, caches local results of a shard. This cache is useless
incase of indices that are being updated consistently as the whole cache is
invalidated on a new write. This cache works well in use-cases with immutable
indices like logging data. In logging data, indices are typically created per
hour or day and old indices are not actively written to. In these use-cases the
cache is useful and can have a high hit-rate. In production this cache was not
frequently used, but there were some small number of clusters where we could
evict entries to free up memory.

#### Node Query cache
Elasticsearch uses Lucene's query cache to cache results. Lucene has a weird
cache implementation where it stores one hash-map per lucene segment. The idea
here is to drop the entire cache when a lucene segment is deleted. But, this in
turn makes the cache slow and complicated. I wrote a simple caching wrapper on
top of Caffeine which was one to two orders of magnitude faster than the Lucene
query cache. The problem though is that most of the time in a Lucene query is
spent in ranking the results and the speed-up in cache performance does not
make a noticeable difference to the overall query latency. 

The other main problem with the Lucene query cache is that it's accounting is
incorrect. The accounting assumes that each entry in the cache is a fixed size
and is thus incorrect when large query objects are stored in the cache. To
avoid this scenario, in newer versions of Lucene large query objects are not
cached at all. To mitigate the incorrect accounting impact, we can dynamically
drop parts of the cache when we see high memory utilization.

#### Queues
Different requests in Elasticsearch like search, index etc have their own queue
for processing. These queues have fixed defaults and are not sized based on
memory, throughput or latency requirements. Instead of a fixed default, we
chose to dynamically increase the size of the queue if there were rejections
and decrease the size incase of memory pressure.

#### Utilization based shard balancing
This feature looked at memory utilization per shard and tried to rebalance
shards across nodes once a day. 

#### Future improvements
Cache aware replica selection. Link to google paper.

### Insights
- There are no good defaults. Workload dependent.
- CMS default for large instance types does not work well for small ones.
- Fixed size heap is not a good idea for all workloads, you will end up leaving
  a lot of memory on the table.
- Trading off CPU for memory always seems to be a good trade-off as memory is
  more expensive than CPU currently.
