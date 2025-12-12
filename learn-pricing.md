---
copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: mongodb gen 2, pricing

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Pricing
{: #pricing}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

All instances of {{site.data.keyword.databases-for-mongodb}} deploy as highly available MongoDB clusters with three data members, with your data replicated on all members. Pricing is based on the total amount of disk storage, RAM, virtual CPU cores, and backup storage that is allocated to deployments, prorated hourly. Gen 2 {{site.data.keyword.databases-for-mongodb}} deployments have a minimum of 5 GB of disk and the smallest profile provides 4 vCPU cores.

## Using the pricing calculator
{: #pricing-calc}

For pricing estimation, use the **Add to estimate** button on the [{{site.data.keyword.databases-for-mongodb}} catalog page](https://cloud.ibm.com/databases/databases-for-mongodb/create){: external}. Input your total consumption across three data members into the calculator. This is roughly triple the size of your data because your data is replicated to all members. For example, 5 GB of disk with a 4 by 20 profile across three data members would be priced at 15 GB of disk and 12 vCPU, 60 GB of RAM respectively. 

## Gen 2 Backups pricing
{: #pricing-backup}

{{site.data.keyword.databases-for}} now uses a snapshot based backup model, with pricing aligned to the size of your provisioned database storage. Snapshots differ from traditional backups in that they are block-level incremental copies, therefore you are billed based on how much data has changed since the last snapshot, not just the total size of your database. 

By default, {{site.data.keyword.databases-for-mongodb}} provides a daily backup that is stored for 30 days. These backups, and any on-demand backups you make, all count toward the above allocation.

## Backup storage included
{: #pricing-backup-storage}

* You receive free backup storage equal to the total provisioned disk size of your deployment. 

* This includes both automated daily backups and manual (on-demand) snapshots.

* Example: If your 3 member {{site.data.keyword.databases-for-mongodb}} deployment is provisioned with 100 GB of disk per member, you get 300 GB of backup storage included at no cost.

Overage charges:

* The overage is billed monthly.

* Total snapshot storage = Day 1 full + (Daily change × 29 days x number of members)

* Overage is charged at $0.095 per GB per month.

Worked example, for a 3-member MongoDB deployment with 100 GB of data per member:

* Day 1: A full snapshot is taken. Each snapshot captures the full volume, so you consume 300 GB of snapshot storage (100 GB × 3).

This example models a worst-case scenario where the snapshot size equals the full file system, which is typically smaller than the disk volume. For new databases, the first snapshot is much smaller and grows over time, helping reduce backup costs.
{: note}

* Day 2: You write 10 GB of new data to each member. The next snapshot is incremental — it only stores the changes since the last snapshot. So you consume an additional 30 GB (10 GB × 3 members), bringing your total snapshot usage to 330 GB.

* Day 30: You write 2 GB of data per day *(--CONFIRM logic - needs clarity), bringing your total snapshot usage to ((2x28) + 330) = 386 GB

* In a given month, if you have a {{site.data.keyword.databases-for-mongodb}} deployment that has 100 GB of disk per member, and has three data members, you receive 300 GB of snapshot storage free for that month. Your backup storage utilization is greater than 300 GB for the month (in this scenario), you are charged an overage of $0.095/month per gigabyte, therefore your total bill = (386 GB - 300 GB) X 0.095 = $8.17.

* With large deployments and frequent writes, you’re more likely to exceed the free tier after the first snapshot, and your snapshot storage costs will grow quickly.

* Cross-region copies: If you choose to copy snapshots to another region, {{site.data.keyword.cloud}} charges for the full size of the snapshot in the destination region (not incremental) and continued incremental growth in the original region as new snapshots are taken.

## Scaling per member
{: #scaling-member}

{{site.data.keyword.databases-for-mongodb}} deployments have minimum and maximum allocation for disk and RAM as shown. Scaling deployments through the API and CLI provides more granularity and also allows a user to scale a database instance up to 4 TB of disk per member. Minimum and maximum CPU and RAM combinations vary per region, see [Isolated Compute](add link).(--will we have an isolated compute page -> yes, it already exists in the common docs).

| Resource | Minimum | Maximum | Scaling granularity (API/CLI) |
| ---------- | ----- | ----- | ------- |
| Disk | 5 GB per member | 4 TB per member | 1024 MB per member |
| RAM | 16 GB | 240 GB | Isolated Compute – Resource scaling via T-shirt sizes |
| CPU | 4 vCPU | 48 vCPU| Isolated Compute – Resource scaling via T-shirt sizes |
{: caption="Scaling limits" caption-side="top"}
