---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: HA, DR, high availability, disaster recovery, disaster recovery plan, disaster event, mongodb

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.databases-for-mongodb}}
{: #mongodb-ha-dr}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the  [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}

{{site.data.keyword.databases-for-mongodb}} is a regional service that fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard and Enterprise plans. For more information, see [Service Level Agreement (SLA)](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en). For more information about the available {{site.data.keyword.cloud_notm}} regions and data centers for {{site.data.keyword.databases-for-mongodb}}, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## High availability architecture
{: #ha-architecture}

![Architecture](images/mongodb-base.svg){: caption="MongoDB architecture" caption-side="bottom"}

{{site.data.keyword.databases-for-mongodb}} provides replication, failover, and high-availability features to protect your databases and data from infrastructure maintenance, upgrades, and some failures. Deployments contain a cluster with three data members - one primary and two secondary members. The two member replica set is kept up to date using asynchronous replication. A distributed consensus mechanism is used to maintain cluster state and handle failovers. If the primary is unavailable, the replica set elects a secondary to be primary and continues normal operation. The old primary rejoins the set when available. The primary and secondary members will always be in different zones of an MZR. If a zone failure results in a member failing, the new replica will be created in a surviving zone.

### High availability features
{: #ha-features}

{{site.data.keyword.databases-for-mongodb}} supports the following high availability features.

| Feature | Description |
| -------------- | -------------- |
| Automatic failover | Standard on all clusters and resilient against a zone or single member failure. |
| Member count | Minimum 3 members. Default is a Standard three member deployment. A three-member cluster will automatically recover from a single instance or zone failure (with data loss up to the lag threshold). |
| Asynchronous replication | Secondaries replicate the primary's operations and apply the operations to their data sets asynchronously. By having the secondaries' data sets reflect the primary's data set, the replica set can continue to function despite the failure of one or more members. |
{: caption="High availability features" caption-side="top"}

## Disaster recovery architecture
{: #disaster-recovery-intro}

The general strategy for disaster recovery is to create a new database, such as the following `MongoDB Restore` database. The contents of the new database can be a backup of the source database created before the disaster. A new database can be created using the point-in-time feature for the Enterprise plan, if the production database is available.

![Architecture](images/mongodb-restore.svg){: caption="MongoDB restore architecture" caption-side="bottom"}

### Disaster recovery features
{: #dr-features}

{{site.data.keyword.databases-for-mongodb}} supports the following disaster recovery features.

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Backup restore | Create database from previously created backup; see [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups). | New connection strings for the restored database must be referenced throughout the workload. |
| Point-in-time restore | Create database from the live production using [point-in-time recovery](/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr). | This is only possible for the Enterprse plan and if the active database is available and the RPO (disaster) falls within the supported window. It is not useful if the production cluster is unavailable. New connection strings for the restored database must be referenced throughout the workload. |
{: caption="Disaster recovery features" caption-side="top"}

### Planning for disaster recovery
{: #features-for-disaster-recovery}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.

| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | IBM provides a database that is resilient from a single point of hardware failure within a zone - no configuration is required. |
| Zone failure | Automatic failover. The database members are distributed between zones. Configuring three members will provide additional resiliency to multiple zone failures.|
| Data corruption | Backup restore. Use the restored database in production or for source data to correct the corruption in the restored database. \n \nPoint-in-time restore. Use the restored database in production or for source data to correct the corruption in the restored database. |
| Regional failure | Backup restore. Use the restored database in production. |
{: caption="Failure scenarios and resolutions" caption-side="top"}

## Application-level high-availability
{: #application-level-ha}

Applications that communicate over networks and cloud services are subject to transient connection failures. You want to design your applications to retry connections when errors are caused by a temporary loss in connectivity to your deployment or to {{site.data.keyword.cloud_notm}}.

Your applications must be designed to handle temporary interruptions to the database, implement error handling for failed database commands, and implement retry logic to recover from a temporary interruption.

Several minutes of database unavailability or connection interruption are not expected. Open a [support case](https://cloud.ibm.com/unifiedsupport/cases/add) with details if you have periods longer than a minute with no connectivity so we can investigate.

## Your responsibilities for HA and DR
{: #feature-responsibilities}

The following information can help you create and continuously practice your plan for HA and DR.



When restoring a database from backups or using point-in-time restore, a new database is created with new connection strings. Existing workloads and processes must be adjusted to consume the new connection strings. Promoting a read replica to a cluster will have a similar impact, although existing read-only portions of the workload will not be impacted.

A recovered database may also need the same customer-created dependencies of the disaster database - make sure the following and other services exist in the recovered region:

- {{site.data.keyword.keymanagementservicefull}}
- {{site.data.keyword.hscrypto}}

Remember that deleting a database also deletes its associated backups. However, deleted databases may be recoverable within a limited timeframe. Refer to the [FAQ backups documentation](/docs/cloud-databases?topic=cloud-databases-faq-backups) for specific details on database recovery procedures.

It is not possible to copy backups off the {{site.data.keyword.cloud_notm}}, so consider using the database-specific tools for additional backups. It may be required to recover from malicious database deletion followed by a reclamation-delete of a database. Careful management of IAM access to databases can help reduce exposure to this problem.

The following checklist associated with each feature can help you to create and practice your plan.

- Backup restore
   - Verify that backups are available at the desired frequency to meet RPO requirements. [Managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups) documents backup frequency. Consider a script using [IBM Cloud® Code Engine - Working with the Periodic timer (cron) event producer](/docs/codeengine?topic=codeengine-subscribe-cron) to create additional on-demand backups to improve RPO if the criticality and size of the database allow.
   - There are some restrictions on database restore regions - verify that your restore goals can be achieved by reading [managing Cloud Databases backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups).
   - Verify that the retention period of the backups meet your requirements.
   - Schedule test restores regularly to verify that the actual restored times meet the defined RTO. Remember that database size significantly impacts restore time. Consider strategies to minimize restore times, such as breaking down large databases into smaller, more manageable units and purging unused data.
   - Verify the Key Protect service.
- Point-in-time restore
   - Verify the procedures covered earlier.
   - Verify desired backup is in the window.

For more information on responsibility ownership between the customer and {{site.data.keyword.cloud_notm}} for using {{site.data.keyword.databases-for-mongodb}}, see [Shared responsibilities for {{site.data.keyword.databases-for}}](/docs/cloud-databases?topic=cloud-databases-responsibilities-cloud-databases).

## Stay informed: {{site.data.keyword.IBM_notm}} notifications
{: #ibm-service-notifications}

Updates affecting customer workloads are communicated through {{site.data.keyword.cloud_notm}} notifications. To stay informed about planned maintenance, announcements, and release notes related to this service, refer to [Monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status). In addition, regularly review the [Version policy](/docs/cloud-databases?topic=cloud-databases-versioning-policy) for the latest updates on End-of-Life versions and dates.

## Additional guidance
{: #ha_dr-guidance}

- [Understanding high availability for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-ha-dr)
- [Understanding business continuity and disaster recovery for Cloud Databases](/docs/cloud-databases?topic=cloud-databases-bc-dr)
- [Managing connections - Databases for {{site.data.keyword.databases-for-mongodb}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-managing-connections)
