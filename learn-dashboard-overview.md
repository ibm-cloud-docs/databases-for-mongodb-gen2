---

copyright:
  years: 2025
lastupdated: "2025-10-07"

keywords: deployment, crn, task, gui, api endpoint, gen2

subcollection: databases-for-mongodb-gen2

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

The resources tab contains information and configuration options on the size and resource usage of your deployment. You can [scale disk, memory, and CPU](/docs/databases-for-mongodb?topic=databases-for-mongodb-resources-scaling).

### Recent tasks
{: #dashboard-overview-recent-tasks}

Every time you make administrative changes to your service (such as scaling, or taking a manual backup), a task starts up. The _Recent tasks_ panel shows only the **latest task**, including its name and progress bar. After completion, the most recent task remains visible for a short period of time.

To view more than the latest task, you can configure an [{{site.data.keyword.logs_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging) instance to capture and retain logs from your deployment.  

A historical record of tasks from any time period is available through [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events). Tasks can also be retrieved programmatically from the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymenttasks) and [CLI plug-in](https://cloud.ibm.com/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-tasks-list).


### Observability
{: #dashboard-overview-observability}

The _Observability_ tab provides access to the {{site.data.keyword.monitoringlong}}, logging, and event tracking integrations available for your deployment.

- [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events)
- [{{site.data.keyword.logs_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging)
- [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring)

### Service endpoints
{: #dashboard-overview-endpoints}

The _Service endpoints_ pane within the _Overview_ pane contains connection strings for your deployment.

For Gen 2 deployments, endpoints are **private only**, ensuring that your database is accessible exclusively within your {{site.data.keyword.cloud}} private network.

The information displayed includes:

- Endpoint string for connection.
- _hostname_ and _port_ values for direct connections to the database members.
- The default database for your service.
- SSL mode parameters. Gen 2 uses certificates from *Let’s Encrypt* instead of proprietary TLS certificates. The recommended setting is **verify-full for secure connections**.

Gen 2 deployments do not include a pre-provisioned database admin password. To begin working with your instance, you must [create a service credential](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-credentials&interface=ui), which provides the authentication details and connection strings required for your applications.  
For more information on reference tables for the different connection types, see [Getting credentials and connection strings](/docs/databases-for-mongodb?topic=databases-for-mongodb-connection-strings).

**(should we merge the above two and they become one link? need confirmation)**

You can manage your {{site.data.keyword.databases-for-mongodb}} service through the {{site.data.keyword.databases-for}} API. This panel provides the essential information for using the API. For more information, see the [API reference](https://{DomainName}/apidocs/cloud-databases-api).

## Backups and restore
{: #dashboard-overview-backups-and-restore}

The _Backups and restore_ tab is the UI for managing your deployments backups. All of the available backups are listed with their timestamps. Click a backup to copy its ID or to restore it into a new deployment. For more information, see [Managing backups](/docs/cloud-databases?topic=cloud-databases-dashboard-backups).

## Settings
{: #dashboard-overview-settings}

The _Settings_ tab contains the UI for many of the tunable settings for your deployment.

- **View encryption details**. All {{site.data.keyword.databases-for-mongodb}} deployments are automatically encrypted at rest. Disks and backups are encrypted, and the encryption keys are managed automatically by {{site.data.keyword.cloud}}. If you brought your own encryption key from [{{site.data.keyword.keymanagementserviceshort}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-key-protect&interface=ui), the panel provides a link to your {{site.data.keyword.keymanagementserviceshort}} instance and displays the key name in the _Encryption key_ field.

- **Service endpoints**. View and manage network access to your deployment. By default, only private endpoints are enabled to restrict access. For more information, see [Private endpoints on Gen 2](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-endpoints&interface=ui).

- **Context-based restrictions (CBR)**. You can create context-based restriction rules to control access to your deployment. For more information, see [Context-based restrictions](/docs/databases-for-mongodb?topic=databases-for-mongodb-cbr&interface=ui).

- **Disconnect all active connections**. You can disconnect all client connections to your database. This action immediately ends all active sessions and can cause service interruptions. Use this option with caution when you need to revoke access or apply configuration changes.

## Service credentials
{: #dashboard-overview-service-cred}

Service credentials provide the authentication details that you need to connect your applications to a {{site.data.keyword.databases-for-mongodb}} deployment.  

For Gen 2 deployments, you must create a service credential before you can begin working with your instance. Unlike Gen 1, there is no pre-provisioned database admin password. On first use (or if a credential has been deleted), the _Overview_ page prompts you to create a credential in order to connect.

### Creating a service credential
{: #create-service-cred}

1. Go to the **Service credentials** tab.  
2. Click **Create credential**.  
3. (Optional) Enable **Control by Secrets Manager** to integrate the credential with your {{site.data.keyword.secrets-manager_full}} instance.  
4. Provide a name for the credential.  
5. (Optional) Expand **Advanced options** to upload a JSON file or add inline configuration parameters.  
6. Click **Create**.

The new credential appears in the list and can be copied into your applications. Credentials are generated as **one-time view only**, so be sure to store them securely after creation.

### Using service credentials
{: #using-service-cred}

- A service credential contains the username, password, and connection strings needed to connect to your database.  
- The credentials can be used directly in your application, or integrated with {{site.data.keyword.secrets-manager_full}} for centralized key management.  
- If you delete a credential, you must create a new one to continue connecting.  

For more information, see [Managing service credentials](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-credentials&interface=ui).

In Gen 2 deployments, **service credentials are required as the first step to connect**. Unlike Gen 1, an admin password is not provided by default. If no credential exists, you are prompted to create one on the _Overview_ page before you can establish any database connection.
{: note}

## View docs
{: #dashboard-overview-view-docs}

[--- I think this paragraph is superfluent and can be deleted - pls. confirm]

The **View docs** link from the *Actions* drop list opens the main documentation page for {{site.data.keyword.databases-for-mongodb}} in a new tab.
