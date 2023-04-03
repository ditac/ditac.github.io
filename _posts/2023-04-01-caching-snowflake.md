---
title: Caching in Snowflake
tags: [snowflake, datawarehouse, architecture, caching]
style: fill
color: warning
description: How caching works in the snowflake data warehouse
---

Snowflake's design maximizes query performance by utilizing both persistent and ephemeral storage. The persistent store is located on S3, while the ephemeral storage is located on SSD. Despite having only 0.1% of the size of persistent storage, it has an 80% cache hit ratio. Queries are split into multiple compute tasks and scheduled on nodes where necessary persistent data is cached, reducing the amount of data that needs to be transferred between nodes.

The workload on Snowflake data warehouse is diverse and primarily focused on analytical tasks. The majority of the queries, accounting for most of the workload, are read-write, indicating that users are performing both analytical tasks and modifying data in the warehouse. However, a significant portion of users (28%) are solely reading data for analysis and exploration purposes. Additionally, 13% of the queries are write-only, suggesting that users are leveraging Snowflake to load large amounts of data into the warehouse for future analysis. 

Snowflake partitions tables horizontally into large, immutable files that are equivalent to blocks in traditional database systems. Data is stored in compressed blocks, which reduces storage space and improves query performance. Within each file, values of each column or attribute are grouped together and compressed using PAX encoding. This reduces I/O operations during query processing by minimizing the amount of data that needs to be read from disk.

Snowflake's data warehousing system uses a write-through cache to ensure consistency between the cache and the main storage. This means that any updates or changes made to data in the cache are immediately propagated to the persistent storage on S3. Ephemeral storage acts as a write-through cache and is partitioned on file-name.

Intermediate data generated during queries is stored on SSD, further optimizing query performance. However, when tasks are spread across many nodes, a lot of intermediate data has to be transferred across nodes which can present a challenge for Snowflake's design. Compute resources are not multi-tenant and are reserved per warehouse. To mitigate this challenge, Snowflake uses a pool of resources to immediately assign compute resources when required.

Overall, Snowflake's design prioritizes speed and efficiency in query processing through its innovative use of both persistent and ephemeral storage and intelligent task scheduling techniques. By utilizing a write-through cache and horizontal partitioning with PAX encoding, Snowflake minimizes I/O operations during query processing while ensuring consistency between the cache and main storage. Intermediary data is stored on SSD to further optimize query performance, and a pool of resources is employed to quickly assign compute resources when required.
