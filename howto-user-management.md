---
copyright:
  years: 2026
lastupdated: "2026-11-05"

keywords: mongodb, databases, admin user, service credentials, ops manager, mongodb managing users, roles, root account

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing users and roles
{: #user-management}

[Gen 2]{: tag-purple}

As part of provisioning a new deployment in {{site.data.keyword.cloud}}, you can use the service credential console page to create a user with different roles (`Manager`, `Writer`, and `Reader`).

{{site.data.keyword.databases-for-mongodb}} deployments no longer include a default `admin` user. Instead, you create a user with the `Manager`, `Writer`, or `Reader` role using the {{site.data.keyword.cloud}} _Service Credentials_ interface — via UI or CLI. These users come with necessary credentials to connect to and manage the deployment.

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
	"restore",
	"changeOwnPasswordRole"
```
{: pre}

On the Enterprise Sharding plan, the `Manager` user is also granted the `enableSharding` privilege.
{: note}

The `Manager` user is also granted the `userAdmin` role on all databases that users create in the deployment. This role allows the `Manager` user to create and manage users on those databases. The role is granted within 60 seconds after a database is created. Until then, you can't create users on that database.
{: note}

The `Manager` user can grant the following database-level roles on databases for which it has the `userAdmin` role. For instructions, see [Creating database users and granting database-level roles](#user-manager-database-users).

| Role | Access on the database |
|------|------------------------|
| `read` | Read data. |
| `readWrite` | Read and write data. |
| `dbAdmin` | Administrative tasks, such as managing indexes and collections. No access to data. |
| `userAdmin` | Create users and manage roles. |
| `dbOwner` | All of the above. |
{: caption="Database-level roles that the Manager user can grant" caption-side="bottom"}

The `Manager` user can't grant cluster-wide roles, such as `dbAdminAnyDatabase`, `readWriteAnyDatabase`, `manager`, `writer`, or `reader`. The `Manager` user also can't create users or grant roles on the `admin`, `local`, `config`, and `$external` databases. To get a user with cluster-wide access, create a service credential.
{: note}

Changing the user password is not supported via the {{site.data.keyword.cloud_notm}} console on Gen 2.

### The writer user
{: #user-writer}

The `Writer` user can read and write data on all databases, but can't administer the deployment or manage users. It is automatically granted the following privileges:

```sh
	"readWriteAnyDatabase",
	"changeOwnPasswordRole"
```
{: pre}

On the Enterprise Sharding plan, the `Writer` user is also granted the `enableSharding` privilege.
{: note}

The `Writer` user is not granted the `userAdmin` role, so it can't create users on any database.

### The reader user
{: #user-reader}

The `Reader` user has read-only access to data on all databases. It is automatically granted the below privileges:

```sh
	"readAnyDatabase",
	"changeOwnPasswordRole"
```
{: pre}

The `Reader` user can't modify data, administer the deployment, or manage users.

### Create a user in the CLI
{: #manager_user_set_cli}
{: cli}

Use one of the following commands from the {{site.data.keyword.cloud_notm}} CLI {{site.data.keyword.databases-for}} plug-in to create a `Manager`, `Writer`, or `Reader` user. Replace `<role>` with the role that you want.

```sh
ibmcloud resource service-key-create <service_key_name> <role> --instance-name <instance_name>
```
{: pre}

```sh
ibmcloud resource service-key-create <service_key_name> <role> --instance-id <guid>
```
{: pre}

For example, to create a `Writer` user:

```sh
ibmcloud resource service-key-create <service_key_name> Writer --instance-name <instance_name>
```
{: pre}

### Creating users in the console
{: #user-management-set-admin-password-ui}
{: ui}

1. Go to the service dashboard for your service.
2. Click **Service credentials**.
3. Click **New credential**.
4. Choose a descriptive name for your new credential.
5. In the **Role** field, select `Manager`, `Writer`, or `Reader`.
6. Click **Add** to provision the new credential. A username and password, and an associated database user in the {{site.data.keyword.databases-for-mongodb}} are generated.

### Creating database users and granting database-level roles
{: #user-manager-database-users}

The `Manager` user can create users on databases that users create in the deployment, and can grant database-level roles, including administrator roles, on those databases. Use this role to give an application or a DBA access to a single database. The roles that you grant apply to that database only.

Before you begin:

- Have a `Manager` credential. See [Creating users in the console](#user-management-set-admin-password-ui) or [Create a user in the CLI](#manager_user_set_cli).
- Each database that you create a user in or grant a role on must exist and be at least 60 seconds old. Until then, the `Manager` user doesn't have the `userAdmin` role on it.

1. Connect to the deployment with the `Manager` credential by using the MongoDB Shell. For more information, see [Connecting with the MongoDB Shell](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-connecting-cli-client).
2. Switch to the database where you want to create the user. The user is defined in this database, which also serves as the authentication database when the user connects. In the following steps, this database is referred to as `<db1>`.

   ```js
   use <db1>
   ```
   {: pre}

3. Create the user and grant the required role on each database that the user needs to access. The role can be granted on `<db1>` or on other databases, such as `<db2>` and `<db3>`. The following example grants the `readWrite` role on three databases.

   ```js
   db.createUser({
     user: "<user_name>",
     pwd: passwordPrompt(),
     roles: [
       { role: "readWrite", db: "<db1>" },
       { role: "readWrite", db: "<db2>" },
       { role: "readWrite", db: "<db3>" }
     ]
   })
   ```
   {: pre}

4. To grant an administrator role, use a database-level role. For the available roles, see [The manager user](#user-manager). For example, to create a user that can administer databases but cannot read or write data, grant the `dbAdmin` role.

   ```js
   db.createUser({
     user: "<dba_user_name>",
     pwd: passwordPrompt(),
     roles: [
       { role: "dbAdmin", db: "<db1>" },
       { role: "dbAdmin", db: "<db2>" },
       { role: "dbAdmin", db: "<db3>" }
     ]
   })
   ```
   {: pre}

   To grant roles to an existing user, run `db.grantRolesToUser()` in the database where the user was created.

   ```js
   db.grantRolesToUser("<user_name>", [
     { role: "dbAdmin", db: "<db1>" },
     { role: "dbAdmin", db: "<db2>" },
     { role: "dbAdmin", db: "<db3>" }
   ])
   ```
   {: pre}

5. Connect as the new user. Set `authSource` to the database where you created the user (`<db1>`).

   ```sh
   mongosh "mongodb://<host1>:<port1>,<host2>:<port2>,<host3>:<port3>/<db1>?authSource=<db1>" -u <user_name>
   ```
   {: pre}
