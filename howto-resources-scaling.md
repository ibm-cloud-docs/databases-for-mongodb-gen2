---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: mongodb, databases, scaling, memory, disk IOPS, CPU

subcollection: databases-for-mongodb-gen2

---
 
{{site.data.keyword.attribute-definition-list}}

# Scaling disk, memory, and CPU
{: #resources-scaling}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the  [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

To scale an [Isolated Compute](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-isolated-compute&interface=cli) host flavor instance, set the relevant `hostflavor` parameter to the Isolated Compute size you're targeting, such as "bx3d.4x20.encrypted". The selected host flavor automatically defines the CPU and RAM configuration for the instance, so no separate CPU or RAM selection is required.
{: cli}

To scale an [Isolated Compute](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-isolated-compute&interface=api) host flavor instance, set the relevant `host_flavor` parameter to the Isolated Compute size you're targeting, such as "bx3d.4x20.encrypted". The selected host flavor automatically defines the CPU and RAM configuration for the instance, so no separate CPU or RAM selection is required.
{: api}

To scale an [Isolated Compute](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-isolated-compute&interface=terraform) host flavor instance, set the relevant `host_flavor` parameter to the Isolated Compute size you're targeting, such as "bx3d.4x20.encrypted". The selected host flavor automatically defines the CPU and RAM configuration for the instance, so no separate CPU or RAM selection is required.
{: terraform}

You can manually adjust the amount of resources available to your {{site.data.keyword.databases-for-mongodb}} deployment to suit your workload and the size of your data.

## Resource breakdown
{: #resources-breakdown}

{{site.data.keyword.databases-for-mongodb}} runs with three data members in a cluster, and resources are allocated to all members equally. For example, the minimum disk size of a MongoDB Standard Plan deployment is 30720 MB, which equates to an initial size of 10240 MB per member. The minimum RAM for a MongoDB deployment is 20480 MB per member, provisioned for a three-member configuration.

Billing is based on the _total_ amount of resources that are allocated to the service.
{: tip}

When you provision a deployment, you can select the initial resource allocation of disk and memory. After provision, you can scale your deployment as it needs more resources.

### Disk usage
{: #resources-scaling-disk-usage}

Your disk allocation must be enough to store all of your data. Your data is replicated to both data members so the total amount of disk that you use is at least twice the size of your data set. 

Disk allocation also affects the performance of the disk, with larger disks having higher performance. Baseline Input-Output Operations per second (IOPS) performance for disk is 5 IOPS for each GB. Scale disk to increase the IOPS that your deployment can handle.
 
You cannot scale down storage. If your data set size has decreased, you can recover space by backing up and restoring to a new deployment.
{: tip} 

## Scaling considerations
{: #resources-scaling-scale-consider}

- Scaling your deployment up might cause your databases to restart. If your scaled deployment needs to be moved to a host with more capacity, then the databases are restarted as part of the move.

- Scaling down RAM or CPU does not trigger database node restarts.

- Disk cannot be scaled down.

- Drastically increasing disk can take longer than smaller increases to account for provisioning more underlying hardware resources.

- Scaling operations are logged in [{{site.data.keyword.atracker_full}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-at_events).

## Review current resources and hosting model
{: #review-resources-ui}
{: ui}

In the **Resources** tab, you find both **Hosting model** and **Resource allocations** tiles. These tiles reflect your current resources and hosting model. Select *Configure* to adjust the settings in the *Resource allocations* tile. 

## Scaling in the UI
{: #resources-scaling-scale-ui}
{: ui}

In the **Resources** tab of the UI, select **Configure** on the **Resource allocations** tile. This opens up a panel where you can adjust your resources. 

If your database is on the Isolated Compute hosting model, you then see a "Host sizes" table, where you can select the vCPU and RAM configuration per member for your database. 

The "Disk (GB/member)" slider is your disk selection per member. Drag the slider or adjust the number in the input box to change the number of GB disk. Note that disk is tied to IOPS at 1 GB = 5 IOPS. 

Members is the number of members of your database. For MongoDB, members are set to 3. 

Review your total estimated cost in the calculator on the bottom.

After you are done, click **Apply changes** to trigger the scaling operation.  

## Review current resources and hosting model 
{: #review-resources-cli}
{: cli}

To interact with {{site.data.keyword.databases-for}} on Gen 2 via the CLI you must utilize the IBM Cloud Resource Controller's CLI. For more information, see the [General IBM Cloud CLI (ibmcloud) commands](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_cli).
To get information about a particular instance, use the following command: 

```sh
ibmcloud resource service-instance <INSTANCE_NAME> -o JSON
```
{: pre}


## Resources and scaling in the CLI
{: #resources-scaling-cli}
{: cli}

To update your instance (this includes operations, such as scaling and modifying other parts of your service), use the `ibmcloud resource service-instance-update` command. 

```sh
ibmcloud resource service-instance-update <INSTANCE_NAME> -p '<{FIELDS_TO_UPDATE}>'
```
{: pre}

For example, to update the `host_flavor` of a {{site.data.keyword.databases-for-mongodb}} instance, use a command like:

```sh
ibmcloud resource service-instance-update test-database databases-for-mongodb standard us-south -p '{"host_flavor": "mx3d.8x80.encrypted", "storage_gb": 10 }'
```
{: pre}


### The `host_flavor` parameter
{: #host-flavor-parameter-cli}
{: cli}   

Isolated Compute offers six size options to choose from.

The host_flavor parameter defines your Compute sizing. Input the appropriate value for your desired size.

| Host size | vCPU x RAM           | host_flavor value         |
|-----------|----------------------|---------------------------|
| 4x20      | 4 vCPU x 20 GB RAM   | bx3d.4x20.encrypted        |
| 8x40      | 8 vCPU x 40 GB RAM   | bx3d.8x40.encrypted        |
| 8x80      | 8 vCPU x 80 GB RAM   | mx3d.8x80.encrypted        |
| 16x80     | 16 vCPU x 80 GB RAM  | bx3d.16x80.encrypted       |
| 32x160    | 32 vCPU x 160 GB RAM | bx3d.32x160.encrypted      |
| 48x240    | 48 vCPU x 240 GB RAM | bx3d.48x240.encrypted      |
{: caption="Isolated Compute CLI selections" caption-side="bottom"}

## Review current resources and hosting model
{: #review-resources-api}
{: api}

The _Foundation endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups]((/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-api)) endpoint.

```sh
curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups -H 'Authorization: Bearer <>' \
```
{: pre}

## Scaling with the API
{: #resources-scaling-api}
{: api}

To scale the `host_flavor` of a deployment to `mx3d.8x80.encrypted` for each memory member, use the following command:

```sh
curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances
-H 'Authorization: Bearer <>' 
-H 'Content-Type: application/json' 
-d '{
   "name": "my-instance",
    "target": "ca-mon",
    "resource_group": "5c49eabc-f5e8-5881-a37e-2d100a33b3df",
    "resource_plan_id": "databases-for-postgresql-standard",
    "dataservices": {
      "resources": {
        "database": {
          "host_flavor": "mx3d.8x80.encrypted"
        }
      }
     }
   }'
```
{: pre}

For more information, see the [API reference](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-api).


### The `host_flavor` parameter
{: #host-flavor-parameter-api}
{: api}

Isolated Compute offers six size options to choose from.

The host_flavor parameter defines your Compute sizing. Input the appropriate value for your desired size.

| Host size | vCPU x RAM           | host_flavor value         |
|-----------|----------------------|---------------------------|
| 4x20      | 4 vCPU x 20 GB RAM   | bx3d.4x20.encrypted        |
| 8x40      | 8 vCPU x 40 GB RAM   | bx3d.8x40.encrypted        |
| 8x80      | 8 vCPU x 80 GB RAM   | mx3d.8x80.encrypted        |
| 16x80     | 16 vCPU x 80 GB RAM  | bx3d.16x80.encrypted       |
| 32x160    | 32 vCPU x 160 GB RAM | bx3d.32x160.encrypted      |
| 48x240    | 48 vCPU x 240 GB RAM | bx3d.48x240.encrypted      |
{: caption="Isolated Compute API selections" caption-side="bottom"}

## Review current resources and hosting model
{: #review-resources-terraform}
{: terraform}

Review resource allocations to your database by checking your Terraform scripts for `cpu { allocation_count = }`, `memory {allocation_mb = }`, and `disk { allocation_mb = }`.

## Scaling with Terraform
{: #resources-scaling-terraform}
{: terraform}

Before executing a Terraform script on an existing instance, use the `terraform plan` command to compare the current infrastructure state with the desired state defined in your Terraform files. Any alteration to the `resource_group_id`, `service plan`, `version`, `key_protect_instance`, `key_protect_key`, `backup_encryption_key_crn` attributes recreates your instance. For a list of current argument references with the `Forces new resource` specification, see the [ibm_database Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: important}

Scale your instance by adjusting your Terraform script for the resource you're interested in. In the following example, `host_flavor` and `disk` allocations are specified. 

To implement your change, run `terraform apply`.

```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "databases-for-mongodb-gen2-standard"
  location          = "eu-gb"
  service           = "databases-for-mongodb"
  resource_group_id = data.ibm_resource_group.group.id
  tags              = ["tag1", "tag2"]
  adminpassword     = "password12"
  group {
    group_id = "member"
    cpu {
      allocation_count = 6
    }
    memory {
      allocation_mb = 24576
    }
    disk {
      allocation_mb = 256000
    }
  }
  users {
    name     = "user123"
    password = "password12"
  }
  allowlist {
    address     = "172.168.1.1/32"
    description = "desc"
  }
}
output "ICD MongoDB database connection string" {
  value = "http://${ibm_database.test_acc.ibm_database_connection.icd_conn}"
}
```
{: codeblock}
