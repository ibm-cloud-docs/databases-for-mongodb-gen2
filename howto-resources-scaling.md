---

copyright:
  years: 2025
lastupdated: "2025-10-15"

keywords: mongodb, databases, scaling, memory, disk IOPS, CPU

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Scaling disk, memory, and CPU
{: #resources-scaling}

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

In the **Resources** tab of the UI, select *Configure* on the **Resource allocations** tile. This opens up a panel where you can adjust your resources. 

If your database is on the Isolated Compute hosting model, you then see a "Host sizes" table, where you can select the vCPU and RAM configuration per member for your database. 

The "Disk (GB/member)" slider is your disk selection per member. Drag the slider or adjust the number in the input box to change the number of GB disk. Note that disk is tied to IOPS at 1 GB = 5 IOPS. 

Members is the number of members of your database. For MongoDB, members are set to 3. 

Review your total estimated cost in the calculator on the bottom.

After you are done, click *Apply changes* to trigger the scaling operation.  

## Review current resources and hosting model 
{: #review-resources-cli}
{: cli}

(--confirm if these are still applicable or needs modifications)
[{{site.data.keyword.cloud_notm}} CLI cloud databases plug-in](/docs/databases-cli-plugin?topic=databases-cli-plugin-cdb-reference) supports viewing and scaling the resources on your deployment. Use the command `cdb deployment-groups` to see current resource information for your service, including which resource groups are adjustable. To scale any of the available resource groups, use `cdb deployment-groups-set` command. 

```sh
ibmcloud cdb deployment-groups <INSTANCE_NAME_OR_CRN>
```
{: pre}
(--need updated output)
This command produces the output:

```sh
Group   member
Count   3
|
+   Memory
|   Allocation                      6144mb
|   Allocation per member           2048mb
|   Minimum                         6144mb
|   Step Size                       256mb
|   Adjustable                      true
|   Cpu Enforcement Ratio Ceiling   49152mb
|   Cpu Enforcement Ratio           8192mb

|
+   CPU
|   Allocation              0
|   Allocation per member   0
|   Minimum                 6
|   Step Size               2
|   Adjustable              true
|                           
+   HostFlavor    
|   ID            multitenant
|   Name          
|   HostingSize   
|
+   Disk
|   Allocation              30720mb
|   Allocation per member   10240mb
|   Minimum                 30720mb
|   Step Size               2048mb
|   Adjustable              true
```
{: pre}
(--update)
The deployment has three members, with 6144 MB of RAM and 30720 MB of disk allocated in total. The "per member" allocation is 2048 MB of RAM and 10240 MB of disk. The minimum value is the lowest the total allocation that can be set. The step size is the smallest amount by which the total allocation can be adjusted.

## Resources and scaling in the CLI
{: #resources-scaling-cli}
{: cli}
(--update can the same work for isolated boxes?)
The `cdb deployment-groups-set` command allows either the total RAM or total disk allocation to be set in MB. For example, to scale the memory of the "example-deployment" to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), you use the command:

```sh
ibmcloud cdb deployment-groups-set <INSTANCE_NAME_OR_CRN> member --memory 12288
```
{: pre}

### The `hostflavor` parameter
{: #host-flavor-parameter-cli}
{: cli}   

Isolated Compute offers six size options across legacy and [newer-generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). Newer-generation profiles are [available only in selected regions](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory).

The host_flavor parameter defines your Compute sizing. Input the appropriate value for your desired size.

| Host size | vCPU x RAM           | host_flavor value         |
|-----------|----------------------|---------------------------|
| 4x16      | 4 vCPU x 16 GB RAM   | b3c.4x16.encrypted        |
| 8x32      | 8 vCPU x 32 GB RAM   | b3c.8x32.encrypted        |
| 8x64      | 8 vCPU x 64 GB RAM   | m3c.8x64.encrypted        |
| 16x64     | 16 vCPU x 64 GB RAM  | b3c.16x64.encrypted       |
| 32x128    | 32 vCPU x 128 GB RAM | b3c.32x128.encrypted      |
| 30x240    | 30 vCPU x 240 GB RAM | m3c.30x240.encrypted      |
{: caption="Isolated Compute CLI selections" caption-side="bottom"}


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
(--needs confirmations)
The _Foundation endpoint_ that is shown on the _Overview_ panel of your service provides the base URL to access this deployment through the API. Use it with the `/groups` endpoint if you need to manage or automate scaling programmatically.

To view the current and scalable resources on a deployment, use the [/deployments/{id}/groups](https://cloud.ibm.com/apidocs/cloud-databases-api#get-currently-available-scaling-groups-from-a-depl) endpoint. (-- API doc link needs to be updated once the new page is ready)

```sh
curl -X GET https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups -H 'Authorization: Bearer <>' \
```
{: pre}

## Scaling with the API
{: #resources-scaling-api}
{: api}
(--same or needs changes?)
To scale the memory of a deployment to 4096 MB of RAM for each memory member (for a total memory of 12288 MB), use the following command:

```sh
curl -X PATCH https://api.{region}.databases.cloud.ibm.com/v5/ibm/deployments/{id}/groups/member 
-H 'Authorization: Bearer <>' 
-H 'Content-Type: application/json' 
-d '{"memory": {"allocation_mb": 12288}}' \
```
{: pre}

For more information, see the [API reference](/apidocs/cloud-databases-api/cloud-databases-api-v5#listdeploymentscalinggroups).


### The `host flavor` parameter
{: #host-flavor-parameter-api}
{: api}

Isolated compute offers six size options across legacy and [newer-generation instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#next-gen-profiles). Newer-generation profiles are [available only in selected regions](/docs/vpc?topic=vpc-profiles&interface=ui#vhmemory).

The host_flavor parameter defines your Compute sizing. Input the appropriate value for your desired size.

| Host size | vCPU x RAM           | host_flavor value         |
|-----------|----------------------|---------------------------|
| 4x16      | 4 vCPU x 16 GB RAM   | b3c.4x16.encrypted        |
| 8x32      | 8 vCPU x 32 GB RAM   | b3c.8x32.encrypted        |
| 8x64      | 8 vCPU x 64 GB RAM   | m3c.8x64.encrypted        |
| 16x64     | 16 vCPU x 64 GB RAM  | b3c.16x64.encrypted       |
| 32x128    | 32 vCPU x 128 GB RAM | b3c.32x128.encrypted      |
| 30x240    | 30 vCPU x 240 GB RAM | m3c.30x240.encrypted      |
{: caption="Isolated Compute API selections" caption-side="bottom"}


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
(--swap with host flavor?)
Review resource allocations to your database by checking your terraform scripts for `cpu { allocation_count = }`, `memory {allocation_mb = }`, and `disk { allocation_mb = }`.

## Scaling with Terraform
{: #resources-scaling-terraform}
{: terraform}

Before executing a Terraform script on an existing instance, use the `terraform plan` command to compare the current infrastructure state with the desired state defined in your Terraform files. Any alteration to the `resource_group_id`, `service plan`, `version`, `key_protect_instance`, `key_protect_key`, `backup_encryption_key_crn` attributes recreates your instance. For a list of current argument references with the `Forces new resource` specification, see the [ibm_database Terraform Registry](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database){: external}.
{: important}
(--swap with host-flavor)
Scale your instance by adjusting your Terraform script for the resource you're interested in. In the following example, `host_flavor` and `disk` allocations are specified. 

To implement your change, run `terraform apply`.
(--update)
```terraform
data "ibm_resource_group" "group" {
  name = "<your_group>"
}
resource "ibm_database" "<your_database>" {
  name              = "<your_database_name>"
  plan              = "standard"
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
