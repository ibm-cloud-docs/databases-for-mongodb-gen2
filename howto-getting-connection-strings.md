---

copyright:
  years: 2018, 2023
lastupdated: "2023-06-15"

keywords: mongodb, databases, connection strings

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# Getting connection strings
{: #connection-strings}

Connection strings allow you to establish a connection between your application and your {{site.data.keyword.databases-for-mongodb}} instance.

## Getting connection strings in the UI
{: #connection-strings-ui}
{: ui}

Follow these steps to retrieve your {{site.data.keyword.databases-for-mongodb}} instance connection strings:

1. In your deployment's **Overview page**, scroll down to the *Endpoints* section. 
1. Within the *Endpoints* section, you find a *Quick start tab* with the following sections: 
   - **Connect using a CLI** - This section contains information for connecting to your deployment through the [{{site.data.keyword.IBM_notm}} CLI](https://www.ibm.com/cloud/cli){: external}.
   - **Connect using a MongoDB Enterprise Client** - This section allows you to get a TLS certificate and connect to your deployment.

## Getting connection strings in the CLI
{: #connection-strings-cli}
{: cli}

You can also retrieve connection strings using the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections) Connection command.

The command looks like this: 

```sh
ibmcloud cdb deployment-connections <INSTANCE_NAME_OR_CRN> -u <NEWUSERNAME> [--endpoint-type <ENDPOINT_TYPE>]
```
{: pre}

For more information, see [Connections Command options](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#connections-command-options).

### Command options
{: #connection-strings-cli-command-options}
{: cli}

- If you don't specify a `user`, the Connections commands return information for the `admin` user, by default. 
- If you don't specify an `endpoint-type`, the connection string returns the public endpoint by default. 
- If your deployment has only a private endpoint, specify `--endpoint-type private` or the commands return an error. The user and endpoint type is not enforced. You can use any user on your deployment with either endpoint.

## Getting connection strings in the API
{: #connection-strings-api}
{: api}

To retrieve users' connection strings from the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction){: external}, use the [Connections endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#getconnection){: external}. To create the connection strings, ensure that the path includes the specific user and endpoint type (public or private) that should be used. The user and endpoint type are not restricted or enforced. You have the flexibility to utilize any user available in your deployment, along with either endpoint (if both are present in your deployment).

The API command looks like: 

```sh
curl -X GET -H "Authorization: Bearer <>" 'https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/users/{userid}/connections/{endpoint_type}'
```
{: pre}

Remember to replace {region}, {id}, {userid}, and {endpoint_type} with the appropriate values.
{: note}

## Additional users and connection strings
{: #connection-strings-additional-users-strings}

Access to your {{site.data.keyword.databases-for-mongodb}} deployment is not limited to the `admin` user. Create more users and retrieve connection strings specific to them by using the UI, the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference), or the [{{site.data.keyword.databases-for}} API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#introduction).

All users on your deployment can use the connection strings, including connection strings for either public or private endpoints.

For more information, see the [Managing Users and Roles](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management) page.
