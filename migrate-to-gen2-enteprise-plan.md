---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: mongodb enterprise migration, gen1 to gen2, classic infrastructure, vpc, continuous replication, mongosync, migration hub, ibmcloud cli, api, private endpoints, version upgrade, context-based restrictions

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Migrating {{site.data.keyword.databases-for-mongodb}} Enterprise Edition from Gen 1 to Gen 2
{: #migrating-mongodb-enterprise-gen1-to-gen2}

[Gen 2]{: tag-purple}

Use this tutorial to migrate your {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployment from Gen 1 (Classic) to Gen 2 (VPC) using the Migration hub in the {{site.data.keyword.cloud_notm}} console, the {{site.data.keyword.cloud_notm}} CLI, or the {{site.data.keyword.cloud_notm}} Resource Controller API.
{: shortdesc}

The Migration hub walks you through the migration in a guided wizard, split into two phases. Phase 1 prepares and provisions your Gen 2 target. Phase 2 handles continuous replication, cutover, and closes out the migration. The CLI and API follow the same two phases. See [Migrating by using the CLI](#cli-migration) and [Migrating by using the API](#api-migration).

The continuous replication path uses MongoSync, MongoDB's native tool for live data synchronization between clusters. For more information about MongoSync, see the [MongoSync documentation](https://www.mongodb.com/docs/cluster-to-cluster-sync/current/){: external}.

## Key considerations before you begin
{: #key-considerations}

Review the following requirements before you start the migration wizard.

### Version requirement
{: #version-requirement}

MongoDB 8.0 is the only version available on Gen 2. The migration path you follow depends on the version running on your Gen 1 source.

| Source version on Gen 1 | Migration path | Target version on Gen 2 |
| --- | --- | --- |
| 8.0 | Continuous replication via MongoSync (same version) | 8.0 |
| 7.0 | Continuous replication via MongoSync (cross-version) | 8.0 |
{: caption="Version readiness for Gen 1 to Gen 2 migration" caption-side="bottom"}

MongoSync supports cross-version replication from v7.0 to v8.0, so a separate in-place upgrade on Gen 1 is not required before migrating. If you prefer to upgrade to v8.0 on Gen 1 first, that option is also available before starting the Migration hub wizard.

For in-place upgrade instructions, see [Upgrading MongoDB](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.

### Private endpoints
{: #private-endpoints-requirement}

Gen 2 deployments use private endpoints only. Public endpoints are not available on Gen 2. Before you start the migration, confirm that your application infrastructure can reach private service endpoints. If your applications currently connect over public endpoints, plan the network changes before you begin.

For continuous replication, the Gen 1 source must have a private endpoint, in addition to Gen 2 being private-only. Replication cannot be set up unless the Gen 1 source has a private endpoint. Public endpoint support for Gen 1 will be rolled out in near future. For the steps to do migration using CLI or API, see [Continuous replication with the CLI](#cli-continuous-replication-migration) and [Continuous replication with the API](#api-continuous-replication-migration).
{: important}

### Context-based restrictions
{: #cbr-requirement}

Migration is not supported for Gen 1 deployments that have context-based restrictions (CBR) rules enabled. If CBR rules apply to your Gen 1 deployment, you can't use the Migration hub, the CLI, or the API to migrate it at this time.

## Planning considerations
{: #planning-considerations}

Keep these points in mind when planning your migration:

* Deployments on v8.0 or v7.0 can both use continuous replication via MongoSync. No Gen 1 upgrade is required before starting the wizard.
* Gen 2 supports private endpoints only. Plan your VPE gateway and any VPN or VSI connectivity before you start the wizard.
* Migration can be done only for Gen 1 instances that have a private endpoint. Support for Gen 1 instances with public endpoints is planned for a future release.
* Migration is not supported for Gen 1 instances that have CBR rules enabled.
* Downtime is limited to the final cutover event. The Gen 1 source stays operational throughout the continuous replication phase.
* Test the full procedure in a non-production environment before scheduling a production cutover.

## Before you begin
{: #prereqs}

Confirm the following before you start the wizard:

* You have a Gen 1 {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployment to migrate.
* You have access to provision a Gen 2 deployment in the target account, resource group, and region.
* A maintenance window is approved for the cutover event.
* You have access to the application configuration so that you can update connection details during cutover.

Run through these checks on the source deployment:

* Note the current MongoDB version (v7.0 or v8.0).
* Confirm there are no in-progress operations such as scaling, backups, or upgrades.
* Verify that a recent successful backup exists.
* Check disk utilization and available capacity.
* Identify whether applications connect over public endpoints, private endpoints, or both.
* Confirm that the Gen 1 deployment has a private endpoint. Migration can be done only for Gen 1 instances that have a private endpoint.
* Confirm that no CBR rules are enabled for the Gen 1 deployment. Migration is not supported for Gen 1 instances that have CBR rules enabled.

Run the migration against a development or low-traffic instance first to validate the procedure before scheduling a production cutover.
{: tip}

---

## Continuous replication migration
{: #continuous-replication-migration}
{: ui}

The continuous replication path migrates your data through live synchronization using MongoSync, keeping the Gen 1 source running until you are ready to cut over. Downtime is limited to the final cutover event.

* [Phase 1: Prepare and provision](#cr-phase-1)

   * [Step 1: Select migration path](#cr-select-path)
   * [Step 2: Assess Gen 1](#cr-assess-gen1)
   * [Step 3: Create Gen 2 cluster](#cr-provision-cluster)

* [Phase 2: Synchronize and cut over](#cr-phase-2)

   * [Step 4: Set network access](#cr-set-network-access)
   * [Step 5: Synchronization](#cr-synchronization)
   * [Step 6: Cutover](#cr-cutover)
   * [Step 7: Finish migration](#cr-finish-migration)

## Phase 1: Prepare and provision
{: #cr-phase-1}
{: ui}

Phase 1 covers the preparation and provisioning steps required before continuous replication can begin.

### Step 1: Select migration path
{: #cr-select-path}
{: step}
{: ui}

Open the Migration hub in the {{site.data.keyword.cloud_notm}} console. Under **Data services**, select **Eligible data instances** to see the Gen 1 Classic deployments in your account that are ready to migrate. Find your {{site.data.keyword.databases-for-mongodb}} Enterprise Edition instance and click **Open wizard**.

The wizard opens on the **Select migration path** screen. For MongoDB Enterprise Edition, the available paths are:

| Path | Downtime | When to use |
| --- | --- | --- |
| Continuous replication | Minimal | Recommended for production workloads. MongoSync replication runs continuously until you choose to cut over. Supports both v7.0 and v8.0 sources. |
| Provision only | None (data not moved) | Use for dev/test environments where data continuity is not required. This option opens the standard {{site.data.keyword.cloud_notm}} provisioning experience for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition on Gen 2 with no data migration step. |
{: caption="Available migration paths for MongoDB Enterprise Edition" caption-side="bottom"}

Select **Continuous replication** and click **Start**.

### Step 2: Assess Gen 1
{: #cr-assess-gen1}
{: step}
{: ui}

After you select the continuous replication path, the wizard runs an automated assessment of your Gen 1 source deployment to confirm it is ready to migrate. The assessment surfaces any required actions and items that need your manual attention before you can proceed.

The wizard surfaces the current MongoDB version and confirms MongoSync compatibility. Both v7.0 and v8.0 sources are supported.

The wizard also surfaces recommendations and contextual information about what changes between Gen 1 and Gen 2. Review this information before proceeding, as some settings behave differently on Gen 2.

Migration is not supported for Gen 1 deployments that have context-based restrictions (CBR) rules enabled. If the wizard detects CBR rules on the Gen 1 source, you can't continue with the migration.
{: important}

After you address any flagged items, use **Refresh** to update the assessment results. When the assessment is clear, click **Next: Create Gen 2** to continue.

### Step 3: Create Gen 2 cluster
{: #cr-provision-cluster}
{: step}
{: ui}

This step provisions the Gen 2 (VPC) cluster and establishes MongoSync continuous replication from your Gen 1 source. The Gen 1 source remains fully operational throughout. While replication is active, the Gen 2 instance is billed based on the configuration you choose.

The wizard shows a **Preview MongoDB Enterprise Edition on Gen 2** summary with the proposed configuration for the new deployment, covering details, resource allocations, service settings, and backup settings. Fields marked **Changed** highlight settings that differ from the Gen 1 source.

Click **Edit configurations** to adjust the Gen 2 settings before provisioning starts. The edit panel shows your Gen 1 resource allocation for reference and lets you configure:

* **Location and platform**: Target region. Platform is locked to Gen 2.
* **Service details**: Instance name, resource group, hosting model, and tags.
* **Resource allocation**: Host size (vCPU and RAM), disk, and member count.
* **Service configuration**: Database version (8.0) and encryption.

Size the Gen 2 deployment to handle production traffic after cutover.

Provision the Gen 2 cluster with **at least 2× the current Gen 1 disk allocation**. MongoSync performs a full initial copy of your data before continuous replication begins. The destination cluster needs enough storage to hold both the migrated dataset and the oplog entries written during that initial sync — for example, migrating 10 GB of data requires approximately 10 GB for the data itself plus another 10 GB for the initial sync oplog entries. Starting at 2× your current disk size provides this headroom and avoids running out of space mid-sync. CPU and RAM can match your Gen 1 allocation and be adjusted after cutover if needed.
{: note}

Once you confirm the configuration, the wizard starts provisioning and establishes the MongoSync connection. A progress bar tracks completion. You can open the Resource list to monitor the new instance while deployment runs. The **Next: Set network access** button becomes available once the Gen 2 cluster is running and initial synchronization has started.

## Phase 2: Synchronize and cut over
{: #cr-phase-2}
{: ui}

Phase 2 covers network access configuration, monitoring synchronization health, performing the cutover, and finishing the migration.

### Step 4: Set network access
{: #cr-set-network-access}
{: step}
{: ui}

Configure private connectivity to the Gen 2 (VPC) cluster before cutover. All connection types require a Virtual Private Endpoint (VPE) gateway for private access to the Gen 2 deployment.

Click **Open VPE gateway** to create the gateway from the VPC networking console. Use **Refresh** after setup to confirm the configuration is detected by the wizard.

The VPE gateway is required. The **Next: Synchronization** button is not available until this step is complete.
{: important}

### Step 5: Synchronization
{: #cr-synchronization}
{: step}
{: ui}

The Synchronization step shows the current status of the MongoSync continuous replication between your Gen 1 source and the Gen 2 cluster.

| Indicator | Description |
| --- | --- |
| Synchronization health status | **Active** means MongoSync is running and healthy. |
| Synchronization lag | The time delay between the Gen 1 source and the Gen 2 cluster. |
| Started | When continuous replication began. |
{: caption="Synchronization health indicators" caption-side="bottom"}

Do not proceed to cutover until synchronization health is **Active** and the lag is within an acceptable threshold for your workload.
{: important}

Use this time to confirm that the Gen 2 cluster is reachable through the VPE private endpoint, brief application owners on the connection details they will need to update, and document your rollback plan before initiating cutover.

Enable {{site.data.keyword.monitoringlong}} and {{site.data.keyword.logs_full}} on the Gen 2 cluster during this phase to have observability in place before cutover.
{: tip}

### Step 6: Cutover
{: #cr-cutover}
{: step}
{: ui}

The cutover step handles the final switchover. In the wizard, you promote the Gen 2 cluster, and then update your application endpoints to use it.

**Promote the Gen 2 cluster**

You do not need to stop writes to Gen 1 manually. When you promote the Gen 2 cluster, MongoSync automatically stops writes and puts the Gen 1 source in read-only mode while it completes the final synchronization.

Confirm that synchronization health is **Active** and lag is at or near zero before you promote the cluster.
{: important}

Promoting the Gen 2 cluster stops MongoSync replication. When you promote the cluster:

* MongoSync stops writes to the Gen 1 source and puts it in read-only mode.
* Continuous replication stops.
* The Gen 2 cluster becomes an independent instance.
* Backups are enabled on the new Gen 2 (VPC) primary cluster.

Promotion cannot be undone. Confirm that synchronization is complete and your rollback plan is in place before clicking **Promote Gen 2 cluster**.
{: important}

**Update application endpoints**

Update your application to use the Gen 2 (VPC) endpoint instead of the Gen 1 (Classic) endpoint. The wizard provides the full connection endpoint string, hostname, port, database name, and SSL mode for easy reference.

Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.
{: note}

**Update authentication credentials**

If your application already uses Gen 2 service credentials, no changes are needed. Otherwise, create a new service credential for the Gen 2 deployment and update your application configuration.

**Redeploy or restart applications**

Redeploy or restart your applications to apply the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

### Step 7: Finish migration
{: #cr-finish-migration}
{: step}
{: ui}

Once your applications are confirmed to be running on Gen 2, click **Next: Finish migration**.

The Finish migration screen shows a side-by-side summary of both instances, including instance status, active connections, last backup timestamp, and disk usage. Additional metrics for both instances are available through IBM Cloud Monitoring.

After finishing, the Gen 1 deployment stays available in a read-only state. Keep it running through your planned retention period before decommissioning, in line with your rollback policy.

Continue to [Post-migration tasks](#post-migration) to wrap up.

## Migrating by using the CLI
{: #cli-migration}
{: cli}

You can migrate your {{site.data.keyword.databases-for-mongodb}} Enterprise Edition instance from Gen 1 (Classic) to Gen 2 (VPC) by using the {{site.data.keyword.cloud_notm}} CLI.

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

### Continuous replication with the CLI
{: #cli-continuous-replication-migration}
{: cli}

The continuous replication path creates a Gen 2 instance that is synchronized with the Gen 1 source by using MongoSync. The Gen 1 source stays operational until you promote the Gen 2 cluster. A replication request carries the Gen 1 **instance** CRN. Do not pass `restore_backup_id`.

#### Phase 1: Prepare and provision Gen 2
{: #cli-cr-phase-1}
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

   The CRN is the Gen 1 instance CRN, which ends with `::`, for example `crn:v1:bluemix:public:databases-for-mongodb:<classic-region>:a/<account>:<classic-instance-guid>::`. It is not a backup CRN.

3. **Verify the MongoDB version on Gen 1.**

   Confirm that the source deployment is on v7.0 or v8.0:

   ```bash
   ibmcloud cdb deployment-about "$CLASSIC_CRN"
   ```
   {: pre}

   Both v7.0 and v8.0 are supported. MongoSync replicates a v7.0 source to v8.0 on Gen 2. For more information, see [Version requirement](#version-requirement).

4. **Verify that CBR rules are not enabled.**

   Migration is not supported for Gen 1 instances that have context-based restrictions (CBR) rules enabled. Confirm that no CBR rules apply to the Gen 1 source before you continue.
   {: important}

5. **Verify that the Gen 1 instance has a private endpoint enabled.**

   Replication cannot be set up unless the Gen 1 source instance has a private service endpoint enabled.
   {: important}

   In the {{site.data.keyword.cloud_notm}} console, open the Gen 1 deployment and verify that a private endpoint is enabled. Deployments that use only a public endpoint aren't currently supported for migration. Support for Gen 1 deployments with public endpoints is planned for a future release.

6. **Prepare the provisioning parameters file.**

   Create a `params.json` file with the following content. Replace `<crn>` with the Gen 1 instance CRN from step 2. Use `host_flavor` to size the Gen 2 cluster. Provision with at least twice the Gen 1 disk allocation. For more information, see [Step 3: Create Gen 2 cluster](#cr-provision-cluster).

   ```json
   {
     "dataservices": {
       "mongodbees": {
         "host_flavor": "bx3d.8x40.encrypted",
         "migrate": {
           "source_crn": "<crn>",
           "source_type": "gen1"
         }
       }
     }
   }
   ```
   {: codeblock}

   Alternatively, generate the value from the variable captured in the previous step:

   ```bash
   jq --arg crn "$CLASSIC_CRN" '.dataservices.mongodbees.migrate.source_crn = $crn' params.json.example > params.json
   ```
   {: pre}

   Use the Gen 1 **instance** CRN, not a backup CRN. Do not pass `restore_backup_id`.
   {: note}

7. **Create the Gen 2 instance.**

   ```bash
   ibmcloud resource service-instance-create "$GEN2_NAME" \
     databases-for-mongodb enterprise-sharding-gen2 "$GEN2_REGION" \
     -p @params.json
   ```
   {: pre}

8. **Wait until the instance is active.**

   Provisioning and the initial synchronization can take several hours depending on data volume. Poll the status until `State` shows `active`:

   ```bash
   ibmcloud resource service-instance "$GEN2_NAME"
   ```
   {: pre}

   Do not delete the Gen 1 instance while replication is in progress.
   {: important}

#### Phase 2: Configure access, cut over, and finish
{: #cli-cr-phase-2}
{: cli}

1. **Configure a VPE gateway for private network access.**

   Gen 2 supports private endpoints only. Create a Virtual Private Endpoint (VPE) gateway from the [{{site.data.keyword.cloud_notm}} VPC networking console](https://cloud.ibm.com/infrastructure/provision/endpointGateway){: external} to connect to the new Gen 2 instance.

2. **Verify replication health and lag.**

   Check the synchronization health and lag in the Migration hub. For more information, see [Step 5: Synchronization](#cr-synchronization).

   Do not continue until synchronization health is **Active** and the lag is within an acceptable threshold for your workload.
   {: important}

3. **Promote the Gen 2 cluster.**

   Promote the Gen 2 cluster in the Migration hub by clicking **Promote Gen 2 cluster**. For more information, see [Step 6: Cutover](#cr-cutover).

   You do not need to stop writes to Gen 1 manually. MongoSync automatically stops writes and puts the Gen 1 source in read-only mode during cutover.
   {: note}

   Promotion stops continuous replication and makes the Gen 2 cluster an independent instance. It cannot be undone.
   {: important}

4. **Create service credentials.**

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

5. **Update application endpoints.**

   Update your application to use the Gen 2 private endpoint from the credentials output instead of the Gen 1 endpoint.

   Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.
   {: note}

6. **Redeploy or restart applications.**

   Restart your applications with the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

7. **Finish and decommission Gen 1.**

   After validating that your applications are running correctly on Gen 2, proceed to [Post-migration tasks](#post-migration). Keep the Gen 1 deployment available through your planned retention period before decommissioning it:

   ```bash
   # ibmcloud resource service-instance-delete "$CLASSIC_NAME"   # only after cutover and retention period
   ```
   {: pre}

## Migrating by using the API
{: #api-migration}
{: api}

You can migrate your {{site.data.keyword.databases-for-mongodb}} Enterprise Edition instance from Gen 1 (Classic) to Gen 2 (VPC) by using the [{{site.data.keyword.cloud_notm}} Resource Controller API](https://cloud.ibm.com/apidocs/resource-controller/resource-controller){: external} and the [{{site.data.keyword.databases-for}} API](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-api){: external}.

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

### Continuous replication with the API
{: #api-continuous-replication-migration}
{: api}

The continuous replication path creates a Gen 2 instance that is synchronized with the Gen 1 source by using MongoSync. The Gen 1 source stays operational until you promote the Gen 2 cluster. A replication request carries the Gen 1 **instance** CRN in `source_crn`. A request that restores from a backup carries a backup CRN in `restore_backup_id`. Do not combine the two parameters.

#### Phase 1: Prepare and provision Gen 2
{: #api-cr-phase-1}
{: api}

1. **Get the resource group ID and the Gen 1 CRN.**

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

   The CRN is the Gen 1 instance CRN, which ends with `::`. It is not a backup CRN.

2. **Verify the database version on Gen 1.**

   Both v7.0 and v8.0 are supported. For more information, see [Version requirement](#version-requirement).

   Set `GEN2_REGION` to the target Gen 2 region (for example, `us-east`):

   ```bash
   GEN2_REGION=<target-gen2-region>
   ```
   {: pre}

   Next, encode the Gen 1 CRN and verify the version:

   ```bash
   ENCODED_CRN=$(jq -rn --arg v "$CLASSIC_CRN" '$v | @uri')
   curl -s -X GET \
     "https://api.${GEN2_REGION}.databases.cloud.ibm.com/v5/ibm/deployments/${ENCODED_CRN}" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '.deployment.version'
   ```
   {: pre}

3. **Verify that CBR rules are not enabled.**

   Migration is not supported for Gen 1 instances that have context-based restrictions (CBR) rules enabled. Confirm that no CBR rules apply to the Gen 1 source before you continue.
   {: important}

4. **Verify that the Gen 1 instance has a private endpoint enabled.**

   Replication cannot be set up unless the Gen 1 source instance has a private service endpoint enabled.
   {: important}

   In the {{site.data.keyword.cloud_notm}} console, open the Gen 1 deployment and verify that a private endpoint is enabled. Deployments that use only a public endpoint aren't currently supported for migration. Support for Gen 1 deployments with public endpoints is planned for a future release.

5. **Provision the Gen 2 instance with replication from Gen 1.**

   The `resource_plan_id` for {{site.data.keyword.databases-for-mongodb}} Enterprise on Gen 2 is `databases-for-mongodb-gen2-enterprise-sharding`. Specify the Gen 1 source in `parameters.dataservices.mongodbees.migrate`. The optional sizing parameter, `host_flavor`, is specified in `parameters.dataservices.mongodbees`. If these parameters are not provided, the service broker applies default values. Provision with at least twice the Gen 1 disk allocation. For more information, see [Step 3: Create Gen 2 cluster](#cr-provision-cluster).

   ```bash
   PROVISION_RESPONSE=$(curl -s -X POST \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances" \
     -H "Authorization: Bearer $IAM_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{
       \"name\": \"$GEN2_NAME\",
       \"target\": \"$GEN2_REGION\",
       \"resource_group\": \"$RESOURCE_GROUP_ID\",
       \"resource_plan_id\": \"databases-for-mongodb-gen2-enterprise-sharding\",
       \"parameters\": {
         \"dataservices\": {
           \"mongodbees\": {
             \"host_flavor\": \"bx3d.8x40.encrypted\",
             \"migrate\": {
               \"source_crn\": \"$CLASSIC_CRN\",
               \"source_type\": \"gen1\"
             }
           }
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

   Provisioning and the initial synchronization can take several hours depending on data volume. Poll the status until `state` shows `active`:

   ```bash
   curl -s -X GET \
     "https://resource-controller.cloud.ibm.com/v2/resource_instances/$GEN2_GUID" \
     -H "Authorization: Bearer $IAM_TOKEN" | jq '{state: .state, last_operation: .last_operation}'
   ```
   {: pre}

   Do not delete the Gen 1 instance while replication is in progress.
   {: important}

#### Phase 2: Configure access, cut over, and finish
{: #api-cr-phase-2}
{: api}

1. **Configure a VPE gateway for private network access.**

   Gen 2 supports private endpoints only. Create a Virtual Private Endpoint (VPE) gateway from the [{{site.data.keyword.cloud_notm}} VPC networking console](https://cloud.ibm.com/infrastructure/provision/endpointGateway){: external} to connect to the new Gen 2 instance.

2. **Verify replication health and lag.**

   Check the synchronization health and lag in the Migration hub. For more information, see [Step 5: Synchronization](#cr-synchronization).

   Do not continue until synchronization health is **Active** and the lag is within an acceptable threshold for your workload.
   {: important}

3. **Promote the Gen 2 cluster.**

   Promote the Gen 2 cluster in the Migration hub by clicking **Promote Gen 2 cluster**. For more information, see [Step 6: Cutover](#cr-cutover).

   You do not need to stop writes to Gen 1 manually. MongoSync automatically stops writes and puts the Gen 1 source in read-only mode during cutover.
   {: note}

   Promotion stops continuous replication and makes the Gen 2 cluster an independent instance. It cannot be undone.
   {: important}

4. **Create service credentials.**

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

5. **Update application endpoints.**

   Update your application to use the Gen 2 private endpoint from the credentials output instead of the Gen 1 endpoint.

   Gen 2 uses Let's Encrypt certificates rather than self-signed TLS certificates. Update any certificate validation or pinning in your application accordingly.
   {: note}

6. **Redeploy or restart applications.**

   Restart your applications with the updated endpoint and credentials. Confirm that they can read existing data and write new data on Gen 2 before proceeding.

7. **Finish and decommission Gen 1.**

   After validating that your applications are running correctly on Gen 2, proceed to [Post-migration tasks](#post-migration). Keep the Gen 1 deployment available through your planned retention period before decommissioning it.

---

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
