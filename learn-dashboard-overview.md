---

copyright:
  years: 2018, 2025
lastupdated: "2025-09-24"

keywords: deployment, crn, task, gui, api endpoint

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# The Dashboard overview
{: #dashboard-overview}

## Overview
{: #dashboard-overview-page}

The _Overview_ page shows you information about your {{site.data.keyword.databases-for-mongodb_full}} deployment. The overview includes essential identifying information.

### Deployment details
{: #dashboard-overview-deployment-details}

- **Type:** The type of database that is offered by the service, and the database version that your service uses.
- **CRN (deployment ID):** The ID is a [CRN (Cloud Resource Name)](/docs/account?topic=account-crn) that uniquely identifies the database deployment. The CRN is used to refer to the database in the API and can be used with the CLI. The _Overview_ pane shows details of your service.

### Resources
{: #dashboard-overview-resources}

The resources tab contains information and configuration options on the size and resource usage of your deployment. You can [scale disk, memory, and CPU](/docs/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling) and [configure Autoscaling](/docs/databases-for-mongodb?topic=databases-for-mongodb-autoscaling).

### Recent tasks
{: #dashboard-overview-recent-tasks}

Every time that you make administrative changes to your service (such as scaling, or taking a manual backup), a task starts up. The _Recent tasks_ panel shows the task name and progress bar for any running tasks, and a list of the most recent completed tasks. Depending on how busy your deployment is, successful tasks can be shown for 24-48 hours. Unsuccessful tasks can show for 7-8 days. Tasks can also be retrieved from the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymenttasks) and [CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-tasks-list). A historical record of tasks from any time period is available through [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events).

### Observability
{: #dashboard-overview-observability}

The _Observability_ tab provides access to the {{site.data.keyword.monitoringlong}}, logging, and event tracking integrations available for your deployment.

- [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events)
- [{{site.data.keyword.logs_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging)
- [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring)

### Endpoints
{: #dashboard-overview-endpoints}

The _Endpoints_ pane within the _Overview_ pane contains connection strings for your deployment. Each tab contains connection information tailored to the type of connection or the protocol that uses it. Basic information includes things like _hostname_ and _port_, as well as the TLS service proprietary certificate, TLS/SSL parameters, and the default database of your deployment.

For more information on reference tables for the different connection types, see [Getting credentials and connection strings](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings).

Connection strings reflect whether your deployment uses public or private endpoints. You can configure which endpoints are available on your deployment. For more information, see [Service endpoints integration](/docs/cloud-databases?topic=cloud-databases-service-endpoints).

You can manage your {{site.data.keyword.databases-for-mongodb}} service through the {{site.data.keyword.databases-for}} API. This panel provides the essential information for using the API. For more information, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api).

## Backups and restore
{: #dashboard-overview-backups-and-restore}

The _Backups_ tab is the UI for managing your deployments backups. All of the available backups are listed with their timestamps. Click a backup to grab its ID or to restore it into a new deployment. More information is on the [Managing backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups) page.

## Settings
{: #dashboard-overview-settings}

The _Settings_ tab contains the UI for many of the tunable settings for your deployment. You can:

- View encryption details. Encryption at rest is enabled for all {{site.data.keyword.databases-for-mongodb}} deployments. If you brought your own encryption key from [Key Protect](/docs/databases-for-mongodb?topic=databases-for-mongodb-key-protect&interface=ui), the panel provides a link to your Key Protect instance and the _Encryption key_ field has the name of the key.
- [Change the admin password](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management&interface=ui#user-management-set-admin-password-ui).
- [Implement or modify an IP allowlist](/docs/databases-for-mongodb?topic=databases-for-mongodb-allowlisting&interface=ui)
- [Context-based restrictions](/docs/databases-for-mongodb?topic=databases-for-mongodb-cbr&interface=ui).

## Service credentials
{: #dashboard-overview-service-cred}

You can generate a new set of credentials for cases where you want to manually [connect an IBM Cloud application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-connecting-ibmcloud-app&interface=api) or [an external application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-external-app&interface=api) to an IBM Cloud service. For more information, see [Service credentials](/docs/account?topic=account-service_credentials).

## View docs
{: #dashboard-overview-view-docs}

The _View docs_ link from the `Actions` drop list opens the main documentation page for {{site.data.keyword.databases-for-mongodb}} in a new tab.
