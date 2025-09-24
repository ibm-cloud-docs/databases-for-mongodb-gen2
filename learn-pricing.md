---
copyright:
  years: 2019, 2025
lastupdated: "2025-09-24"

keyowrds: mongodb, databases, pricing, scaling, resources

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}

All instances of {{site.data.keyword.databases-for-mongodb_full}} deploy as highly available MongoDB clusters with three data members. Your data is replicated on all members. 

Both the Standard Plan and the Enterprise Plan are priced based on the total amount of disk storage, RAM, and backup storage that is allocated to deployments, prorated hourly. 

Minimum deployment sizes depend on the hosting model used. For more information, see [hosting models](/docs/databases-for-mongodb?topic=databases-for-mongodb-hosting-models&interface=api).

## Shared Compute
{: #pricing-shared-compute}

{{site.data.keyword.databases-for-mongodb}} Standard Plan deployments have a minimum of 10 GB of disk, 4 GB of RAM and 2 CPU cores per data member.
{{site.data.keyword.databases-for-mongodb}} Enterprise Plan deployments are not available on Shared Compute.

## Isolated Compute
{: #pricing-isolated-compute}

{{site.data.keyword.databases-for-mongodb}} Standard Plan deployments have a minimum of 10 GB of disk, 16 GB of RAM and 4 CPU cores per data member.
{{site.data.keyword.databases-for-mongodb}} Enterprise Plan deployments have a minimum of 20 GB of disk, 16 GB of RAM and 4 CPU cores per data member.

A {{site.data.keyword.databases-for-mongodb}} EE Analytics node (only available on the Enterprise Plan) is priced as an additional data member.

## Using the pricing calculator
{: #mongodb-price-calc}

Templates are provided for ease of use and provide balanced resource allocations appropriate for general-purpose workloads. The **Custom** tab can be used to configure Disk, RAM, and vCPU, as desired.

For combined pricing estimation, you can use the **Add to estimate** button at the bottom of the [{{site.data.keyword.databases-for-mongodb}} catalog page](https://cloud.ibm.com/databases/databases-for-mongodb/create){: external}.

## Backups pricing
{: #pricing-backup}

You receive your total disk space purchased, per database, in free backup storage. For example, in a given month, if you have a {{site.data.keyword.databases-for-mongodb}} deployment that has 20 GB of disk per member, you receive 60 GB of backup storage free for that month (because all deployments have three members). If your backup storage utilization is greater than 60 GB for the month (in this scenario), you are charged an overage of $0.03/ month per gigabyte. 

By default, {{site.data.keyword.databases-for}} provides a daily backup that is stored for 30 days. These backups, and any on-demand backups you make, all count toward the allocation.

In the example, if your database contains 2 GB of data and you have not taken any on-demand backups, then your total backup size is 2 GB x 30 = 60 GB. Your backup costs are nil.

If your database contains 15 GB of data and you have not taken any on-demand backups, your total backup size is 15 GB x 30 = 450 GB. In this scenario, your backup costs are (450 GB - 60 GB) * 0.03 = $11.7 per month.

Most deployments will never go over the allotted credit.

## Scaling per member
{: #mongodb-scale-member}

{{site.data.keyword.databases-for-mongodb}} deployments have allocations for disk, CPU and RAM that are set by you at provisioning time. RAM and CPU can be scaled up and down depending on your workload needs. Disk cannot be scaled down, only up, to protect the integrity of your data. For more information on how to scale your resources, see [Scaling disk, memory, and CPU](/docs/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling).

