---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: mongodb standard migration, gen1 to gen2, classic infrastructure, vpc, backup and restore, migration hub, terraform, ibmcloud cli, private endpoints, version upgrade, context-based restrictions

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.databases-for-mongodb}} from Gen 1 to Gen 2
{: #migrating-mongodb-standard-gen1-to-gen2}

[Gen 2]{: tag-purple}

Use this tutorial to migrate your {{site.data.keyword.databases-for-mongodb}} deployment from Gen 1 (Classic) to Gen 2 (VPC) using the Migration hub in the {{site.data.keyword.cloud_notm}} console, the {{site.data.keyword.cloud_notm}} CLI, the {{site.data.keyword.cloud_notm}} Resource Controller API, or Terraform.
{: shortdesc}

MongoDB Standard plan does not support read replicas or continuous replication tools. The only migration path is backup and restore.

## Key considerations before you begin
{: #key-considerations}

Review these requirements before you start the migration. They apply to all interfaces.

### Version requirement
{: #version-requirement}

MongoDB 8.0 is the only version available on Gen 2. The migration path you follow depends on the version running on your Gen 1 source. Cross-version restore from v7.0 to v8.0 is supported.

| Source version on Gen 1 | Migration path | Target version on Gen 2 |
| --- | --- | --- |
| 8.0 | Backup and restore (same version) | 8.0 |
| 7.0 | Backup and restore (cross-version) | 8.0 |
{: caption="Version readiness for Gen 1 to Gen 2 migration" caption-side="bottom"}

If you prefer to upgrade from v7.0 to v8.0 on Gen 1 before migrating, that option is available as a separate step before starting the migration.

For in-place upgrade instructions, see [Upgrading MongoDB](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.

### Planned downtime
{: #planned-downtime}

The backup and restore path requires a planned maintenance window. For a production migration, the Gen 1 source is recommended to be set to read-only while the backup is taken, and remain unavailable to write traffic until your applications are switched to Gen 2. Plan downtime duration based on your data volume and the time needed to complete the backup, provision, restore, and validate the Gen 2 deployment.

### Private endpoints
{: #private-endpoints-requirement}

Gen 2 deployments use private endpoints only. Public endpoints are not available on Gen 2. Before you start the migration, confirm that your application infrastructure can reach private service endpoints. If your applications currently connect over public endpoints, plan the network changes before you begin.

## Planning considerations
{: #planning-considerations}

Keep these points in mind when planning your migration:

* Deployments on v7.0 or v8.0 can both use backup and restore. Cross-version restore from v7.0 to v8.0 is supported.
* Gen 2 supports private endpoints only. Plan your VPE gateway and any VPN or VSI connectivity before you start the migration.
* Downtime duration depends on data volume. For large deployments, estimate backup and restore time in a non-production environment first.
* Test the full procedure in a non-production environment before scheduling a production cutover.

## Before you begin
{: #prereqs}

Confirm the following before you start the migration:

* You have a Gen 1 {{site.data.keyword.databases-for-mongodb}} deployment to migrate.
* You have access to provision a Gen 2 deployment in the target account, resource group, and region.
* A maintenance window is approved for the migration activity.
* You have access to the application configuration so that you can update connection details during cutover.

Run through these checks on the source deployment:

* Note the current MongoDB version (v7.0 or v8.0).
* Confirm there are no in-progress operations such as scaling, backups, or upgrades.
* Verify that a recent successful backup exists.
* Check disk utilization and available capacity.
* Identify whether applications connect over public endpoints, private endpoints, or both.

Run the migration against a development or low-traffic instance first to validate the procedure before scheduling a production cutover.
{: tip}

## Migrating by using the console
{: #console-migration}
{: ui}

The Migration hub walks you through the migration in a guided wizard, split into two phases. Phase 1 prepares and provisions your Gen 2 target. Phase 2 handles the cutover and closes out the migration.

## Backup and restore migration
{: #backup-restore-migration}
{: ui}

The backup and restore path provisions a new Gen 2 deployment from a backup of the Gen 1 source. For a production migration, stopping writes to Gen 1 before taking the backup ensures the restore captures the latest data. For dev or test migrations, you can skip the write-stop step and restore from the most recent automatic backup instead.

* [Backup and restore Phase 1: Prepare and provision](#br-phase-1)
   * [Backup and restore Step 1: Select migration path](#br-select-path)
   * [Backup and restore Step 2: Assess Gen 1 deployment](#br-assess-gen1)
   * [Backup and restore Step 3: Back up your Gen 1 deployment](#br-backup-gen1)
   * [Backup and restore Step 4: Create a Gen 2 deployment from the backup](#br-provision-gen2)
* [Backup and restore Phase 2: Connect and cut over](#br-phase-2)
   * [Backup and restore Step 5: Configure network access](#br-configure-network-access)
   * [Backup and restore Step 6: Cut over applications](#br-cutover)
   * [Backup and restore Step 7: Complete migration](#br-complete-migration)

## Backup and restore Phase 1: Prepare and provision
{: #br-phase-1}
{: ui}

Phase 1 covers the preparation, backup, and provisioning steps required before the Gen 2 instance is ready.

### Backup and restore Step 1: Select migration path
{: #br-select-path}
{: step}
{: ui}

Open the Migration hub in the {{site.data.keyword.cloud_notm}} console. Under **Data services**, select **Eligible data instances** to see the Gen 1 Classic deployments in your account that are ready to migrate. Find your {{site.data.keyword.databases-for-mongodb}} instance, click **Open wizard**, then select **Backup and restore** and click **Start**.

### Backup and restore Step 2: Assess Gen 1 deployment
{: #br-assess-gen1}
{: step}
{: ui}

After you select the backup and restore path, the wizard runs an automated assessment of your Gen 1 source deployment to confirm it is ready to migrate. The assessment surfaces any required actions and items that need your manual attention before you can proceed.

The wizard surfaces the current MongoDB version and confirms backup and restore compatibility. Both v7.0 and v8.0 sources are supported. If the source is on v7.0, the restore to Gen 2 will target v8.0 directly.

The wizard also surfaces recommendations and contextual information about what changes between Gen 1 and Gen 2. Review these before proceeding, as some settings behave differently on Gen 2.

After you address any flagged items, use **Refresh** to update the assessment results. When the assessment is clear, click **Next: Backup Gen 1** to continue.

### Backup and restore Step 3: Back up your Gen 1 deployment
{: #br-backup-gen1}
{: step}
{: ui}

This step takes a backup of the Gen 1 source to use as the restore source for the new Gen 2 deployment. The step has two parts: optionally stopping writes, then selecting a backup option.

#### Stop writes to Gen 1 (optional for production, skip for dev/test)
{: #br-stop-writes}
{: ui}

For a production migration, set the Gen 1 (Classic) source to read-only before creating the backup. This ensures the backup captures a consistent final state with no in-flight writes. You can also disconnect all active connections to prevent any new writes during the backup window.

Do not stop writes for dev or test migrations. Setting the source to read-only before you are ready for final cutover can result in data loss on the active instance.
{: important}

Once writes are stopped, the wizard shows a **Re-enable writes to Gen 1** option in case you need to roll back before taking the backup.

#### Select a backup option
{: #br-select-backup}
{: ui}

The wizard presents two backup options:

| Option | Description | Best for |
| --- | --- | --- |
| Create on-demand backup | Creates a new backup so that the restore uses the most recent data from the Gen 1 deployment. | Production migrations where data completeness is required. |
| Use last backup | Restores from the most recent completed automatic backup. Changes made after that backup are not included. | Development and test migrations where speed matters more than data completeness. |
{: caption="Backup options for Gen 1" caption-side="bottom"}

For a production migration, use **Create on-demand backup** to ensure the restore includes the latest data.
{: tip}

### Backup and restore Step 4: Create a Gen 2 deployment from the backup
{: #br-provision-gen2}
{: step}
{: ui}

This step provisions a new Gen 2 (VPC) instance restored from the Gen 1 backup. The Gen 1 source remains available and continues to be billed while the new instance is provisioned. If the source is on v7.0, the Gen 2 instance is restored to v8.0.

The wizard shows a **Preview MongoDB on Gen 2** summary with the proposed configuration, covering details, resource allocations, service settings, and backup settings. Fields marked **Changed** highlight differences from the Gen 1 source.

Click **Edit configurations** to adjust the Gen 2 settings before provisioning begins. The edit panel shows your Gen 1 resource allocation for reference and lets you configure:

* **Location and platform**: Target region. Platform is locked to Gen 2.
* **Service details**: Instance name, resource group, hosting model, and tags.
* **Resource allocation**: Host size (vCPU and RAM), disk, and member count.
* **Service configuration**: Database version (8.0) and encryption.

Size the Gen 2 deployment to handle production traffic.

Disk allocation should meet or exceed the Gen 1 source.
{: note}

Once you confirm, the wizard starts the restore and a progress bar tracks completion. The **Next: Set network access** button becomes available once the Gen 2 instance is running.

## Backup and restore Phase 2: Connect and cut over
{: #br-phase-2}
{: ui}

Phase 2 covers the steps to configure network access, perform the cutover, and finish the migration.

### Backup and restore Step 5: Configure network access
{: #br-configure-network-access}
{: step}
{: ui}

Configure private connectivity to the Gen 2 (VPC) instance before cutover. All connection types require a Virtual Private Endpoint (VPE) gateway for private access to the Gen 2 deployment.

Click **Open VPE gateway** to create the gateway from the VPC networking console. Use **Refresh** after setup to confirm the configuration is detected by the wizard.

The VPE gateway is required. The **Next: Cutover** button is not available until this step is complete.
{: important}

### Backup and restore Step 6: Cut over applications
{: #br-cutover}
{: step}
{: ui}

With the backup and restore path, the Gen 1 source is in read-only state if you stopped writes in Phase 1. The cutover step focuses on switching your applications to the new Gen 2 deployment.

**Update application endpoints**

Update your application to use the Gen 2 (VPC) endpoint instead of the Gen 1 (Classic) endpoint. The wizard provides the full connection endpoint string, hostname, port, database name, and SSL mode for easy reference.

Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.
{: note}

**Update authentication credentials**

If your application already uses Gen 2 service credentials, no changes are needed. Otherwise, create a new service credential for the Gen 2 deployment and update your application configuration.

**Redeploy or restart applications**

Redeploy or restart your applications to apply the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

### Backup and restore Step 7: Complete migration
{: #br-complete-migration}
{: step}
{: ui}

Once your applications are confirmed to be running on Gen 2, click **Next: Finish migration** to complete the workflow.

The Finish migration screen shows a side-by-side summary of both instances, including instance status, active connections, last backup timestamp, and disk usage. Additional metrics for both instances are available through {{site.data.keyword.monitoringlong}}.

After finishing, the Gen 1 deployment stays available. Keep it running through your planned retention period before decommissioning, in line with your rollback policy.

Continue to [Post-migration tasks](#post-migration) to wrap up.

## Migrating by using the CLI
{: #cli-migration}
{: cli}

You can migrate your {{site.data.keyword.databases-for-mongodb}} instance from Gen 1 (Classic) to Gen 2 (VPC) by using the {{site.data.keyword.cloud_notm}} CLI.

Before you begin:

- Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.
- Install the {{site.data.keyword.databases-for}} plug-in:

   ```bash
   ibmcloud plugin install cloud-databases -f
   ```
   {: pre}

- Log in and target the resource group where the Gen 2 instance will be created:

   ```bash
   ibmcloud login --apikey "$IBMCLOUD_API_KEY"
   ibmcloud target -g <resource-group>
   ```
   {: pre}

Migration to Gen 2 is initially available in the `us-east` (Washington, DC) region, with additional regions to follow. Ensure that the region you specify is a supported Gen 2 region, such as `us-east`.
{: note}

### Backup and restore migration with the CLI
{: #cli-backup-restore-migration}
{: cli}

The backup and restore path creates a new Gen 2 instance from a backup of the Gen 1 source. All writes to Gen 1 after the backup are not included in the Gen 2 instance. Plan a maintenance window and stop application writes before taking a backup to ensure a consistent migration.

#### Phase 1: Capture backup and provision Gen 2
{: #cli-br-phase-1}
{: cli}

1. **Set environment variables.**

   ```bash
   export CLASSIC_NAME=<gen1-instance-name>
   export GEN2_NAME=<gen2-instance-name>
   export GEN2_REGION=us-east
   ```
   {: pre}

2. **Get the Gen 1 CRN.**

   ```bash
   CLASSIC_CRN="$(ibmcloud resource service-instance "$CLASSIC_NAME" --output json | jq -r '.[0].crn')"
   echo "$CLASSIC_CRN"
   ```
   {: pre}

3. **Verify the MongoDB version on Gen 1.**

   Confirm that the source deployment is on v7.0 or v8.0:

   ```bash
   ibmcloud cdb deployment-about "$CLASSIC_CRN"
   ```
   {: pre}

   Both v7.0 and v8.0 are supported. A v7.0 source is restored directly to v8.0 on Gen 2. For more information, see [Version requirement](#version-requirement).

4. **Stop writes to Gen 1 and take an on-demand backup.**

   For production migrations, stop application writes to ensure a consistent restore point, then trigger a backup. For development or test migrations where data completeness is not required, you can skip this step.

   ```bash
   ibmcloud cdb backup-now "$CLASSIC_CRN"
   ```
   {: pre}

5. **Retrieve the backup CRN.**

   List backups and wait until one is completed and restorable:

   ```bash
   ibmcloud cdb backups "$CLASSIC_CRN"
   ```
   {: pre}

   The `restore_backup_id` must be the full coupled backup CRN in the following format:

   ```text
   crn:v1:bluemix:public:databases-for-mongodb:<region>:a/<account_id>:<instance_id>:backup:<backup_id>
   ```
   {: screen}

   If the output shows only a backup UUID, compose the full CRN from the Gen 1 CRN:

   ```bash
   BACKUP_UUID=<backup-guid>
   BACKUP_CRN="${CLASSIC_CRN%::}:backup:${BACKUP_UUID}"
   ```
   {: pre}

   Passing only the backup UUID causes provisioning to fail. You must use the full backup CRN.
   {: important}

6. **Prepare the provisioning parameters file.**

   Create a `params.json` file with the following content. The `storage_gb` value is per member, in GB, and must meet or exceed the Gen 1 source data volume. Set `version` to `"8.0"` regardless of the Gen 1 source version:

   ```json
   {
     "dataservices": {
       "mongodb": {
         "version": "8.0",
         "members": 3,
         "storage_gb": 20,
         "host_flavor": "bx3d.4x20"
       },
       "restore_backup_id": "<full backup CRN from the previous step>"
     }
   }
   ```
   {: codeblock}

   Or generate it from the variable captured earlier:

   ```bash
   jq --arg crn "$BACKUP_CRN" '.dataservices.restore_backup_id = $crn' params.json.example > params.json
   ```
   {: pre}

   Do not use the flat `{"backup_id": ...}` format that is used for Classic restores. In Gen 2, specify the backup CRN in `dataservices.restore_backup_id`.

7. **Create the Gen 2 instance from the backup.**

   ```bash
   ibmcloud resource service-instance-create "$GEN2_NAME" \
     databases-for-mongodb standard-gen2 "$GEN2_REGION" \
     -p @params.json
   ```
   {: pre}

8. **Wait until the instance is active.**

   Restore can take several hours depending on data volume. Poll the status until `State` shows `active`:

   ```bash
   ibmcloud resource service-instance "$GEN2_NAME"
   ```
   {: pre}

   Do not delete the Gen 1 instance while the restore is running. Deleting the source also deletes its backups.
   {: important}

#### Phase 2: Configure access, cut over, and finish
{: #cli-br-phase-2}
{: cli}

1. **Configure a VPE gateway for private network access.**

   Gen 2 supports private endpoints only. Before switching applications, create a Virtual Private Endpoint (VPE) gateway from the [{{site.data.keyword.cloud_notm}} VPC networking console](https://cloud.ibm.com/infrastructure/provision/endpointGateway){: external} to connect to the new Gen 2 instance.

2. **Create service credentials.**

   ```bash
   ibmcloud resource service-key-create "${GEN2_NAME}-credentials" \
     --instance-name "$GEN2_NAME" \
     -p '{"role":"Manager"}'
   ```
   {: pre}

   Retrieve the credentials and connection details:

   ```bash
   ibmcloud resource service-key "${GEN2_NAME}-credentials" --output json
   ```
   {: pre}

   Pass `role` as a JSON parameter (`-p '{"role":"Manager"}'`), not as the positional `ROLE_NAME` argument. The `databases-for-mongodb` service does not have a `Manager` IAM role, and the CLI fails if `Manager` is passed positionally.
   {: note}

3. **Update application endpoints.**

   Update your application to use the Gen 2 private endpoint from the credentials output instead of the Gen 1 endpoint.

   Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.

4. **Redeploy or restart applications.**

   Restart your applications with the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

5. **Finish and decommission Gen 1.**

   After validating that your applications are running correctly on Gen 2, proceed to [Post-migration tasks](#post-migration). Keep the Gen 1 deployment available through your planned retention period before decommissioning it:

   ```bash
   # ibmcloud resource service-instance-delete "$CLASSIC_NAME"   # only after cutover and retention period
   ```
   {: pre}

## Migrating by using the API
{: #api-migration}
{: api}

You can migrate your {{site.data.keyword.databases-for}} instance from Gen 1 (Classic) to Gen 2 (VPC) by using the [{{site.data.keyword.cloud_notm}} Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external} and the [{{site.data.keyword.databases-for}} API](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-api){: external}.

Before you begin, obtain an IAM token:

```bash
IAM_TOKEN=$(curl -s -X POST "https://iam.cloud.ibm.com/identity/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" \
  | jq -r '.access_token')
```
{: pre}

Migration to Gen 2 is initially available in the `us-east` (Washington, DC) region, with additional regions to follow. Ensure that the region you specify is a supported Gen 2 region, such as `us-east`.
{: note}

### Backup and restore migration with the API
{: #api-backup-restore-migration}
{: api}

The backup and restore path creates a new Gen 2 instance from a backup of the Gen 1 source. All writes to Gen 1 after the backup are not included in the Gen 2 instance. Plan a maintenance window and stop application writes before taking a backup to ensure a consistent migration.

#### Phase 1: Capture backup and provision Gen 2
{: #api-br-phase-1}
{: api}

1. **Get the Gen 1 resource group ID and CRN.**

   Retrieve the resource group ID for the resource group where you plan to create the Gen 2 instance:

   ```bash
   curl -s -X GET \
     "https://resource-controller.cloud.ibm.com/v2/resource_groups?account_id=$ACCOUNT_ID" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '.resources[] | {name, id}'
   ```
   {: pre}

   Retrieve the Gen 1 instance CRN:

   ```bash
   CLASSIC_CRN=$(curl -s -X GET \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances?name=$CLASSIC_NAME" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     | jq -r '.resources[0].crn')
   echo "$CLASSIC_CRN"
   ```
   {: pre}

2. **Verify the database version on Gen 1.**

   Confirm that the source deployment is running the required version before proceeding. If the source is not on the required version, complete an in-place upgrade on Gen 1 first. For more information, see [Version requirement](#version-requirement).

   Set `GEN2_REGION` to the target Gen 2 region (for example, `us-east`):

   ```bash
   GEN2_REGION=<target-gen2-region>
   ```
   {: pre}

   Then encode the Gen 1 CRN and verify the version:

   ```bash
   ENCODED_CRN=$(jq -rn --arg v "$CLASSIC_CRN" '$v | @uri')
   curl -s -X GET \
     "https://api.${GEN2_REGION}.databases.cloud.ibm.com/v5/ibm/deployments/${ENCODED_CRN}" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '.deployment.version'
   ```
   {: pre}

3. **Stop writes to Gen 1 and take an on-demand backup.**

   For production migrations, stop application writes to ensure a consistent restore point, then trigger a backup. For development or test migrations where data completeness is not required, you can skip this step.

   ```bash
   curl -s -X POST \
     "https://api.${GEN2_REGION}.databases.cloud.ibm.com/v5/ibm/deployments/${ENCODED_CRN}/backups" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     -H "Content-Type: application/json" | jq '.task'
   ```
   {: pre}

4. **Retrieve the backup CRN.**

   List backups and wait until one is completed and restorable:

   ```bash
   curl -s -X GET \
     "https://api.${GEN2_REGION}.databases.cloud.ibm.com/v5/ibm/deployments/${ENCODED_CRN}/backups" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '.backups[] | {id, status, is_restorable}'
   ```
   {: pre}

   Each backup's `id` is already the full backup CRN. Capture the most recent restorable backup automatically:

   ```bash
   BACKUP_CRN=$(curl -s -X GET \
     "https://api.${GEN2_REGION}.databases.cloud.ibm.com/v5/ibm/deployments/${ENCODED_CRN}/backups" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     | jq -r '[.backups[] | select(.is_restorable == true)] | first | .id')
   echo "$BACKUP_CRN"
   ```
   {: pre}

5. **Provision the Gen 2 instance from the backup.**

   The `resource_plan_id` for Gen 2 is `databases-for-mongodb-standard-gen2`. Specify `restore_backup_id` directly in `parameters.dataservices`; do not nest it under the service key. Optional service-specific sizing parameters, including `version`, `host_flavor`, `members`, and `storage_gb`, are specified in `parameters.dataservices.mongodb`. If these parameters are not provided, the service broker applies default values.

   The service shortname matches the product: `mysql`, `postgresql`, `redis`, `elasticsearch`, or `mongodb`.
   {: note}

   ```bash
   PROVISION_RESPONSE=$(curl -s -X POST \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{
       \"name\": \"$GEN2_NAME\",
       \"target\": \"$GEN2_REGION\",
       \"resource_group\": \"$RESOURCE_GROUP_ID\",
       \"resource_plan_id\": \"databases-for-mongodb-standard-gen2\",
       \"parameters\": {
         \"dataservices\": {
           \"restore_backup_id\": \"$BACKUP_CRN\"
         }
       }
     }")
   echo "$PROVISION_RESPONSE" | jq '{name: .name, guid: .guid, state: .state}'
   ```
   {: pre}

   Store the returned GUID for polling:

   ```bash
   GEN2_GUID=$(echo "$PROVISION_RESPONSE" | jq -r '.guid')
   ```
   {: pre}

6. **Wait until the instance is active.**

   Restore can take several hours depending on data volume. Poll the status until `state` shows `active`:

   ```bash
   curl -s -X GET \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances/$GEN2_GUID" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '{state: .state, last_operation: .last_operation}'
   ```
   {: pre}

   Do not delete the Gen 1 instance while the restore is running. Deleting the source also deletes its backups.
   {: important}

#### Phase 2: Configure access, cut over, and finish
{: #api-br-phase-2}
{: api}

1. **Configure a VPE gateway for private network access.**

   Gen 2 supports private endpoints only. Before switching applications, create a Virtual Private Endpoint (VPE) gateway from the [{{site.data.keyword.cloud_notm}} VPC networking console](https://cloud.ibm.com/infrastructure/provision/endpointGateway){: external} to connect to the new Gen 2 instance.

2. **Create service credentials.**

   Retrieve the Gen 2 instance CRN from the Resource Controller, then create a service credential with the `Manager` role:

   ```bash
   GEN2_CRN=$(curl -s -X GET \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances/$GEN2_GUID" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq -r '.crn')

   curl -s -X POST \
     "https://resource-controller.cloud.ibm.com/v2/resource_keys" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{
       \"name\": \"${GEN2_NAME}-credentials\",
       \"source\": \"$GEN2_CRN\",
       \"parameters\": {
         \"role\": \"Manager\"
       }
     }" | jq '.credentials'
   ```
   {: pre}

3. **Update application endpoints.**

   Update your application to use the Gen 2 private endpoint from the credentials output instead of the Gen 1 endpoint.

   Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.
   {: note}

4. **Redeploy or restart applications.**

   Restart your applications with the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

5. **Finish and decommission Gen 1.**

   After validating that your applications are running correctly on Gen 2, proceed to [Post-migration tasks](#post-migration). Keep the Gen 1 deployment available through your planned retention period before decommissioning it.

## Migrating by using Terraform
{: #migrating-using-terraform}
{: terraform}

You can migrate your {{site.data.keyword.databases-for-mongodb}} instances from Gen 1 (Classic) to Gen 2 (VPC) by using Terraform and the {{site.data.keyword.cloud_notm}} provider (`ibm_database` resource).

MongoDB Standard supports only the **Backup and restore** (planned downtime) path. Read replicas are not available on MongoDB Standard.

Before you begin:

- [Install the Terraform CLI and the IBM Cloud Provider plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#tf_installation){: external}.
- Ensure that you have an [IBM Cloud API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external}.

Gen 2 is initially available in the `us-east` (Washington, DC) region, with additional regions to follow. Ensure that the location parameter in your configurations specifies a supported Gen 2 region, such as `us-east`.
{: note}

### Backup and restore migration with Terraform
{: #tf-backup-restore-migration}
{: terraform}

The backup and restore path restores a Gen 1 backup onto a newly provisioned Gen 2 instance.

#### Phase 1: Capture backup and restore to Gen 2
{: #tf-br-phase-1}
{: terraform}

1. **Verify the MongoDB version on Gen 1.**

   Confirm that the source deployment is on v7.0 or v8.0. Both are supported. A v7.0 source is restored directly to v8.0 on Gen 2. For more information, see [Version requirement](#version-requirement).

2. **Stop writes and capture an on-demand backup.**

   Stop application writes, then trigger an on-demand backup of the Gen 1 instance:

   ```bash
   ibmcloud cdb deployment-backup-now <gen1-instance-name-or-crn>
   ```
   {: pre}

   Retrieve the backup CRN:

   ```bash
   ibmcloud cdb deployment-backups <gen1-instance-name-or-crn>
   ```
   {: pre}

3. **Provision Gen 2 instance from backup.**

   Specify `backup_id` with the **full backup CRN** in your Gen 2 resource definition. Set `version` to `"8.0"` regardless of the Gen 1 source version.

   The `backup_id` parameter requires the full CRN format of the backup:

   `crn:v1:bluemix:public:databases-for-mongodb:<region>:a/<account_id>:<instance_id>:backup:<backup_id>`

   Example:

   `crn:v1:bluemix:public:databases-for-mongodb:us-east:a/df5f422898b540846cefeb9c5ad0aa66:623ebc19-ad61-47f2-84c5-b5d5b8c7a15a:backup:8d29c132-f351-4067-9b7b-0f307fe0ef84`

   Passing only the backup UUID will cause provisioning to fail.
   {: important}

   ```hcl
   resource "ibm_database" "gen2_from_backup" {
     name              = "mongodb-gen2-restored"
     service           = "databases-for-mongodb"
     plan              = "standard-gen2"
     location          = "us-east"
     resource_group_id = var.resource_group_id
     version           = "8.0"

     # Restore from Gen 1 backup CRN (must be the full CRN, not just the UUID)
     backup_id         = var.gen1_backup_crn

     service_endpoints = "private"

     group {
       group_id = "member"

       host_flavor {
         id = "bx3d.4x20"
       }

       disk {
         allocation_mb = 61440
       }
     }

     tags = ["env:production", "restored-from:gen1"]
   }
   ```
   {: codeblock}

   Apply the restore:

   ```bash
   terraform apply -target=ibm_database.gen2_from_backup
   ```
   {: pre}

#### Phase 2: Network access and application cutover
{: #tf-br-phase-2}
{: terraform}

1. **Configure private network access and credentials.**

   Follow the steps in [Configuring network access and credentials with Terraform](#tf-network-and-credentials) to configure the VPE gateway for `ibm_database.gen2_from_backup.crn` and generate new service credentials.

2. **Switch application endpoints.**

   Update application connection strings with the new private endpoints and Let's Encrypt certificates, then restart your applications. Confirm that they can read existing data and write new data on Gen 2.

3. **Generate credentials and switch applications.**

   Create credentials for the Gen 2 instance and update application connection endpoints:

   ```hcl
   resource "ibm_resource_key" "gen2_service_credentials" {
     name                 = "mongodb-gen2-app-credentials"
     resource_instance_id = ibm_database.gen2_from_backup.id
     parameters           = { "role" = "Manager" }
   }

   output "gen2_connection_strings" {
     value     = ibm_resource_key.gen2_service_credentials.credentials
     sensitive = true
   }
   ```
   {: codeblock}

   Restart application workloads and validate database connectivity.

### Configuring network access and credentials with Terraform
{: #tf-network-and-credentials}
{: terraform}

Because Gen 2 deployments only support private service endpoints and use a different credential model than Gen 1, all Gen 2 deployments require a Virtual Private Endpoint (VPE) gateway and new service credentials regardless of the migration path used.

#### 1. Configure VPE gateway for private network access
{: #tf-vpe-setup}
{: terraform}

Create a Virtual Private Endpoint (VPE) gateway in your VPC for your Gen 2 database instance CRN:

```hcl
resource "ibm_is_virtual_endpoint_gateway" "mongodb_vpe" {
  name           = "vpe-mongodb-gen2"
  target {
    crn           = ibm_database.gen2_from_backup.crn
    resource_type = "provider_cloud_service"
  }
  vpc            = var.vpc_id
  resource_group = var.resource_group_id
}
```
{: codeblock}

Apply the VPE gateway:

```bash
terraform apply -target=ibm_is_virtual_endpoint_gateway.mongodb_vpe
```
{: pre}

#### 2. Generate Gen 2 service credentials
{: #tf-credentials-setup}
{: terraform}

Create service credentials for the Gen 2 instance and export the connection strings:

```hcl
resource "ibm_resource_key" "gen2_service_credentials" {
  name                 = "mongodb-gen2-app-credentials"
  resource_instance_id = ibm_database.gen2_from_backup.id
}

output "gen2_connection_strings" {
  value     = ibm_resource_key.gen2_service_credentials.credentials
  sensitive = true
}
```
{: codeblock}

Apply the credentials:

```bash
terraform apply -target=ibm_resource_key.gen2_service_credentials
```
{: pre}

## Post-migration tasks
{: #post-migration}

After migration is complete:

* Validate application behavior and key business workflows on the Gen 2 deployment.
* Review performance, logs, and monitoring dashboards.
* Confirm that backup policies and alerting are configured on the Gen 2 deployment.
* Update runbooks, inventory records, and internal documentation to reflect the new Gen 2 endpoints and networking model.
* Delete migration-related service credentials that are no longer needed.
* Keep the Gen 1 deployment available through your planned retention period, as required by your rollback policy.
* Decommission the Gen 1 deployment only after validation and retention requirements are met.
