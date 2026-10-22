---

copyright:
  years: 2026
lastupdated: "2026-10-22"

keywords: mongodb gen 2 differences, mongodb gen 2 limitations, cloud databases gen 2 mongod, gen 1 vs gen 2

subcollection: databases-for-mongodb-gen2

content-type: reference

---

{{site.data.keyword.attribute-definition-list}}

# Architectural differences and feature parity roadmap
{: #mongodb-gen1-vs-gen2-differences-feature-parity}

[Gen 2]{: tag-purple}

Building upon the [Overview of Gen 1 (Classic) and Gen 2 (VPC)](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2), this reference details the functional comparison between {{site.data.keyword.databases-for-mongodb}} Generation 1 (Classic) and Generation 2 (VPC).
{: shortdesc}

To help you design, provision, and migrate workloads effectively, this guide is organized into two distinct sections:

- **[Architectural differences](#gen1-vs-gen2-architectural-differences)**: Permanent platform decisions, foundational shifts, and database engine design constraints that define how Gen 2 operates.
- **[Feature parity](#gen1-vs-gen2-feature-parity)**: Temporary feature parity gaps where capabilities supported in Gen 1 are actively being implemented for Gen 2 in upcoming releases.

## Architectural differences
{: #gen1-vs-gen2-architectural-differences}

Architectural differences represent permanent design changes in Gen 2. Gen 2 is built on software-defined {{site.data.keyword.cloud}} VPC infrastructure, a zero-trust security architecture, and predictable compute performance.

| Feature or area | Gen 1 (Classic) | Gen 2 (VPC) | Gen 2 architecture details |
| --- | --- | --- | --- |
| **Network endpoints** | Public endpoints, Classic Service Endpoints (CSE), and Virtual Private Endpoints (VPE). | Virtual Private Endpoints only (VPE). | Gen 2 is private networking only. All client connections route through Virtual Private Endpoints (VPE), eliminating support for public internet access and aligning with a zero-trust networking model. |
| **Service provisioning and API** | [Custom Cloud Databases (`cdb`) API](/docs/apis/cloud-databases-api/cloud-databases-api-v5). | [{{site.data.keyword.cloud_notm}} Resource Controller API](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-api). | Lifecycle management is standardized directly onto the core {{site.data.keyword.cloud_notm}} Resource Controller and Resource Manager APIs, unifying resource tagging, IAM governance, and deployment orchestration. |
| **Hosting models** | Shared compute and isolated compute. | Isolated compute only. | No sources for noisy-neighbor contention on Gen 2 by deploying all instances on single-tenant compute with guaranteed vCPU, memory, and I/O allocations. |
| **Resource scaling** | Fine-grained, linear resource scaling. | Standardized VPC compute flavors. | Standardized host profiles optimize container placement, hypervisor scheduling, and memory-to-vCPU ratios. |
| **Backup architecture** | Backups operate at file and application level; backup duration and I/O impact scale with data size. | Backups based on VPC storage block-level snapshots. | The new architecture decouples backup operations from database compute; volume snapshots complete near-instantly with zero operational overhead on active transactions. |
| **TLS and SSL certificates** | Certificates signed by the internal IBM Cloud Database Certificate Authority. | Certificates signed by a public Certificate Authority (Let's Encrypt). | Standardizes on industry-standard CA signatures, simplifying automated certificate rotation and client trust store management. |
| **Administrative access** | Single `admin` database user provisioned by IBM. | Dynamic database Manager role by using service credentials. | Removes fixed administrative credentials with IAM-governed, service credentials. |
| **Supported major versions** | MongoDB v7.0 to v8.0. | MongoDB v8.0 (and future major versions). | Gen 2 launches on the current MongoDB major version. Support for additional (newer) major versions is planned. |
| **Plan changes** | {{site.data.keyword.databases-for-mongodb}} Gen 1 offers a Standard plan, an Enterprise Edition plan, and a Sharding plan.| {{site.data.keyword.databases-for-mongodb}} Gen 2 offers a Standard Plan and a Sharding plan (starting with a single shard). | {{site.data.keyword.databases-for-mongodb}} Gen 2 has a simplified plan structure. Existing Enterprise Edition deployments are migrated to the Sharding plan and continue to use a single shard. |
{: caption="Architectural differences between {{site.data.keyword.databases-for-mongodb}} Gen 1 and Gen 2" caption-side="bottom"}

## Feature parity roadmap
{: #gen1-vs-gen2-feature-parity}

The following list represents capabilities available in Gen 1 that are not yet available in Gen 2. These capabilities are actively being addressed as the service progresses toward full feature parity.

| Capability | Gen 2 status | Notes and workarounds |
| --- | --- | --- |
| **{{site.data.keyword.databases-for}} CLI plug-in (`ibmcloud cdb`)**  \n CLI plug-in for managing {{site.data.keyword.databases-for}} instances. | Future release | The `cdb` CLI plug-in operates on Gen 1 instances only. Dedicated Gen 2 CLI plug-in commands are planned in a future release.  \n **Workaround:** Manage instances by using `ibmcloud resource` CLI commands, the Gen 2 REST APIs, or the {{site.data.keyword.cloud_notm}} Terraform provider. |
| **Point-in-time recovery (PITR)**  \n Granular recovery to any transaction point using continuous oplog replay. | Future release | PITR will be available on the [Enterprise Sharding plan](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-mongodb-plans) using continuous, incremental backups with configurable recovery history (minimum 7 days).  \n **Workaround for Standard plan:** Restore from the latest automated or on-demand VPC storage snapshot. |
| **Autoscaling**  \n Automatic threshold-based scaling of compute and storage resources. | Future release | Automatic threshold-based scaling for compute and storage resources is planned in a future release.  \n **Workaround:** Manually scale compute and disk allocations by using the {{site.data.keyword.cloud_notm}} CLI, API, or Terraform. |
| **Cross-region backup replication**  \n Automated replication of backup snapshots to a secondary region for disaster recovery. | [Available in select regions](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-independent-backups&interface=ui#independent-backups-crc) | Gen 1 backup storage is distributed across multiple regions by default. In Gen 2, cross-region backup replication is customer configurable and lets you choose into which worldwide regions backup copies are stored. This feature provides greater flexibility to align with your resilience, compliance, and data locality requirements. |
| **Small scale database instances**  \n Support for lowest price vCPU hosting models. | Future release | Cost-optimized deployment (nano) profiles that provide a lower-cost alternative for smaller workloads while maintaining the benefits of the Gen 2 architecture. Nano profiles replace the Gen 1 shared hosting model with dedicated Gen 2 deployments that offer greater flexibility and isolation.  |
| **{{site.data.keyword.keymanagementservicelong}}: Key Ring scope**  \n Scoping IAM Service-to-Service authorization policies to a Key Ring. | Future release | IAM Service-to-Service (S2S) policies restricted to a Key Ring return `422 Unprocessable Entity`.  \n **Workaround:** Scope the S2S authorization policy to the {{site.data.keyword.keymanagementserviceshort}} instance or a single Key CRN. |
| **{{site.data.keyword.keymanagementserviceshort}}: Resource group scope**  \n Scoping IAM Service-to-Service authorization policies to a resource group. | Future release | The Gen 2 provisioning broker does not evaluate broad resource group-scoped S2S policies.  \n **Workaround:** Scope the S2S policy directly to the specific {{site.data.keyword.keymanagementserviceshort}} instance. |
| **Read replicas**  \n Dedicated read replicas for offloading read queries or disaster recovery (available cross-region or locally).  | Future release | Dedicated cross-region and local read-only replicas for query offloading are planned in a future release. **Workaround:** Scale horizontally by adding internal cluster data members to distribute read traffic. |
{: caption="Feature parity for {{site.data.keyword.databases-for-mongodb}} Gen 2" caption-side="bottom"}

## Detailed domain breakdown
{: #gen1-vs-gen2-domain-details}

### Security, encryption, and IAM
{: #security-iam-differences}

- **Customer-managed encryption (BYOK):** Root encryption keys must be configured at initial instance provisioning. Attaching a BYOK key to an existing provider-managed database without reprovisioning is not supported on either generation.
- **IAM S2S policy scoping:** To avoid `422 Unprocessable Entity` errors during Gen 2 provisioning, ensure your IAM Service-to-Service authorization policy targets the {{site.data.keyword.keymanagementserviceshort}} service instance or a single Key CRN. Scoping by Key Ring or resource group is not supported in Gen 2.
- **HPCS support:** If your workload requires encryption keys managed through {{site.data.keyword.hscrypto}}, use [{{site.data.keyword.keymanagementserviceshort}} on Gen 2](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-key-protect&interface=ui).
- **Private network isolation:** Gen 2 instances do not support public internet routing. All database traffic must originate from within an authorized {{site.data.keyword.cloud_notm}} VPC or over Direct Link or Transit Gateway connected to the instance's VPE.

### Hosting, compute, and scaling
{: #hosting-compute-differences}

- **Dedicated resources:** Gen 2 runs exclusively on isolated compute profiles, providing dedicated vCPU, memory, and IOPS allocations without shared multitenant overhead.
- **Scaling operations:** While automated scaling policies (Autoscaling) are in development for Gen 2, instances can be scaled dynamically by using the {{site.data.keyword.cloud_notm}} CLI (`ibmcloud resource`), API, or Terraform.

### Backup, restore, and data lifecycle
{: #backup-restore-differences}

- **Volume snapshots:** Gen 2 uses underlying VPC block storage snapshots instead of logical dumps. This eliminates the compute and I/O performance impact during backup operations and ensures consistent backup timing regardless of database size.
- **Point-in-time recovery (PITR):** PITR will be available on the Enterprise Sharding plan using continuous, incremental oplog-based backups with configurable recovery history. Workloads on the Standard plan should restore from available automated snapshot points.
- **Cross-region replication of backups:** Automated cross-region replication of backup snapshots is available in Gen 1 and is customer-configurable in Gen 2. See [Cross-region backup replication](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-independent-backups&interface=ui#independent-backups-crc).

### High availability and replication
{: #ha-replication-differences}

- **Multizone high availability:** Both Gen 1 and Gen 2 deploy three-member replica sets that span multiple availability zones within a region.
- **Replica set replication:** MongoDB uses replica set-based replication in both generations. The primary node accepts all writes and replicates to two secondary nodes. Elections are automatic if the primary becomes unavailable.
- **Horizontal scaling with sharding:** The Enterprise Sharding plan supports horizontal scaling by adding shards. Standard plan deployments use a single three-member replica set.

### Database operations, tooling, and connections
{: #operations-tooling-differences}

- **Control plane and CLI tooling:** Gen 2 architecture is deeply integrated into the {{site.data.keyword.cloud_notm}} Resource Controller. The legacy `cdb` CLI plug-in operates exclusively against Gen 1 instances. Native Gen 2 plug-in commands are planned in a future release; currently, instance lifecycle management is performed by using standard `ibmcloud resource` CLI commands, the Resource Controller API, or Terraform.
- **Connection management:** Gen 1 provides a built-in UI button and `ibmcloud cdb deployment-kill-connections` command to immediately terminate all active sessions. In Gen 2, connection termination tooling is planned for a future release. In the interim, administrators can terminate individual connections by running `db.killOp(<opid>)` in the MongoDB shell with Manager privileges, or by initiating an instance restart.
- **Client tooling:** Because Gen 2 operates exclusively over private endpoints, database administration and querying are performed by using standard MongoDB client tools (for example, `mongosh`, MongoDB Compass, or Studio 3T) running inside an authorized VPC network.
