---
title: An intriguing story about latencies
tags: [latency, linux]
style: fill
color: primary
description: An attempt to write a story about latencies
---

As the clock struck midnight, the networked servers were working tirelessly to process requests from around the world. But deep within the system, a problem was brewing.

The engineers had noticed a strange pattern in the tail latency, with some requests taking significantly longer than others to process. They knew that this could be detrimental to their clients' experience and could potentially lead to a loss of business.

Determined to solve the issue, they began investigating various factors that could be contributing to the problem. They eliminated background process interference, request re-ordering, and suboptimal interrupt routing, but still, the tail latency persisted.

As they delved deeper, they discovered that CPU power-saving mechanisms were causing delays in processing requests. Disabling these mechanisms led to a significant reduction in tail latency, but it also caused an increase in energy consumption.

The engineers were faced with a difficult decision - should they prioritize reducing tail latency or minimizing energy consumption? The answer was not clear-cut.

To make matters worse, as they scaled up beyond multiple cores on a single CPU to multiple CPUs, non-uniform memory access latency (NUMA) became an issue. This caused even more delays in processing requests and led to further increase in tail latency.

The engineers worked tirelessly day and night to find a solution. They tried out several techniques like dedicating cores entirely to server applications and configuring systems for interrupt processing. But still, they couldn't eliminate the problem entirely.

As time passed by without any resolution in sight, they began feeling increasingly frustrated and helpless. Would they ever be able to solve this problem? Or would it continue haunting them forever?

Finally, after months of trial and error, they stumbled upon a breakthrough - spatial scheduling. This involved partitioning CPU cores among applications instead of conventional time-sharing.

With this new approach, they were able to significantly reduce tail latency while minimizing energy consumption. The engineers rejoiced as their hard work finally paid off.

The networked servers were now running smoothly, and clients were experiencing faster response times. The engineers knew that they had overcome a significant challenge, one that would make them better equipped to tackle similar issues in the future.

## References
[Tales of the Tail](https://dl.acm.org/doi/10.1145/2670979.2670988)
