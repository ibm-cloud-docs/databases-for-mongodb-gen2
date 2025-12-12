---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: mongodb, databases, monitoring, scaling, autoscaling, resources, WiredTiger

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Performance
{: #performance}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

{{site.data.keyword.databases-for-mongodb_full}} deployments can be manually scaled to your usage. If you are tuning the performance of your deployment, consider a few factors first.

## Monitoring your deployment
{: #monitor-deployment}

{{site.data.keyword.databases-for-mongodb}} deployments offer an integration with the [{{site.data.keyword.monitoringfull}} service](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-monitoring) for basic monitoring of resource usage, like disk usage and IOPS of your deployment. Observing trends in your usage can help alleviate performance problems before your databases become unstable due to resource exhaustion.

## Disk Usage
{: #disk-usage}

If you are concerned about how much space MongoDB is using to store your data, you can run some native MongoDB [data storage diagnostics](https://docs.mongodb.com/manual/faq/storage/#data-storage-diagnostics){: .external} to find the sizes of things like databases, collections, and indexes. If the approximate size of your data set is known and fixed, you can manually scale your disk to accommodate your data.

## Disk I/O
{: #disk-iops}

The number of Input-Output Operations per second (IOPS) on {{site.data.keyword.databases-for-mongodb}} deployments is limited by the type of storage volume. Storage volumes for {{site.data.keyword.databases-for-mongodb}} deployments are [Block storage endurance volumes in the 5 IOPS per GB tier](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#block-storage-profile-overview){: .external}. Hitting I/O utilization limits can cause your databases to respond slowly or appear unresponsive. Things like unoptimized queries, [index building](https://docs.mongodb.com/manual/core/index-creation/){: .external}, and creating new indexes can cause spikes in IOPS, but it's also possible that normal work loads for your applications can exceed the available IOPS for your deployment.

You can increase the number IOPS available to your deployment by increasing disk space. 

For more information, see the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#how-frequently-does-wiredtiger-write-to-disk){: .external}.

## WiredTiger Cache and Memory
{: #wiredtiger-cache-memory}

{{site.data.keyword.databases-for-mongodb}} uses the [WiredTiger storage engine](https://docs.mongodb.com/manual/core/wiredtiger/#memory-use){: .external}, which uses both the file system memory cache and an internal memory cache. MongoDB is most performant when it serves your data from its internal cache, a little less performant when the data is in the file system cache, and least performant when it has to grab your data from disk.

The default size of the internal cache is `50% of (total RAM per member - 1 GB)` or `256 MB`, whichever is larger. For example, the minimum memory size of a {{site.data.keyword.databases-for-mongodb}} deployment is 4096 MB per data member, so the internal cache is 1536 MB (because `0.5 * (4096 MB -  1024 MB) = 1536 MB`.

The internal/file system cache ratio is not user-configurable on your deployment, but you can scale the total amount of memory to adjust the internal cache to make your database more performant. For example, if you scale the memory to 5120 MB per member the internal cache size becomes 2048 MB. `0.5 * (5120 MB - 1024 MB) = 2048 MB.`

Another way to use autoscaling is to set memory to scale when disk I/O utilization hits a certain threshold. Increasing memory decreases the amount that MongoDB reads or writes to disk, so additional memory might alleviate pressure on disk I/O by supporting more caching.

More information about the WiredTiger cache is in the [MongoDB documentation](https://docs.mongodb.com/manual/faq/storage/#to-what-size-should-i-set-the-wiredtiger-internal-cache){: .external}.

## Query Performance
{: #query-performance}

The MongoDB documentation has multiple resources on query performance, including a how-to on [analyzing query performance](https://docs.mongodb.com/manual/tutorial/analyze-query-plan/){: .external}. Once you have a general idea on how your queries perform, they also have tips on [optimizing your queries](https://docs.mongodb.com/manual/core/query-optimization/){: .external}.

As a more advanced topic, you can learn how MongoDB [manages query plans](https://docs.mongodb.com/manual/core/query-plans/){: .external}.

## Other MongoDB Monitoring Tools
{: #other-monitor}

You can also take advantage of some of the native MongoDB monitoring functions. For example, you can use both [`mongotop`](https://docs.mongodb.com/manual/reference/program/mongotop/#bin.mongotop){: .external} and [`mongostat`](https://docs.mongodb.com/manual/reference/program/mongostat/#bin.mongostat){: .external}.

```sh
mongotop 30 --username admin --password $PASSWORD --tls --tlsCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712, host2.databases.appdomain.cloud:31712

mongostat -n 20 1 --username admin --password $PASSWORD --tls --tlsCAFile $CERTFILE --authenticationDatabase admin --host host1.databases.appdomain.cloud:31712,host2.databases.appdomain.cloud:31712 --json
```

Run any of the [documented commands](https://docs.mongodb.com/manual/administration/monitoring/#commands){: .external} that report on the status of your MongoDB database.

Many of the MongoDB utilities and commands need the [Cluster Monitor](https://www.mongodb.com/docs/manual/reference/built-in-roles/#mongodb-authrole-clusterMonitor){: .external} role to execute. It is not part of the `admin` default role set. [Grant the Cluster Monitor role](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management#the-admin-user) to the `admin` user on your deployment.
{: .tip}
