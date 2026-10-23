---

copyright:
  years: 2026
lastupdated: "2026-10-23"

keywords: provision single node, single member, non-ha, single zone, mongodb enterprise sharding, mongodb ees, databases-for-mongodb, single shard

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Provisioning a single-node instance
{: #enterprise-sharding-provisioning-single-node}

[Gen 2]{: tag-purple}

Provision a {{site.data.keyword.databases-for-mongodb_full}} Enterprise Sharding deployment as a single-node, single-zone formation through the [{{site.data.keyword.databases-for}} CLI plug-in](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-cdb-reference){: external}, the [{{site.data.keyword.databases-for}} API](/apidocs/cloud-databases-api/cloud-databases-api-v5){: external}, or through [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: shortdesc}

Single-node provisioning is currently supported only for **{{site.data.keyword.databases-for-mongodb_full}} Enterprise** on Gen 2, and only for **single-shard deployments**. Multi-shard single-node formations are not supported at this time.
{: restriction}

By default, {{site.data.keyword.databases-for-mongodb_full}} Enterprise Sharding deployments are provisioned as highly available formations with three members per shard. The single-node formation described in this topic provisions one member and is intended for non-production use cases such as development, testing, and proof-of-concept workloads. For standard high-availability provisioning, see [Provisioning](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-enterprise-sharding-provisioning).
{: note}

## Considerations for single-node deployments
{: #single-node-considerations}

Before you provision a single-node instance, review the following limitations compared to a standard high-availability deployment.

No high availability
:   A single-node instance runs with one member and no replica set peers. If the node becomes unavailable due to host failure, planned maintenance, or a zone-level disruption, the database is inaccessible until the node recovers. There is no automatic failover.

Reduced service level objectives
:   Single-node instances do not provide the same availability guarantees as multi-member deployments. Review your Service Level Agreement (SLA) carefully before using a single-node instance in any workload that requires continuous availability.

Not recommended for production
:   Single-node formations are suitable for development, testing, and exploratory workloads. Do not use them for production workloads that require resilience, failover, or data redundancy.

Single shard only
:   Single-node deployments are supported only on single-shard instances. You cannot provision a single-node formation on a multi-shard deployment. If your workload requires multiple shards, use a standard high-availability formation instead.

No in-place conversion to HA yet
:   Currently, you cannot convert a single-node instance to a high-availability multi-member formation in place. To move to an HA deployment, provision a new high-availability instance and migrate your data.

## Provisioning through the CLI
{: #single-node-cli}
{: cli}

### Create a single-node instance through the CLI
{: #single-node-create-cli}
{: cli}

Before provisioning, follow the instructions provided in the documentation to install the [{{site.data.keyword.cloud_notm}} CLI tool](/docs/cli?topic=cli-install-ibmcloud-cli){: external}.

1. Log in to {{site.data.keyword.cloud_notm}}. If you use a federated user ID, switch to a one-time passcode (`ibmcloud login --sso`), or use an API key (`ibmcloud login --apikey key or @key_file`) to authenticate. For more information about logging in by using the CLI, see [General CLI (ibmcloud) commands](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login){: external} under `ibmcloud login`.

   ```sh
   ibmcloud login
   ```
   {: pre}

2. Provision your single-node database with the following command. The key parameter is `"members": 1` in the `dataservices.mongodbees` object, which instructs the platform to provision a single-member formation instead of the default three-member high-availability formation.

   ```sh
   ibmcloud resource service-instance-create <INSTANCE_NAME> \
     databases-for-mongodb \
     <SERVICE_PLAN_NAME> \
     <LOCATION> \
     -p '{
       "dataservices": {
         "mongodbees": {
           "host_flavor": "<host_flavor_value>",
           "members": 1
         }
       }
     }'
   ```
   {: pre}

   For example, to provision a single-node instance in Washington DC:

   ```sh
   ibmcloud resource service-instance-create my-single-node-mongodb \
     databases-for-mongodb \
     enterprise-sharding-gen2 \
     us-east \
     -p '{
       "dataservices": {
         "mongodbees": {
           "host_flavor": "bxf.4x16",
           "members": 1
         }
       }
     }'
   ```
   {: pre}

   The fields in the command are described in the table that follows.

   | Field | Description | Flag |
   |-------|-------------|------|
   | `INSTANCE_NAME` [Required]{: tag-red} | The instance name can be any string and is the name that is used on the web and in the CLI to identify the new deployment. | |
   | `SERVICE_NAME` [Required]{: tag-red} | Name or ID of the service. Use `databases-for-mongodb`. | |
   | `SERVICE_PLAN_NAME` [Required]{: tag-red} | `enterprise-sharding-gen2` | |
   | `LOCATION` [Required]{: tag-red} | The location where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. | |
   | `RESOURCE_GROUP` | The resource group name. The default value is `default`. | -g |
   | `--parameters` | JSON file or JSON string of parameters to create the service instance. | -p |
   | `host_flavor` | The compute size for the instance. For more information, see [The `host_flavor` parameter](#single-node-host-flavor-cli). | |
   | `members` [Required]{: tag-red} | Set to `1` to provision a single-node, non-HA formation. Omitting this parameter provisions the default three-member high-availability formation. | |
   {: caption="Basic command format fields" caption-side="bottom"}

   You see a response similar to the following example:

   ```text
   Creating service instance my-single-node-mongodb in resource group default of account USER...
   OK
   Service instance my-single-node-mongodb was created.
   Name:                my-single-node-mongodb
   ID:                  crn:v1:bluemix:public:databases-for-mongodb:us-east:a/40ddc34a846383BGB5b60e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                dd13152c-fe15-4bb6-af94-fde0af56897
   Location:            us-east
   State:               provisioning
   Type:                service_instance
   Sub Type:            Public
   Service Endpoints:   private
   Allow Cleanup:       false
   Locked:              false
   Created at:          2026-06-26T19:42:07Z
   Updated at:          2026-06-26T19:42:07Z
   Last Operation:
                        Status    create in progress
                        Message   Started create instance operation
   ```
   {: codeblock}

3. To check provisioning status, use the following command:

   ```sh
   ibmcloud resource service-instance <INSTANCE_NAME>
   ```
   {: pre}

   When complete, you see a response similar to the following example:

   ```text
   Retrieving service instance my-single-node-mongodb in resource group default under account USER's Account as USER...
   OK

   Name:                  my-single-node-mongodb
   ID:                    crn:v1:bluemix:public:databases-for-mongodb:us-east:a/40ddc34a953a8c02f109835656860e:dd13152c-fe15-4bb6-af94-fde0af5303f4::
   GUID:                  dd13152c-fe15-4bb6-af94-fde5654765
   Location:              us-east
   Service Name:          databases-for-mongodb
   Service Plan Name:     enterprise-sharding-gen2
   Resource Group Name:   default
   State:                 active
   Type:                  service_instance
   Sub Type:              Public
   Locked:                false
   Service Endpoints:     private
   Created at:            2026-06-26T19:42:07Z
   Created by:            USER
   Updated at:            2026-06-26T19:53:25Z
   Last Operation:
                          Status    create succeeded
                          Message   Provisioning mongodb with version 8 (100%)
   ```
   {: codeblock}

4. Optional: To delete a service instance, run the following command:

   ```sh
   ibmcloud resource service-instance-delete <INSTANCE_NAME_OR_CRN>
   ```
   {: pre}

### The `host_flavor` parameter
{: #single-node-host-flavor-cli}
{: cli}

The `host_flavor` parameter defines the compute sizing for your instance. To provision an instance, input the appropriate value for your desired CPU and RAM configuration.

| Host size | vCPU x RAM           | host_flavor value     |
|-----------| ---------------------| ----------------------|
| 4x16      | 4 vCPU x 16 GB RAM   | bxf.4x16              |
| 4x20      | 4 vCPU x 20 GB RAM   | bx3d.4x20.encrypted   |
| 8x40      | 8 vCPU x 40 GB RAM   | bx3d.8x40.encrypted   |
| 16x80     | 16 vCPU x 80 GB RAM  | bx3d.16x80.encrypted  |
| 32x160    | 32 vCPU x 160 GB RAM | bx3d.32x160.encrypted |
| 48x240    | 48 vCPU x 240 GB RAM | bx3d.48x240.encrypted |
{: caption="Host flavor sizing parameter" caption-side="bottom"}

CPU and RAM autoscaling is not supported on {{site.data.keyword.databases-for}} isolated compute. Disk autoscaling is planned for a future release. If you provisioned an isolated instance or switched from a deployment with autoscaling, monitor your resources by using [{{site.data.keyword.monitoringfull}} integration](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-monitoring){: external}, which provides metrics for memory, disk space, and disk I/O utilization. To add resources to your instance, manually scale your deployment.
{: note}

## Provisioning through the Resource Controller API
{: #single-node-api}
{: api}

Follow these steps to provision a single-node instance by using the [Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external}.

1. Obtain an [IAM token](https://cloud.ibm.com/apidocs/resource-controller/resource-controller#authentication){: external}.

2. Obtain the ID of the resource group that you want to deploy to:

   ```sh
   curl -X GET "https://resource-controller.cloud.ibm.com/v2/resource_groups?account_id=<YOUR_ACCOUNT>" \
     -H "Authorization: Bearer <TOKEN>"
   ```
   {: pre}

3. Provision the instance by calling the Resource Controller API. Set `"members": 1` in the `dataservices.mongodbees` object to request a single-node formation.

   ```sh
   curl -X POST \
     https://resource-controller.cloud.ibm.com/v2/resource_instances \
     -H "Authorization: Bearer <TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "<INSTANCE_NAME>",
       "target": "<REGION>",
       "resource_group": "<RESOURCE_GROUP_ID>",
       "resource_plan_id": "databases-for-mongodb-gen2-enterprise-sharding",
       "dataservices": {
         "mongodbees": {
           "host_flavor": "<host_flavor_value>",
           "members": 1
         }
       }
     }'
   ```
   {: pre}

   ### Example
   {: #single-node-api-example}
   {: api}

   To provision a single-node instance in Washington DC:

   ```sh
   curl -X POST \
     https://resource-controller.cloud.ibm.com/v2/resource_instances \
     -H "Authorization: Bearer <IAM_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "my-single-node-mongodb",
       "target": "us-east",
       "resource_group": "5c49eabc-f5e8-5881-a37e-2d100a33b3df",
       "resource_plan_id": "databases-for-mongodb-gen2-enterprise-sharding",
       "dataservices": {
         "mongodbees": {
           "host_flavor": "bxf.4x16",
           "members": 1
         }
       }
     }'
   ```
   {: pre}

   The fields in the request body are described in the table that follows.

   | Field | Description |
   |-------|-------------|
   | `name` [Required]{: tag-red} | The instance name can be any string and is the name used on the web and in the CLI to identify the new deployment. |
   | `target` [Required]{: tag-red} | The region where you want to deploy. To retrieve a list of regions, use the `ibmcloud regions` command. |
   | `resource_group` [Required]{: tag-red} | The ID of the resource group to deploy into. |
   | `resource_plan_id` [Required]{: tag-red} | Use `databases-for-mongodb-gen2-enterprise-sharding` for {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding on Gen 2. |
   | `host_flavor` | The compute size for the instance. See [The `host_flavor` parameter](#single-node-host-flavor-cli). |
   | `members` [Required]{: tag-red} | Set to `1` to provision a single-node, non-HA formation. Omitting this parameter provisions the default three-member high-availability formation. |
   {: caption="API request body fields" caption-side="bottom"}

Since the `host_flavor` selection includes CPU and RAM sizes, this request does not accept both an isolated size selection and separate CPU and RAM allocation selections.
{: note}

## Provisioning with Terraform
{: #single-node-terraform}
{: terraform}

**Before you begin:**

- [Install the Terraform CLI and the IBM Cloud Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#tf_installation){: external}.
- Make sure you have an [{{site.data.keyword.cloud}} API key](/docs/iam?topic=iam-userapikey&interface=ui#create_user_key){: external}.

Use the [`ibm_database` resource](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external} to provision a single-node {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding instance. Set the `members` argument to `1` in your configuration to request a single-member formation.

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}

resource "ibm_database" "mongodb_single_node" {
  name              = "<your_database_name>"
  plan              = "enterprise-sharding-gen2"
  location          = "us-east"
  service           = "databases-for-mongodb"
  service_endpoints = "private"
  tags              = ["tag1", "tag2"]

  group {
    group_id = "member"
    members {
      allocation_count = 1
    }
    host_flavor {
      id = "bxf.4x16"
    }
    disk {
      allocation_mb = 102400
    }
  }

  resource_group_id = data.ibm_resource_group.group.id

  timeouts {
    create = "120m"
    update = "120m"
    delete = "15m"
  }
}
```
{: codeblock}

[VERIFY: Confirm the correct Terraform argument for setting a single-member formation. The API and CLI use `"members": 1` inside `dataservices.mongodbees`. Confirm whether this maps to `members { allocation_count = 1 }` inside the `group` block, or whether a top-level `members` argument is used instead, with the development or Terraform provider team.]
{: note}

The fields relevant to single-node provisioning are described in the table that follows.

| Field | Description |
|-------|-------------|
| `plan` [Required]{: tag-red} | Must be `enterprise-sharding-gen2` for {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding on Gen 2. |
| `location` [Required]{: tag-red} | The region where you want to deploy. |
| `group.group_id` | Must be `member` to configure the member group for the instance. |
| `group.members.allocation_count` | Set to `1` to provision a single-node, non-HA formation. Omitting this, or setting it to `3`, provisions the default high-availability formation. |
| `group.host_flavor.id` | The compute size for the instance. See the `host_flavor` values in [The `host_flavor` parameter](#single-node-host-flavor-cli). |
| `timeouts.create` | The maximum time allowed for the provisioning operation to complete. Set to `120m` for single-node instances. |
{: caption="Terraform single-node provisioning fields" caption-side="bottom"}
