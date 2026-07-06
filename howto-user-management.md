---
copyright:
  years: 2026
lastupdated: "2026-07-02"

keywords: mongodb, databases, admin user, service credentials, ops manager, mongodb managing users, roles, root account

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing users and roles
{: #user-management}

[Gen 2]{: tag-purple}

As part of provisioning a new deployment in {{site.data.keyword.cloud}}, you can use the service credential console page to create a user with different roles (`Manager` and `Writer`).

{{site.data.keyword.databases-for-mongodb}} deployments no longer include a default `admin` user. Instead, you create a user with the `Manager` or `Writer` role using the {{site.data.keyword.cloud}} _Service Credentials_ interface — via UI or CLI. These users come with necessary credentials to connect to and manage the deployment.

### The manager user
{: #user-manager}

The `Manager` user functions as an admin-like user and is automatically granted the below privileges:

```sh
  "dbAdminAnyDatabase",
	"readWriteAnyDatabase",
	"readAnyDatabase",
	"clusterMonitor",
	"clusterManager",
	"backup",
	"restore"
```
{: pre}

Changing the user password is not supported via the {{site.data.keyword.cloud_notm}} console on Gen 2.

### Create the manager user in the CLI
{: #manager_user_set_cli}
{: cli}

Use one of the following commands from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to create the `Manager` user.

```sh
ibmcloud resource service-key-create <service_key_name> Manager --instance-name <instance_name>
```
{: pre}

```sh
ibmcloud resource service-key-create <service_key_name> Manager --instance-id <guid>
```
{: pre}


### Creating users in the console
{: #user-management-set-admin-password-ui}
{: ui}

1. Go to the service dashboard for your service.
2. Click **Service credentials**.
3. Click **New credential**.
4. Choose a descriptive name for your new credential.
5. Click **Add** to provision the new credential. A username and password, and an associated database user in the {{site.data.keyword.databases-for-mongodb}} are generated.
