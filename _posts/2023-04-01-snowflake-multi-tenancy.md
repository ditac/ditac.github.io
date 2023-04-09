---
title: Why Snowflake needs Multi-tenancy
tags: [snowflake, datawarehouse, architecture, caching]
style: fill
color: warning
description: How snowflake can improve by supporting multi-tenancy
---

Snowflake, the cloud-based data warehouse, has been providing dedicated virtual warehouses to its customers for a long time. However, with the growing demand for fine-grained billing and better resource utilization, it is time for Snowflake to move towards multi-tenancy.

The current model of dedicated virtual warehouses is good but not ideal for resource utilization. Customers are paying for resources they might not need all the time. This often leads to overprovisioning and wastage of resources. With multi-tenancy, customers can share resources and pay only for what they use. This will lead to better resource utilization and cost savings.

The move towards per-second billing has made it even more important to move towards multi-tenancy. Earlier, customers who used the warm pool of instances in the last 5 minutes of an hour paid for the whole hour. With per-second billing, nobody pays for the warm pool of instances. Multi-tenancy will ensure that resources are shared and billed only for what is used.

Another issue with dedicated virtual warehouses is the high skew in memory usage. Memory is often provisioned based on peak usage, which leads to underutilization during off-peak hours. With multi-tenancy, memory can be shared based on demand, leading to better resource utilization.

Scaling up before demand is also a challenge with dedicated virtual warehouses. It is hard to predict demand and scale up accordingly, which often leads to overprovisioning or underprovisioning of resources. With multi-tenancy, resources can be scaled up or down based on demand from multiple customers at once.

Lastly, multi-tenant caching is hard with Snowflake's current design. If one customer causes a scale-up, a new node comes up with a cold cache and affects other customers' performance. This is one of the key challenges that needs to be addressed before moving to a multi-tenant design.

In conclusion, moving towards multi-tenancy is the logical next step for Snowflake. It will lead to better resource utilization, cost savings, and improved performance for all customers. With the growing demand for fine-grained billing and better resource utilization, it is time for Snowflake to embrace multi-tenancy.


## References
[Snowflake Elastic Query Engine](https://www.usenix.org/system/files/nsdi20-paper-vuppalapati.pdf)
