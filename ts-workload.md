---

copyright:
  years: 2026
lastupdated: "2026-02-25"

keywords: troubleshooting for MongoDB, workload, logging, latency mean, disk latency

subcollection: databases-for-mongodb-gen2

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How can I measure a deployment's workload?
{: #troubleshoot-workload}
{: troubleshoot}
{: support}

[Gen 2]{: tag-purple}

You'd like to upgrade your {{site.data.keyword.databases-for-mongodb}} deployment, but you want to check if it's being used.
{: shortdesc}

You're ready to upgrade to a new major version, but you'd like to ensure that your deployment isn't under heavy workload.
{: tsSymptoms}

Check your [platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling){: external} within {{site.data.keyword.monitoringfull}}, specifically, [`ibm_databases_for_mongodb_disk_read_latency_mean`](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-monitoring#ibm_databases_for_mongodb_disk_read_latency_mean){: external}. Disk read latency mean is the time that it takes for MongoDB to read data from disk, measured in milliseconds. A low disk read latency indicates that MongoDB is able to read data from disk quickly. A high disk read latency indicates that MongoDB is taking a long time to read data from disk.
For more information, see [Monitoring Integration](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-monitoring){: external}.
{: tsResolve}
