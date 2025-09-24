---
copyright:
  years: 2024, 2025
lastupdated: "2025-09-24"

keywords: mongodb ee, mongodb enterprise, mongodb enterprise edition, mongodb enterprise plan

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Plan overview
{: #mongodb-plans}

{{site.data.keyword.databases-for}} offers two plans for MongoDB: **{{site.data.keyword.databases-for-mongodb_full}} Standard** and **{{site.data.keyword.databases-for-mongodb_full}} Enterprise**. Both plans provide managed MongoDB deployments with built-in high availability, scaling, and security.  

The choice between the two depends on your specific needs, security requirements, and budget.

## Feature comparison
{: #mongodb-plans-features}

| Feature | Standard plan | Enterprise plan |
|---------|---------------|-----------------|
| **Edition** | Fully managed MongoDB Community Edition | Fully managed MongoDB Enterprise Edition |
| **Hosting model** | Available on Shared or Isolated compute | Available only on Isolated compute |
| **Ops Manager** | Not included | Included. Provides monitoring, configuration, backup, automation, and security tooling |
| **Backups** | Daily automated snapshots for disaster recovery. Manual, on-demand backups also supported | Continuous, incremental backups with [Point-in-Time Recovery](#mongodbee-pitr) for up to 7 days. |
| **Restores** | Standard restore from backups | Faster restore performance from incremental snapshots and PITR |
| **Monitoring** | Basic host and database metrics (CPU, memory, ops/sec) through [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) integration | Ops Manager dashboards with real-time and historical views, query profiler, automated index recommendations, custom alerts, plus [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) integration |
| **Auditing** | Event auditing through IBM Cloud Activity Tracker | IBM Cloud Activity Tracker plus granular auditing of schema changes, authentication events, and CRUD operations via Ops Manager |
{: caption="Feature comparison" caption-side="top"}

## Choosing a plan
{: #mongodb-plans-choosing}

- Choose the **Standard plan** for general-purpose workloads that need managed MongoDB with automated backups and a flexible hosting model.  
- Choose the **Enterprise plan** for workloads that require Ops Manager, Point-in-Time Recovery, advanced monitoring, or detailed auditing to meet compliance and regulatory needs.  
