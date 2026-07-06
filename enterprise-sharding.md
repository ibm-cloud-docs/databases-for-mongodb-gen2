---
copyright:

  years: 2026
lastupdated: "2026-07-01"

keywords: databases, mongodbee, Enterprise Edition, sharding, horizontal scaling, gen2, plan consolidation

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Sharding
{: #enterprise-sharding}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition on Gen 2 provides a unified plan that starts with one shard by default and allows you to add shards as your workload grows, enabling horizontal scaling by distributing data across multiple machines.
{: shortdesc}

## Plan consolidation on Gen 2
{: #enterprise-sharding-plan-consolidation}

On Gen 2, the Enterprise Edition plan consolidates the previous Gen 1 Enterprise and Enterprise Sharding plans into a single offering. All Enterprise Edition deployments start with one shard (3 members), which is equivalent to the basic Enterprise Edition on Gen 1. As your needs grow, you can add up to 5 additional shards for a total of 6 shards (18 members), providing a seamless scaling path without requiring migration to a different plan.



## Benefits of horizontal scaling with sharding
{: #enterprise-sharding-benefits}

Sharding provides a future-proof architecture that grows with your application:

- **Break through vertical scaling limits** – When adding more CPUs or RAM to a single cluster reaches physical or practical limits, sharding distributes your data across multiple machines. {{site.data.keyword.databases-for-mongodb}} supports up to 4 TB per shard, allowing a maximum of 24 TB across 6 shards.

- **Increase throughput for reads and writes** – Horizontal scaling adds cluster nodes, distributing the load across multiple shards instead of overloading a single node. Each shard handles a portion of your workload, enabling higher overall throughput.

- **Optimize query performance** – By choosing appropriate shard keys, you can keep related documents together while evenly distributing the rest of the data. This reduces cross-shard traffic for common queries and prevents hot spots, improving query isolation and performance across large collections.

- **Scale incrementally** – Start with one shard and add shards only when needed, paying for additional infrastructure as your workload demands it rather than over-provisioning upfront.

## When to add shards
{: #enterprise-sharding-when-to-use}

Consider adding shards to your Enterprise Edition deployment when:

- Your data size approaches 2 TB and vertical scaling is no longer sufficient.
- Your workload requires higher read and write throughput than a single shard can provide.
- You need to distribute data logically for performance optimization.
- Your application architecture benefits from data partitioning based on shard keys.

## Sharding considerations
{: #enterprise-sharding-considerations}

Sharding is a shared responsibility between {{site.data.keyword.databases-for}} and you as the customer. {{site.data.keyword.databases-for}} provisions and manages the infrastructure, while you are responsible for the application-level sharding configuration.

### Your responsibilities
{: #enterprise-sharding-customer-responsibilities}

When you add shards to your deployment, you must:

- **Enable sharding for each collection** – Collections must be individually configured for sharding. Unsharded collections remain on a single shard, which can create unbalanced nodes that fill up before others and cause operational problems.

- **Select appropriate shard keys** – Choose shard keys carefully to distribute data evenly and avoid shard overloading. Use high-cardinality keys to ensure balanced distribution and reduce scatter/gather overhead.

- **Optimize queries** – Design queries to minimize scatter and gather operations, which query multiple shards and are costly in both time and resources.

- **Plan for additional costs** – Additional infrastructure incurs additional cost. For more information, see [Pricing](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-pricing).

For more information, see [Responsibilities for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-responsibilities-cloud-databases).
{: tip}

### Technical specifications
{: #enterprise-sharding-specifications}

- **Default configuration**: 1 shard (3 members)
- **Maximum cluster size**: 6 shards × 3 members (18 members total)
- **Storage per shard**: Maximum of 4 TB (2 TB recommended)
- **Total storage capacity**: Up to 24 TB across all shards
- **Scaling direction**: Shards can only be added, not removed. Plan your scaling strategy in advance.

### Migration and adoption
{: #enterprise-sharding-migration}

Existing single-shard deployments do not require migration to adopt multi-shard architecture. Simply add shards to your deployment and enable sharding for your databases and collections. However, plan your shard key strategy carefully before enabling sharding, as shard keys cannot be changed after they are set.

## Next steps
{: #enterprise-sharding-next-steps}

* Review [Pricing](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-pricing) to understand the cost implications of adding shards.

* Plan your shard key strategy before enabling sharding on your collections.
* For more information, see [MongoDB's official documentation on shard key selection and sharding best practices](https://www.mongodb.com/docs/manual/core/sharding-choose-a-shard-key/).{: external}
