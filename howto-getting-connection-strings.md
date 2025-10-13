---

copyright:
  years: 2025
lastupdated: "2025-10-13"

keywords: mongodb, databases, connection strings, gen 2

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}


# Getting connection strings
{: #connection-strings}

Connection strings allow you to establish a connection between your application and your {{site.data.keyword.databases-for-mongodb}} instance.

## Getting connection strings in the UI
{: #connection-strings-ui}
{: ui}

Follow these steps to retrieve your {{site.data.keyword.databases-for-mongodb}} instance connection strings:

1. In your deployment's **Overview page**, scroll down to the *Service endpoints* section. 
1.  The *Service endpoints* section displays tabs for available connection methods:  
   - **MongoDB** – Shows the connection string, hostnames, ports, database name, authentication source, and replica set for your deployment.  
   - **CLI** – Provides details for connecting by using the [{{site.data.keyword.IBM_notm}} CLI](https://www.ibm.com/cloud/cli){: external}.  

## Getting connection strings in the CLI
{: #connection-strings-cli}
{: cli}

You can also retrieve connection strings using the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections) Connection command.

The command looks like this: 

```sh
ibmcloud cdb deployment-connections <INSTANCE_NAME_OR_CRN> -u <NEWUSERNAME> [--endpoint-type <ENDPOINT_TYPE>]
```
{: pre}

For more information, see [Connections command options](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#connections-command-options).

### Command options
{: #connection-strings-cli-command-options}
{: cli}

- If you don't specify a `user`, the Connections commands return information for the `admin` user, by default. (--confirm if it works for manager user)
- If you don't specify an `endpoint-type`, the connection string returns the public endpoint by default. (--does it return private by default now?)
- If your deployment has only a private endpoint, specify `--endpoint-type private` or the commands return an error. The user and endpoint type is not enforced. (--still enforced?)

## Getting connection strings in the API
{: #connection-strings-api}
{: api}

To retrieve users' connection strings from the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction){: external}, use the [Connections endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#getconnection){: external}. To create the connection strings, ensure that the path includes the specific user and endpoint type that should be used. The `user` is not restricted or enforced. You have the flexibility to utilize any user available in your deployment. 

(--should we swap endpoint type with only private as static type?)
The API command looks like: 

```sh
curl -X GET -H "Authorization: Bearer <>" 'https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/users/{userid}/connections/{endpoint_type}'
```
{: pre}

Remember to replace {region}, {id}, {userid}, and {endpoint_type} with the appropriate values.
{: note}

## Additional users and connection strings
{: #connection-strings-additional-users-strings}

Access to your {{site.data.keyword.databases-for-mongodb}} deployment is not limited to the `manager` user. Create more users and retrieve connection strings specific to them by using the UI, the (--fix with updated links if needed, verify api link) [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction).

All users on your deployment can use the connection strings, including connection strings for either public or private endpoints.

For more information, see the [Managing users and roles](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management) page. (--user page needs updates/to be created)

Unlike Gen 1, Gen 2 deployments do not include a pre-provisioned database admin password. To connect, you must (--fix with updated link when page created) [create a service credential](/docs/databases-for-mongodb?topic=databases-for-mongodb-service-credentials&interface=ui), which provides the necessary authentication details for your applications.

Gen 2 deployments display only private connection details. Public endpoints are not available.
{: note}
