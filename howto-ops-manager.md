---
copyright:
  years: 2020, 2025
lastupdated: "2025-09-24"

keywords: databases, opsman, mongodbee, Enterprise Edition

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.databases-for-mongodb}} Enterprise Ops Manager
{: #ops-manager}

The Ops Manager is only available with a {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployment.

## Before you begin with the MongoDB Enterprise Ops Manager
{: #ops-manager-before-begin}

- Follow the [Getting started](/docs/databases-for-mongodb?topic=databases-for-mongodb-getting-started-new&interface=ui) instructions to provision an instance of {{site.data.keyword.databases-for-mongodb}} Enterprise Edition and set the admin password.

## Create an Ops Manager user in the CLI
{: #create-ops-man}
{: cli}

Before logging in to the {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Ops Manager, you must create an Ops Manager username and password for your deployment. To do that, run the following command:

```sh
ibmcloud cdb user-create <INSTANCE_CRN> <USERNAME> <PASSWORD> -t ops_manager -r <ROLE>
```
{: pre}

All fields except `role` are required. The `instance_crn` is also known as deployment ID and is the unique identifier of your resources. It starts with `crn:...`.

Passwords must be at least 15 characters long and must contain at least one number and one letter. Passwords can contain uppercase and lowercase letters, numbers, - (hyphen), or _ (underscore).
{: .note}

Example command:

```sh
 ibmcloud cdb user-create "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/40ddc34a953a8c02f10987b59085b60e:32bd88c9-1d96-4486-8012-1dgcd629e609::" newuser01 SuperSecure001! -t ops_manager -r group_read_only
```
{: .pre}

The Ops Manager user has limited permissions.
{: .note}

## Create an Ops Manager user in the API
{: #create-ops-man}
{: api}

Before logging in to the {{site.data.keyword.databases-for-mongodb}} Enterprise Edition Ops Manager, you must create an Ops Manager username and password for your deployment. To do that via the API, run the following command:

```sh
   curl -X POST https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{service_id}/users/ops_manager \
   -H 'Authorization: Bearer <>' \
   -H 'Content-Type: application/json' \
   -d '{"user": {"username": "om_user", "password": "v3ry-1-secUre-pAssword-2", "role":"group_data_access_admin"}}' 
```
{: pre}

All fields except `role` are required. The `service_id` is also known as a CRN or deployment ID and is the unique identifier of your resources. It starts with `crn:...`.

Passwords must be at least 15 characters long and must contain at least one number and one letter. Passwords can contain uppercase and lowercase letters, numbers, - (hyphen), or _ (underscore).
{: .note}

You will get a response like this:

```json
{
  "task": {
    "id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/f19c0f5eff94b69ae419d3se4rfta7a0ed:de50e7c9-d534-4e13-af4f-c7429c58b78f:task:21a5721c-5eb2-4b8a-bb30-fd4f88e4ac49",
    "description": "Creating Ops Manager user",
    "status": "running",
    "deployment_id": "crn:v1:bluemix:public:databases-for-mongodb:us-south:a/f19c0f5eff945rf6ythgf9db57e9a7a0ed:de50e7c9-d534-4e13-af4f-c7429c58b78f::",
    "progress_percent": 0,
    "created_at": "2024-08-19T10:44:08.000Z"
  }
}
```
{: pre}

The `id` parameter is the user id and can be used below to obtain connection strings to your Ops Manager instance.

## Roles within Ops Manager
{: #create-roles-man}

When creating an Ops Manager user, you have the option of creating two roles: `group_data_access_admin` and `group_read_only`.

If no role is specified, `group_data_access_admin` is the default user, affording you Ops Manager default access and privileges. The `group_data_access_admin` role is equivalent to MongoDB's [Project Data Access Admin](https://docs.opsmanager.mongodb.com/current/reference/user-roles/#Project-Data-Access-Admin){: external} role.

The `group_read_only` role, which is equivalent to MongoDB's [Project Read Only role](https://docs.opsmanager.mongodb.com/current/reference/user-roles/#Project-Read-Only){: external}, can view most components, including activity, operational data, Ops Manager users, and Ops Manager User roles. This user cannot modify or delete anything. `group_read_only` users also do not have access to view data in the Ops Manager UI.

For more information on roles with Ops Manager, see MongoDB's [Ops Manager roles](https://docs.opsmanager.mongodb.com/current/reference/user-roles/){: external}.
{: .tip}

## Initial login
{: #initial-login}

The {{site.data.keyword.databases-for-mongodb}} Enterprise Edition service is provisioned with access to the MongodDB Ops Manager user interface.

As {{site.data.keyword.databases-for-mongodb}} Enterprise Edition uses service proprietary certificates, your browser may require you to accept the certificate or add the certificate to your certificate store before being able to log in.
{: .tip}

After you create an Ops Manager username and password, you can follow these instructions to get access to the {{site.data.keyword.databases-for-mongodb}} Enterprise Edition instance within the Ops Manager UI:

1. Discover the Ops Manager link.

   - You can do that with the following CLI command:

     ```sh
     ibmcloud cdb deployment-connections <service_id> -t ops_manager
     ```
     {: .pre}

     If setting up Ops Manager with private endpoints, you must append `-e 'private'` to your command.
     {: .note}

   - Or with this [API command](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#getconnection){: external}:

     ```sh
     curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{service_id}/users/ops_manager/{user_id}/connections/{endpoint_type} -H 'Authorization: Bearer <>'
     ```
     {: .pre}

1. Use the url retrieved in the previous step to log in with the Ops Manager username and password you created for your deployment.

1. In the resulting view, select the **Invitations** tab.

1. Click **Accept** for the invitation as role *Project Data Access Admin*. This step adds your Ops Manager user ID to the organization and project shown.

1. Lastly, to navigate to the instance view:

   - Click the **Ops Manager** logo in the menu bar.
   - Or select the **All Clusters** link.

On subsequent logins you arrive at the last view, so the prior procedure is only necessary on the first login.
{: .tip}

## Connecting through private endpoints
{: #private-endpoints}

{{site.data.keyword.databases-for-mongodb}} Enterprise Edition offers an HTTPS accessible endpoint for the Ops Manager user interface.

{{site.data.keyword.databases-for-mongodb}} Enterprise Edition also offers both private and public cloud service endpoints. If you want to access the Management UI from a browser that is not on the private network, you must take these additional steps as listed in the [Connecting Through Private Endpoints](/docs/cloud-databases?topic=cloud-databases-service-endpoints#private-endpoints) documentation for {{site.data.keyword.cloud}} Databases.

After you configure your environment for private endpoint access, you can go to to the {{site.data.keyword.databases-for-mongodb}} Enterprise management endpoint URL from your browser. For example, `https://bfdb-4263-8ad2-c9a4beaf4591.8f7bfc8f3faa4218afd56e0.databases.appdomain.cloud:323232`.

## Next steps
{: #enterprise-ops-next-steps}

MongoDB Ops Manager is a feature-rich user interface to manage your MongoDB deployment. For more information, see the [MongoDB Ops Manager documentation](https://www.mongodb.com/docs/ops-manager/current/){: external}.
