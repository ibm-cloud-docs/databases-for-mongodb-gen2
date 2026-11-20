---

copyright:
  years: 2026
lastupdated: "2026-11-20"

keywords: databases, ops manager, opsman, mongodbee, enterprise sharding, gen2, isolated compute, monitoring

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing MongoDB Ops Manager
{: #ops-manager-gen2}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Sharding deployments on the Gen 2 platform include access to MongoDB Ops Manager. Ops Manager provides operational visibility, performance metrics, and telemetry for your database clusters. For more information about core capabilities and architecture, see the [MongoDB Ops Manager documentation](https://www.mongodb.com/docs/ops-manager/){: external}.
{: shortdesc}

## Overview of Ops Manager on Gen 2
{: #ops-manager-gen2-overview}

On the Gen 2 platform, {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding is provisioned on the isolated compute hosting model across private cloud endpoints. Ops Manager is integrated with your deployment to help monitor cluster health, shard distributions, and query performance.

Review the following key considerations for Ops Manager on Gen 2:

Unified user credentials
:   You do not need to create a separate Ops Manager user account. When you create a database user, those credentials initially grant access to both the MongoDB database and the Ops Manager interface.

Decoupled password lifecycles
:   Although a database user and an Ops Manager account share the same initial password upon creation, their password lifecycles diverge immediately afterward. Changing a password in one environment does not propagate or synchronize to the other.

Private network access
:   On Gen 2, Ops Manager endpoints are accessible exclusively over private cloud endpoints. Access requires a secure network route from within your VPC or connected private infrastructure.

## Credentials and user management
{: #ops-manager-gen2-credentials}

On Gen 2, user provisioning includes the following workflow:

- When you create a database user (through the {{site.data.keyword.cloud_notm}} console, CLI, or API), the credentials initially grant access to both the database and the Ops Manager console.
- Initial credentials are displayed once upon creation. Store them securely in your secret management system.

### User roles and permissions
{: #ops-manager-gen2-roles}

User access within MongoDB and Ops Manager is governed by assigned roles. When you assign a database user role, it automatically maps to a corresponding role in Ops Manager.

| Database user role | Mapped Ops Manager role | Privileges and access |
|--------------------|-------------------------|-----------------------|
| `Reader` | [`Project Data Access Read Only`](https://www.mongodb.com/docs/ops-manager/current/reference/user-roles/#mongodb-authrole-Project-Data-Access-Read-Only){: external} | Provides read-only access to cluster monitoring metrics, performance telemetry, operational topology, and deployment data. Users cannot modify configurations, alert settings, or data. |
| `Writer` | [`Project Data Access Read/Write`](https://www.mongodb.com/docs/ops-manager/current/reference/user-roles/#mongodb-authrole-Project-Data-Access-Read-Write){: external} | Provides read and write access to deployment data and enables viewing of operational metrics, activity, and cluster performance. |
| `Manager` | [`Project Data Access Admin`](https://www.mongodb.com/docs/ops-manager/current/reference/user-roles/#mongodb-authrole-Project-Data-Access-Admin){: external} | Provides full administrative access within the Ops Manager project, including managing project alerts, integration hooks, team access, and database access settings. |
{: caption="User roles and Ops Manager permission mapping" caption-side="bottom"}

Assign user roles based on the principle of least privilege required for both database operations and operational monitoring in Ops Manager. For more information about granular Ops Manager role capabilities, see the [MongoDB Ops Manager user roles documentation](https://www.mongodb.com/docs/ops-manager/current/reference/user-roles/){: external}.
{: note}

## Connecting to Ops Manager
{: #ops-manager-gen2-connectivity}

Gen 2 deployments operate exclusively with private endpoints on a software-defined VPC architecture. Ops Manager cannot be accessed directly over the public internet.

To reach the Ops Manager web interface, your browser must reside on a network with private routing to the deployment's endpoint.

### Recommended access patterns
{: #ops-manager-gen2-access-patterns}

1. **VPC Virtual Server Instance (VSI):**
   - Deploy a Linux or Windows desktop VSI in the same VPC (or a peered VPC with transit routing) as your {{site.data.keyword.databases-for-mongodb}} deployment.
   - Access the Ops Manager URL through a browser running on or forwarded from the VSI.

1. **VPN Gateway:**
   - Configure an [{{site.data.keyword.cloud_notm}} VPN for VPC](/docs/vpc?topic=vpc-vpn-overview){: external} connection (such as Client-to-Site VPN or Site-to-Site VPN) connecting your local environment or corporate network to the target VPC.
   - Access the private Ops Manager URL directly through your local web browser after the VPN tunnel is established and DNS resolution to the VPC is configured.

Ensure that your VPC security groups and network ACLs allow outbound HTTPS traffic to the database and Ops Manager subnet and port ranges.
{: tip}

## Locating the Ops Manager URL and initial login
{: #ops-manager-gen2-login}

### Retrieving the endpoint details
{: #ops-manager-gen2-find-url}

The Ops Manager connection URL is available in your instance deployment metadata. You can retrieve instance details in JSON format by using the {{site.data.keyword.cloud_notm}} CLI:

```sh
ibmcloud resource service-instance <INSTANCE_NAME_OR_CRN> --output json
```
{: pre}

Locate the Ops Manager web console endpoint in the returned connection metadata.

### Accessing the web interface and accepting the project invitation
{: #ops-manager-gen2-accept-invite}

1. Navigate to the Ops Manager HTTPS URL by using a browser within your connected private network.
1. Sign in by using your database username and initial password.
1. Upon your first login, go to the **Invitations** tab in the Ops Manager interface.
1. Review and click **Accept** on the pending project invitation. This step registers your user profile with your deployment's organization and project workspace.
1. Click the **Ops Manager** logo or navigate to **All Clusters** to view your deployment telemetry, nodes, and sharding topology.

Subsequent logins bypass the invitation workflow and take you directly to the cluster management dashboard.
{: note}

## Managing passwords
{: #ops-manager-gen2-password-management}

Managing passwords for Ops Manager on Gen 2 requires careful operational planning due to decoupled credential stores:

Independent credential stores
:   When a user is first created, the same password is set for both the MongoDB database and the Ops Manager console. However, MongoDB authentication and Ops Manager authentication maintain completely independent credential stores.

No automatic synchronization
:   If you change your database user password (through the database CLI, administrative commands, or client drivers), that change is applied only to database authentication. The Ops Manager user password remains unchanged. Similarly, changing your password inside Ops Manager does not update your database login credentials.

Platform flow decoupling
:   Password updates are not managed through a unified {{site.data.keyword.cloud_notm}} platform flow. Instead, you manage database passwords directly within MongoDB and Ops Manager passwords within the Ops Manager interface.

Self-service password update in Ops Manager
:   Users can update their own Ops Manager password directly within the Ops Manager UI under account settings.

Maintain and track updated credentials in your organization's secure secret management system to avoid administrative lockouts.
{: important}
