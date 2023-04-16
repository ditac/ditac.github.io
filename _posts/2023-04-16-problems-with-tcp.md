---
title: Problems with tcp
tags: [latency, tcp, network, load-balancing]
style: fill
color: warning
description: Problems with tcp in the datacenter
---

Stream orientation, which is one of TCP's key properties, is bad for load balancing because it does not guarantee complete message delivery and requires extra complexity and overheads. TCP applications must use inferior forms of load balancing, which can lead to hot spots and increased latency. Streaming data in ranges of bytes causes problems because it doesn't correspond to dispatchable units of work, which are messages. This means that threads can't process messages until they receive the entire message, and multiple messages received in a single read operation can only be processed one at a time. Additionally, streaming negatively impacts load balancing and induces head-of-line blocking, where short messages are delayed behind long messages.

In-order packet delivery restricts load balancing because it requires packets to be delivered in the order they were sent, which can cause delays if one packet is held up. This can lead to hot spots in both hardware and software, where certain nodes or threads are overloaded while others are underutilized. In contrast, a message-based protocol like Homa allows messages to be processed independently and out of order, enabling more efficient load balancing.

Long-lived connection state is undesirable in datacenter environments due to its high overheads in space and time. Maintaining connection state requires the server to allocate memory and CPU resources to store and manage the state for each peer. This can be a significant burden in large-scale datacenters with thousands or millions of connections. Additionally, establishing connections requires a setup phase before any data can be transmitted, which can't be effectively amortized in new serverless environments. Therefore, protocols like Homa, which are connectionless and don't require long-lived connection state, are better suited for datacenter transport.

Bandwidth sharing in TCP can impact the transmission of small messages. When multiple TCP connections share the available bandwidth, the bandwidth allocation is divided among the connections based on their congestion control mechanisms. This can result in small messages getting less bandwidth compared to larger messages, as the overhead involved in sending each packet can add up when sending many small packets, reducing the overall bandwidth available for data transmission. However, some TCP bandwidth sharing mechanisms aim to maintain a minimal and stable queue length at a bottleneck link while achieving full utilization of the link bandwidth, which can improve the overall performance of the network. Therefore, the impact of bandwidth sharing on small messages depends on the specific TCP bandwidth sharing mechanism used

Sender-driven congestion control has limitations as senders have no first-hand knowledge of congestion

Congestion control is limited by the fact that it can only detect congestion when there is buffer occupancy, leading to packet queueing and a trade-off between latency and throughput.

TCP's reliability guarantees are not suitable for applications that require round trip guarantees because TCP only guarantees in-order delivery of packets, not timely delivery. This means that packets may be delayed or lost, which can lead to retransmission and increased latency. Applications that require round trip guarantees need to know that their messages will be delivered within a certain time frame to ensure timely responses. TCP's reliability guarantees do not provide this level of assurance.

TCP's problems impact systems at multiple levels because its design decisions, such as its stream orientation and requirement of in-order packet delivery, are not suitable for modern datacenters. These problems affect the network by causing congestion and limiting bandwidth sharing, the kernel software by adding complexity and overheads to load balancing, and applications by restricting message throughput and inducing head-of-line blocking. TCP's limitations also impact hardware load balancing, causing hot spots and increased latency. Therefore, a new protocol like Homa is needed to address these issues and provide a better solution for datacenter computing.

### Solution
Homa solves the problems with TCP by using a message-based protocol that implements remote procedure calls (RPCs) and is connectionless. It avoids TCP's stream orientation, which is disastrous for software load balancing, by using explicit message boundaries. Homa manages congestion from the receiver, not the sender, which is better because the receiver has knowledge of all incoming messages. Homa can tolerate out-of-order packet arrivals, which provides more flexibility for load balancing. Additionally, Homa uses priority queues to eliminate the tradeoff between latency and bandwidth and takes advantage of modern switches' priority queues to ensure that shorter messages are favored over longer ones. Finally, Homa provides end-to-end reliability for RPCs and implements mechanisms such as flow control, retry, and congestion control using per-RPC state.


## References
[Its time to replace TCP](https://arxiv.org/abs/2210.00714)
