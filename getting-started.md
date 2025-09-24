---
copyright:
  years: 2019, 2025
lastupdated: "2025-09-24"

keywords: mongodb, databases, mongodb compass, mongodbee, mongodb enterprise, mongodb ee provision, mongodb compass, mongodb ops manager, mongodb compass, admin password, logging and monitoring

subcollection: databases-for-mongodb

content-type: tutorial
services:
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.databases-for-mongodb}}
{: #getting-started-new}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="30m"}

This tutorial guides you through the steps to quickly start using {{site.data.keyword.databases-for-mongodb}} by provisioning an instance, setting your Admin password, connecting to it and writing and reading a simple document.
{: shortdesc}

Follow these steps to complete the tutorial: {: ui}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the console](#provision_instance_ui)
* [Step 3: Set your Admin password through the console](#admin_pw)
* [Step 4: Connect to your instance](#mongodb_connect)
* [Next steps](#next_steps)
{: ui}

Follow these steps to complete the tutorial: {: cli}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the CLI](#provision_instance_cli)
* [Step 3: Set your Admin password through the CLI](#admin_pw)
* [Step 4: Connect to your instance](#mongodb_connect)
* [Next steps](#next_steps)
{: cli}

Follow these steps to complete the tutorial: {: api}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through the API](#provision_instance_api)
* [Step 3: Set your Admin password](#admin_pw)
* [Step 4: Connect to your instance](#mongodb_connect)
* [Next steps](#next_steps)
{: api}

Follow these steps to complete the tutorial: {: terraform}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision through Terraform](#provision_instance_tf)
* [Step 3: Set your Admin password](#admin_pw)
* [Step 4: Connect to your instance](#mongodb_connect)
* [Next steps](#next_steps)
{: terraform}

## Before you begin
{: #prereqs}

* You need an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.

## Step 1: Choose your plan
{: #choose_plan}

{{site.data.keyword.databases-for-mongodb}} offers two different plans:

* {{site.data.keyword.databases-for-mongodb}} **Standard** is a fully managed NoSQL database service based on the MongoDB Community Edition.

* {{site.data.keyword.databases-for-mongodb}} **Enterprise** offers advanced features, such as the [MongoDB Ops Manager](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans#ops-manager) and [point-in-time recovery](/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr).

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
1. Click the [{{site.data.keyword.databases-for-mongodb}} service](https://cloud.ibm.com/databases/databases-for-mongodb/create){: external} in the **catalog**.
1. Follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=ui) to provision a {{site.data.keyword.databases-for-mongodb}} instance.
1. When your instance is provisioned, click the instance name to view more information.

## Step 2: Provision through the CLI
{: #provision_instance_cli}
{: cli}

You can provision a {{site.data.keyword.databases-for-mongodb}} instance by using the CLI. If you don't already have it, you need to install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

You can follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=cli) to provision a {{site.data.keyword.databases-for-mongodb}} instance.

## Step 2: Provision through the resource controller API
{: #provision_instance_api}
{: api}

Follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=api) to provision a {{site.data.keyword.databases-for-mongodb}} instance using the Resource Controller API.

## Step 2: Provision through Terraform
{: #provision_instance_tf}
{: terraform}

You need an API key to perform actions via Terraform. Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an IBM Cloud API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
 {: note}

Once you have an API Key, follow [these steps](/docs/databases-for-mongodb?topic=databases-for-mongodb-provisioning&interface=terraform) to provision a {{site.data.keyword.databases-for-mongodb}} instance using Terraform.

## Step 3: Set the admin password
{: #admin_pw}

### The admin user
{: #admin_pw_admin_user}

When you provision a {{site.data.keyword.databases-for-mongodb}} deployment, an `admin` user is automatically created.

Set the admin password before using it to connect.
{: important}

### Set the admin password through the UI
{: #admin_pw_set_ui}
{: ui}

Set your admin password through the UI by selecting your instance from the [{{site.data.keyword.cloud_notm}} Resource list](https://cloud.ibm.com/resources){: external}. Then, select **Settings**. Next, select *Change Database Admin password*.

### Set the admin password through the CLI
{: #admin_pw_set_cli}
{: cli}

Use the `cdb user-password` command from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to set the admin password.

For example, to set the admin password for your deployment, use the following command:

```sh
ibmcloud cdb user-password <INSTANCE_NAME_OR_CRN> admin <NEWPASSWORD>
```
{: pre}

### Set the admin password through the API
{: #admin_pw_set_api}
{: api}

YOu can use the `id` parameter obtained in the response to Step 2 above with the [Set specified user's password](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#updateuser){: external} endpoint to set the admin password.

```sh
curl -X PATCH -H "Authorization: Bearer <TOKEN>" \
     -H 'Content-Type: application/json' \
     -d '{"password":"newrootpasswordsupersecure21"}' \
      "https://api.<REGION>.databases.cloud.ibm.com/v5/ibm/deployments/<DEPLOYMENT_ID>/users/database/admin"
```
{: pre}

The `id` parameter needs to be URL-encoded for the above API call to work.
{: important}

### Setting the admin password through Terraform
{: #admin_pw_set_tf}
{: terraform}

The admin password is passed in as one of the database resource parameters in the Terraform script. There is no need for any further action.

## Step 4: Connect to your {{site.data.keyword.databases-for-mongodb}} instance
{: #mongodb_connect}

You can easily connect to your instance by either using the Mongo Shell (a command line interface) or Mongo Compass, a powerful GUI for querying and analyizing your data. Both of these tools are provided my MongoDB.

### Using the Mongo Shell
{: #mongo_shell}

Follow [these instructions](/docs/databases-for-mongodb?topic=databases-for-mongodb-connecting-cli-client&interface=ui) to download and connect to the Mongo Shell.

You can then test your deployment by inserting a document into a collection:

```sh
use sample_mflix

db.movies.insertOne(
  {
    title: "The Favourite",
    genres: [ "Drama", "History" ],
    runtime: 121,
    rated: "R",
    year: 2018,
    directors: [ "Yorgos Lanthimos" ],
    cast: [ "Olivia Colman", "Emma Stone", "Rachel Weisz" ],
    type: "movie"
  }
)
```
{:pre}

The above command switches to a database called `sample_mflix` (and creates it if it does not already exist), and then insert a document into the `movies` collection (which gets also be created if it does not already exist).

You can then retrieve the document with:

```sh
db.movies.find( { title: "The Favourite" } )
```
{:pre}

You now connected to your database and wrote and read data using the Mongo Shell.

### Using MongoDB Compass
{: #mongodb_compass}

Follow [these instructions]( /docs/databases-for-mongodb?topic=databases-for-mongodb-connecting-mongodb-compass&interface=ui) to download MongoDB Compass and use it to connect to your {{site.data.keyword.databases-for-mongodb}} instance. You can write and read data using the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current){: external}.

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
