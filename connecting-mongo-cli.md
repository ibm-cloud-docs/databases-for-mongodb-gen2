---
copyright:
  years: 2019, 2025
lastupdated: "2025-09-24"

keywords: mongodb, databases, mongo shell, mongosh

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Connecting with the MongoDB Shell
{: #connecting-cli-client}

You can access your MongoDB database from a command-line client, which allows for direct interaction and monitoring of the data structures that are created within the database. Use it to query and update data, as well to perform administrative operations and monitor performance.

## Installing
{: #connecting-cli-client-install}

The MongoDB shell is available as part of the MongoDB distribution. Download it [here](https://www.mongodb.com/try/download/shell){: .external}.

## Connecting
{: #connecting-cli-connect}

Connection strings are displayed in the _Endpoints_ panel of your deployment's _Overview_, and can also be retrieved from the [cloud databases CLI plugin](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference#deployment-connections), and the [API](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#getconnection).

The information the MongoDB shell needs to connect to your instance is in the "cli" section of the connection strings. The table contains a breakdown for reference.

| Field name | Index | Description |
| ---------- | ----- | ----------- |
| `Bin` | | The recommended binary to create a connection; in this case it is `mongosh`. |
| `Composed` | | A formatted command to establish a connection to your deployment. The command combines the `Bin` executable, `Environment` variable settings, and uses  |`Arguments` as command-line parameters.
| `Environment` | | A list of key/values you set as environment variables. |
| `Arguments` | `0...` | The information that is passed as arguments to the command shown in the Bin field. |
| `Certificate` | `Base64` | A service proprietary certificate that is used to confirm that an application is connecting to the appropriate server. It is base64 encoded. |
| `Certificate` | `Name` | The allocated name for the service proprietary certificate. |
| `Type` | | The type of package that uses this connection information; in this case `cli`.  |
{: caption="mongo/cli connection information" caption-side="top"}

* `0...` indicates that there might be one or more of these entries in an array.

## MongoDB Shell example
{: #connecting-cli-client-mongo-example}

```sh
mongosh -u admin -p $PASSWORD --tls --tlsCAFile c5f07836-d94c-11e8-a2e9-62ec2ed68f84 --authenticationDatabase admin --host replset/bd574ce4-7b36-4274-9976-96db98a3ac10-0.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:30484,bd574ce4-7b36-4274-9976-96db98a3ac10-1.b8a5e798d2d04f2e860e54e5d042c915.databases.appdomain.cloud:30484
```

* `mongosh` - The command itself.
* `-u` - The parameter for the username.
* `-p` - The parameter for the password.
* `--tls --tlsCAFile` - The path and name of the service proprietary certificate for your deployment.
* `--authenticationDatabase` - The database where the user and its credentials are created and stored.
* `--host` - The replica set name, followed by a `/`, and the hosts of the replica set members.

## Starting the MongoDB Shell from the IBM Cloud CLI
{: #connecting-cli-client-ibmcloud-cli}

If the MongoDB Shell is locally installed, you can use the `ibmcloud cdb deployment-connections` command to handle everything that is involved in creating the client connection. For example, to connect to a deployment named "example-mongo" with an "example-user", use the following command.

```sh
ibmcloud cdb deployment-connections --start -u example-user example-mongo
```
{: pre}

The command prompts for the user's password and then runs the MongoDB command-line client to connect to the database.

The option `--start` must come before the parameters, otherwise connection information is returned and the MongoDB Shell is not started.
{: .tip}

## Using the service proprietary certificate
{: #connecting-cli-client-cert}

1. Copy the certificate information from the _Endpoints_ panel or the Base64 field of the connection information.
2. If needed, decode the Base64 string into text.
3. Save the certificate to a file. (You can use the Name that is provided or your own file name).
4. Provide the path to the certificate to the `--tlsCAFile` parameter.

You can display the decoded certificate for your deployment with the CLI plug-in with the command:

```sh
ibmcloud cdb deployment-cacert <INSTANCE_NAME_OR_CRN>
```
{: pre}

It decodes the base64 into text. Copy and save the command's output to a file and provide the file's path to the `--tlsCAFile` parameter.
