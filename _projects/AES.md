---
name: Amazon Elasticsearch Service
tools: [aws, elasticsearch, opendistro]
image: https://raw.githubusercontent.com/ditac/ditac.github.io/master/assets/aes.png
---

I worked with a group of three other engineers to build Amazon Elasticsearch
Service. We were able to build a minimal implementation and on-board private
beta customers in 6 months. Some of the intersting features that I would like
highlight are -
1.  Online upgrades and the ability to change instance types of a cluster with
    no impact.
2.  Networking design and integration with the load-balancer.
3.  Security.
4.  Infrastructure provisioning - multi-az, multi-region, master/data nodes
    etc.

I will highlight some of the features and design choices that we made in each
of these areas.

### Moving between instance types
AWS offers multiple instance type configurations with different ratios of CPU,
RAM and storage. Elasticsearch workloads can be CPU or IO bound and offering
all possible configurations made it easy to onboard customers running their
existing workloads on EC2 or on-prem into the service.

The biggest challenge of supporting multiple instance types was
around letting customers migrate between them. Many cloud services allow you to
choose an instance type for a cluster but dont not have the flexibility to move
between them without downtime.

#### Workflow
The workflow to create a cluster was simple and incorporated the following
steps.

1. Verify that the workload can fit in the new instance type, otherwise reject
   the request.
3. Add new instances requested by the customer to the fleet.
4. Add new instances to the load-balancer and start sending traffic to the new
   instances.
5. If a new instance does not have a shard a query is interested in, it will
   fetch the necessary data from the node with the data.
6. Debar old instances and stop placing new shards in the old instance.
7. Copy data from the old instances to the new instance.
8. Remove old instances from the load-balancer.

#### Pros
1. We could move between any two instance types.
2. We never had any down-time and the system could always serve traffic.
3. There provisioned capacity of the cluster never went below what was
   requested. We always added additional capacity during migration.

#### Cons
1. On large instances, a large amount of data had to be copied across the
   network and it took a long time.
2. The cache was completely invalidated on the new instance type.
3. Workload verification was not fool-proof and there were cases where the new
   workload could not fit on the requested instance type and the migration
failed.

In other projects, I have come across different designs. The most popular one
is compute, storage separation via a separate storage service like Amazon EBS.
In this design, you can detach the storage volume and attach it to the new
instance. The problem with this approach is that you end up with reduced
capacity when you detach the storage volume from the old instance to the new
one.

An optimization is to bring up the new instance in a read-only mode so that it
can catch up to the old one, warm up its cache and then switch from the old to
the new. This is easy to achieve with a purpose built storage engine like
Amazon Aurora, but should also be possible with EBS now that it supports
multi-attach.

One interesting implementation improvement that I worked on was around debaring
instance types. In the initial implementation, we used IP addresses to debar
instances. While this was a quick implementation it had a few defects - 
1. Hardware failures could lead to instance replacements. Now, the new instance
   would come up with a brand new IP addresses. Hence the code had to
constantly monitor the IP addresses of all the instances in the cluster.
2. Theoretically, there was nothing that prevented a new instance from coming
   up with the same IP address as the old one. This would confuse the algorithm
and move data back and forth.

To avoid these edge cases, I introduced a mechanism where all instance types
were tagged with a generation number corresponding to the configuration
requested by the customers. The workflow debarred an entire generation and only
cared about the instance type tag. The tag was allotted at instance creation by
EC2 auto-scaling and did not change for the lifetime of the instance.

### Networking design
All instances sat behind a single Amazon ELB(Elastic Load Balancer) fleet. The
load-balancer monitored the health of each instance via a health-check and only
forwarded traffic to healthy instances. The health-check was smart and did not
report healthy till requests could be successfully processed by the instance.
The health-check was run every 5 seconds by the ELB and if the instance ran
into issues (for example if the ES process died), the load-balancer stopped
sending traffic to it till the instance recovered.

### Security
We implemented authentication and authorization using the AWS signature V4
algorithm. This algorithm works on HTTP. The customer had to create an access
key and a secret key in their AWS account and use those keys for creating
a unique signature for each HTTP request. This signature is then validated at
the server to establish the identity of the requester. 

In addition to authentication, we implemented authorization mechanism that
allowed administrators of the cluster to set rules on who could access the
cluster. Since the signature was for each HTTP request, the administrator was
allowed to associate permissions with URL paths. Once the identity of a request
was established, the web-server on the Elasticsearch cluster fetched the
permissions associated with that cluster and validated if the requester was
allowed to access the resource. 

### Instance roles
Elasticsearch nodes are not all the same, and had to be provisioned differently
depending on their role.
1. Master node - These nodes were responsible for light-weight cluster wide
   operations. We provisioned 3 master-eligible nodes spread across different
availability zones.
2. Zone aware clusters - For production grade clusters, we provisioned nodes
   across two different availability zones. We had to go with two availability
zones and not three as not all AWS regions at the time supported 3 zones. This
has changed with time and the service today supports three zones. Three zones
are critical for availability and durability of any database.

### Impact
The Elasticsearch service was a huge hit in AWS and is one of the fastest
growing services at AWS since 2015. You can measure impact in many different
ways, but if I were to measure the number of times the code that I have written
has been executed or the sheer number of hosts on which it has been deployed,
I dont think any feature that I have implemented since will come close.
