---

copyright:
  years: 2025
lastupdated: "2025-09-25"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager, mongodb compass, admin password, logging and monitoring, gen2

subcollection: databases-for-mongodb-gen2

content-type: tutorial

services:
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.databases-for-mongodb}}
{: #getting-started-gen2}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

This tutorial guides you through the steps to quickly start using {{site.data.keyword.databases-for-mongodb}} on the Gen 2 platform by provisioning an instance, setting up a secure connection through a VSI and VPE, and enabling logging and monitoring.
{: shortdesc}

Follow these steps to complete the tutorial: {: ui}


* [Before you begin](#prereqs)

* [Step 1: Choose your plan](#choose_plan)

* [Step 2: Provision through the console](#provision_instance_ui)

* [Step 3: Set up Private Connections](#private_connect_setup)

* [Step 4: Set up context-based restrictions](#mongodb_cbr)

* [Step 5: Connect IBM Cloud Monitoring](#mongodb_monitoring)

* [Step 6: Connect IBM Cloud Logs](#mongodb_logs)

* [Next steps](#next_steps)

{: ui}



Follow these steps to complete the tutorial: {: cli}

* [Before you begin](#prereqs)

* [Step 1: Choose your plan](#choose_plan)

* [Step 2: Provision through the console](#provision_instance_cli)

* [Step 3: : Set up Private Connections](#private_connect_setup)

* [Step 4: Set up context-based restrictions](#mongodb_cbr)

* [Step 5: Connect IBM Cloud Monitoring](#mongodb_monitoring)

* [Step 6: Connect IBM Cloud Logs](#mongodb_logs)

* [Next steps](#next_steps)

{: cli}


Follow these steps to complete the tutorial: {: api}

* [Before you begin](#prereqs)

* [Step 1: Choose your plan](#choose_plan)

* [Step 2: Provision through the console](#provision_instance_api)

* [Step 3: : Set up Private Connections)](#private_connect_setup)

* [Step 4: Set up context-based restrictions](#mongodb_cbr)

* [Step 5: Connect IBM Cloud Monitoring](#mongodb_monitoring)

* [Step 6: Connect IBM Cloud Logs](#mongodb_logs)

* [Next steps](#next_steps)

{: api}


Follow these steps to complete the tutorial: {: terraform}

* [Before you begin](#prereqs)

* [Step 1: Choose your plan](#choose_plan)

* [Step 2: Provision through the console](#provision_instance_tf)

* [Step 3: : Set up Private Connections](#private_connect_setup)

* [Step 4: Set up context-based restrictions](#mongodb_cbr)

* [Step 5: Connect IBM Cloud Monitoring](#mongodb_monitoring)

* [Step 6: Connect IBM Cloud Logs](#mongodb_logs)

* [Next steps](#next_steps)

{: terraform}

## Before you begin
{: #prereqs}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.

## Step 1: Choose your plan
{: #choose_plan}

{{site.data.keyword.databases-for-mongodb}} on Gen 2 platform currently offers only the {{site.data.keyword.databases-for-mongodb}} **Standard** plan, which is a fully managed NoSQL database service based on the MongoDB Community Edition.

--fix api here

### Using APIs
{: #using_apis}
{: api}

Use the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction){: external} to work with your {{site.data.keyword.databases-for-mongodb}} instance. The resource controller API is used to [provision an instance](#provision_instance_api).

You will need an API key to perform actions via the API. Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables you to use the API to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}



## Step 2: Provision through the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
2. Click the [{{site.data.keyword.databases-for-mongodb}} service](https://cloud.ibm.com/databases/databases-for-mongodb/create){: external} in the **catalog**.
3. In Service details, configure the following:

  - Location - Select a location with Gen 2 support.

  - Service name - The name can be any string and is the name that is used on the web and in the CLI to identify the new deployment.

  - Resource group - If you are organizing your services into resource groups, specify the resource group in this field. Otherwise, you can leave it at default. For more information, see [Managing resource groups](docs/account?topic=account-rgs&interface=ui)

4. Resource allocation - Specify the initial RAM, disk, and cores for your databases. The minimum sizes of memory and disk are selected by default. With dedicated cores, your resource group is given a single-tenant host with a minimum reserve of CPU shares. Your deployments are then allocated the number of cores that you specify. Once provisioned, disk cannot be scaled down.

5. In Service configuration, configure the following:

  - Database version Set only at deployment - The deployment version of your database. To ensure optimal performance, run the preferred version. The latest minor version is used automatically and currently the only option for Gen 2 databases. For more information, see Database versioning policy.--fix
  - Encryption - If you use Key Protect, an instance and key can be selected to encrypt the deployment's disk. If you do not use your own key, the deployment automatically creates and manages its own disk encryption key.

6. Click Create. The Cloud Databases Resource list page opens.
7.When your instance has been provisioned, click the instance name to view more information.

## Step 2: Provision through the CLI
{: #provision_instance_cli}
{: cli}

You can provision a {{site.data.keyword.databases-for-mongodb}} instance by using the CLI. If you don't already have it, you need to install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

1. Log in to IBM Cloud with the following command:

`ibmcloud login`

 If you use a federated user ID, it's important that you switch to a one-time passcode (ibmcloud login --sso), or use an API key (ibmcloud --apikey key or @key_file) to authenticate. For more information about how to log in using the CLI, see General CLI (ibmcloud) commands under ibmcloud login.

2. Create a {{site.data.keyword.databases-for-mongodb}} instance.

 To create an instance from the CLI, run the following command:

`ibmcloud resource service-instance-create <INSTANCE_NAME> <SERVICE_NAME> <SERVICE_PLAN_NAME> <LOCATION> <SERVICE_ENDPOINTS_TYPE> <RESOURCE_GROUP>`

 The fields in the command are described in the following table.


 | Field | Description | Flag |
 |-------|------------|------------|
 | `INSTANCE_NAME` [Required]{: tag-red} | The instance name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. | |
 | `SERVICE_NAME` [Required]{: tag-red} | Name or ID of the service. For {{site.data.keyword.databases-for-mongodb}}, use `databases-for-mongodb`. | |
 | `SERVICE_PLAN_NAME` [Required]{: tag-red} | `enterprise` or `platinum` | |
 | `LOCATION` [Required]{: tag-red} | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. | |
 | `RESOURCE_GROUP` | The Resource group name. The default value is `default`. | -g |
 | `--parameters` | JSON file or JSON string of parameters to create service instance | -p |
 {: caption="Basic command format fields" caption-side="top"}

## Step 2: Provision through the resource controller API
{: #provision_instance_api}
{: api}

Follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=api) to provision a {{site.data.keyword.databases-for-mongodb}} instance using the Resource Controller API. Obtain an IAM token from your API token.

1. You need to know the ID of the resource group that you would like to deploy to. This information is available through the IBM Cloud CLI.

 Use a command like:

`ibmcloud resource groups`

2. You need to know the region that you would like to deploy into.

 To list all of the regions that deployments can be provisioned into from the current region, use the Cloud Databases CLI plug-in.

 The command looks like:

`ibmcloud cdb regions --json`

3. Once you have all the information, provision a new resource instance with the IBM Cloud resource controller.

`curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances -H 'Authorization: Bearer <>' -H 'Content-Type: application/json' -d '{ "name": "my-instance", "target": "blue-us-south", "resource_group": "5g9f447903254bb58972a2f3f5a4c711", "resource_plan_id": "databases-for-monogodb" }'`

 The parameters name, target, resource_group, and resource_plan_id are all required.

List of Additional Parameters (all new parameters)

- backup_id- A CRN of a backup resource to restore from. The backup must be created by a database deployment with the same service ID. The backup is loaded after provisioning and the new deployment starts up that uses that data. A backup CRN is in the format crn:v1:<...>:backup:<uuid>. If omitted, the database is provisioned empty.
- version - The version of the database to be provisioned. If omitted, the database is created with the most recent major and minor version.
- disk_encryption_key_crn - The CRN of a KMS key (for example, Hyper Protect Crypto Services or Key Protect), which is then used for disk encryption. A KMS key CRN is in the format crn:v1:<...>:key:<id>.
- backup_encryption_key_crn - The CRN of a KMS key (for example, Hyper Protect Crypto Services or Key Protect), which is then used for backup encryption. A KMS key CRN is in the format crn:v1:<...>:key:<id>.

Note: To use a key for your backups, you must first enable the service-to-service delegation.
{: note}

- members_memory_allocation_mb - Total amount of memory to be shared between the database members within the database. For example, if the value is "6", and there are three database members, then the deployment gets 6 GB of RAM total, giving 2 GB of RAM per member. If omitted, the default value is used for the database type is used.
- members_disk_allocation_mb - Total amount of disk to be shared between the database members within the database. For example, if the value is "30720", and there are three members, then the deployment gets 30 GB of disk total, giving 10 GB of disk per member. If omitted, the default value for the database type is used.
- members_cpu_allocation_count - Enables and allocates the number of specified dedicated cores to your deployment. For example, to use two dedicated cores per member, use "members_cpu_allocation_count":"2". If omitted, the default value "Shared CPU" uses compute resources on shared hosts.
- service_endpoints - The Service endpoints supported on your deployment, public or private. This is a required parameter. --fix needed?

--fix the TF steps

## Step 2: Provision through Terraform
{: #provision_instance_tf}
{: terraform}

You need an API key to perform actions via Terraform. Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

Once you have an API Key, follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=terraform) to provision a {{site.data.keyword.databases-for-mongodb}} instance using Terraform.

## Step 3: Set up Private Connections
{: #private_connect_setup}

## Step 4: Set up context-based restrictions
{: #mongodb_cbr}

Context-based restrictions give account owners and administrators the ability to define and enforce access restrictions for IBM Cloud® resources based on the context of access requests. Access to Cloud Databases resources can be controlled with context-based restrictions and Identity and Access Management (IAM) policies.

To set up context-based restrictions for your {{site.data.keyword.databases-for-mongodb}} instance, follow the steps at Protecting Cloud Databases resources with context-based restrictions.

## Step 5: Connect IBM Cloud Monitoring
{: #mongodb_monitoring}

You can use IBM Cloud Monitoring to get operational visibility into the performance and health of your applications, services, and platforms. IBM Cloud Monitoring provides administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.

For more information about how to use Monitoring with {{site.data.keyword.databases-for-mongodb}}, see Monitoring integration.

Note: You cannot connect IBM Cloud Monitoring by using the CLI. Use the console to complete this task. For more information, see Monitoring integration.
{:note }

## Step 6: Connect IBM Cloud Logs Activity Tracker

IBM Cloud Activity Tracker Event Routing allows you to view, manage, and audit service activity to comply with corporate policies and industry regulations. Activity Tracker Event Routing records user-initiated activities that change the state of a service in IBM Cloud. Use Activity Tracker Event Routing to track how users and applications interact with the {{site.data.keyword.databases-for-mongodb}} service.

To get up and running with Activity Tracker Event Routing, see [Getting Started with Activity Tracker Event Routing]/docs/atracker?topic=atracker-getting-started){: external}.

Activity Tracker Event Routing can have only one instance per location. To view events, you must access the web UI of the Activity Tracker Event Routing service in the same location where your service instance is available. For more information, see Launch the web UI. For more information about events specific to {{site.data.keyword.databases-for-mongodb}}, see Activity tracking events. Events are formatted according to the Cloud Auditing Data Federation (CADF) standard. For further details of the information they include, see CADF standard.

Note: You cannot connect Activity Tracker Event Routing by using the API. Use the console to complete this task. For more information, see Activity tracking events.
{:note }

## Next steps
{: #next_steps}

- If you are using MongoDB for the first time, see the official [MongoDB documentation](https://docs.mongodb.com/){: external}.

- For guidance on best practices, see [Best practices for MongoDB on the IBM Cloud](https://www.ibm.com/blog/best-practices-for-mongodb-on-the-ibm-cloud/){: .external}.

- Secure your deployment by adding [context-based restrictions](/docs/cloud-databases?topic=cloud-databases-cbr&interface=ui).

- Connect your deployment to [{{site.data.keyword.logs_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging) and [{{site.data.keyword.monitoringfull}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-monitoring) for observability and alerting.

- Explore the [Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-ops-manager) functionality offered in the {{site.data.keyword.databases-for-mongodb}} Enterprise Edition.



- Looking for more tools on managing your databases? Connect to your instance with the following tools:

- [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli){: external}

- [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference){: external}

- [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api){: external}



- If you plan to use {{site.data.keyword.databases-for-mongodb}} for your applications, see the following topics:

- [Connecting an external application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-external-app)

- [Connecting an {{site.data.keyword.cloud_notm}} application](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-connecting-ibmcloud-app)

- [MongoDB Node.js Driver Connection Guide](https://docs.mongodb.com/drivers/node/current/fundamentals/connection/){: external}



- For information on TLS/SSL certificate configuration in the API, see the following topics:

- [Driver TLS and service proprietary certificate support](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-external-app#mongodb-tls-certificate-support)

- [Using the service proprietary certificate in mongo Shell](/docs/databases-for-mongodb?topic=databases-for-mongodb-connecting-cli-client#connecting-cli-client-cert)

- [MongoDB TLS/SSL configuration for clients](https://docs.mongodb.com/manual/tutorial/configure-ssl-clients/){: external}



- To ensure the stability of your applications and databases, see the following topics:

- [High-availability](/docs/databases-for-mongodb?topic=databases-for-mongodb-ha-dr)

- [Performance](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance)
