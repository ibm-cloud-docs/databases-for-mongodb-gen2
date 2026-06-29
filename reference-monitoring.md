---
copyright:
  years: 2020, 2023
lastupdated: "2023-04-17"

keywords: mongodb, monitoring, metrics, iops, disk usage, memory usage, page faults, oplog size, oplog

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring Integration
{: #monitoring}

Monitoring for {{site.data.keyword.databases-for-mongodb_full}} deployments is provided through integration with the {{site.data.keyword.monitoringfull}} service. Your deployments forward selected information so you can monitor deployment health and resource usage. To see your {{site.data.keyword.databases-for-mongodb}} dashboards in {{site.data.keyword.monitoringfull_notm}}, you have to [Enable Platform Metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling) in the same region as your deployment. If you have deployments in more than one region, you have to provision {{site.data.keyword.monitoringfull_notm}} and enable platform metrics in each region.

To access {{site.data.keyword.monitoringfull_notm}} from your deployment, use the _Monitoring_ link from the right menu. (If you do not already have a monitoring service in the same region as your deployment it says _Add monitoring_.)

![The Monitoring link in a deployment](images/monitoring-ui-link.png){: caption="The Monitoring link in a deployment" caption-side="bottom"}

To access your deployment's monitoring dashboard from {{site.data.keyword.monitoringfull_notm}}, it's in the sidebar, under _IBM_.

![Cloud databases dashboard in monitoring](images/monitoring-ibm-list.png){: caption="Cloud databases dashboard in monitoring" caption-side="bottom"}

## Monitoring Availability
{: #monitoring-availability}

{{site.data.keyword.monitoringfull_notm}} is available for deployments in every region. Deployments in Multi-zone Regions (MZRs) - `eu-gb`, `eu-de`, `us-east`, `us-south`, `jp-tok`, `au-syd` - have their metrics in the corresponding region.

If you have deployments that are in Single-zone Region (SZR)`che01` then your logs are forwarded to an {{site.data.keyword.monitoringfull_notm}} instance in another region. You need to provision monitoring instances in the region where your metrics are forwarded to. Metrics for deployments in `che01` go to `jp-tok`. 

## Oplog Sizing
{: #oplog-sizing}

Oplog sizing is handled by {{site.data.keyword.databases-for}}. If you run a command to resize your Oplog in Compass, the resize will not persist. For more information, please [create a support ticket](https://www.ibm.com/cloud/support){: external}. 

## Available Metrics
{: #metrics-by-plan}

| Metric Name |
|-----------|
| [Average time spent acquiring locks in microseconds](#ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average) | 
| [Average time spent acquiring locks in microseconds](#ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average) | 
| [Connections](#ibm_databases_for_mongodb_connections) | 
| [Disk read latency mean](#ibm_databases_for_mongodb_disk_read_latency_mean) | 
| [Disk write latency mean](#ibm_databases_for_mongodb_disk_write_latency_mean) | 
| [IO utilization as a percent - 5 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m) |
| [IO utilization as a percent - 15 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m) | 
| [IO utilization as a percent - 30 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m) | 
| [IO utilization as a percent - 60 minute average](#ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m) | 
| [IOPS read & write total count for an instance](#ibm_databases_for_mongodb_disk_iops_read_write_total) | 
| [Max allowed memory for an instance](#ibm_databases_for_mongodb_memory_limit_bytes) | 
| [Oplog gigabyte per hour](#ibm_databases_for_mongodb_oplog_gb_per_hour) | 
| [Oplog used bytes](#ibm_databases_for_mongodb_oplog_used_bytes) | 
| [Oplog used bytes percent of total](#ibm_databases_for_mongodb_oplog_used_bytes_percent) | 
| [Oplog window hours](#ibm_databases_for_mongodb_oplog_window_hours) | 
| [Page faults](#ibm_databases_for_mongodb_page_faults) | 
| [Process resident memory in bytes](#ibm_databases_for_mongodb_process_resident_memory_bytes) |
| [Process virtual memory in bytes](#ibm_databases_for_mongodb_process_virtual_memory_bytes) |
| [Replica set member state](#ibm_databases_for_mongodb_status) |
| [Replication lag](#ibm_databases_for_mongodb_replica_lag) |
| [Status](#ibm_databases_for_mongodb_status) | 
| [Total disk space for an instance](#ibm_databases_for_mongodb_disk_total_bytes) | 
| [Used CPU for an instance](#ibm_databases_for_mongodb_cpu_used_percent) | 
| [Used disk space for an instance](#ibm_databases_for_mongodb_disk_used_bytes) | 
| [Used memory for an instance](#ibm_databases_for_mongodb_memory_used_bytes) | 
{: caption="Available Metrics" caption-side="top"}


### Average time spent acquiring locks in microseconds total_average
{: #ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average}

Average time spent acquiring locks in microseconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_locks_time_acquiring_microseconds_total_average`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 2: Average time spent acquiring locks in microseconds metric metadata" caption-side="top"}

### Average time spent acquiring locks in microseconds W_average
{: #ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average}

Average time spent acquiring locks in microseconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_locks_time_acquiring_microseconds_W_average`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 3: Average time spent acquiring locks in microseconds metric metadata" caption-side="top"}

### Connections
{: #ibm_databases_for_mongodb_connections}

The number of connections to the database.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_connections`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Connections metric metadata" caption-side="top"}

### Disk read latency mean
{: #ibm_databases_for_mongodb_disk_read_latency_mean}

Disk read latency mean

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_read_latency_mean`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 5: Disk read latency mean metric metadata" caption-side="top"}

### Disk write latency mean
{: #ibm_databases_for_mongodb_disk_write_latency_mean}

Disk write latency mean

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_write_latency_mean`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 6: Disk write latency mean metric metadata" caption-side="top"}

### IO utilization in percent 5 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m}

How much disk I/O has been used over 5 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_5m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="IO utilization in percent 5 minute average metric metadata" caption-side="top"}

### IO utilization in percent 15 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m}

How much disk I/O has been used over 15 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_15m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="IO utilization in percent 15 minute average metric metadata" caption-side="top"}

### IO utilization in percent 30 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m}

How much disk I/O has been used over 30 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_30m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="IO utilization in percent 30 minute average metric metadata" caption-side="top"}

### IO utilization in percent 60 minute average
{: #ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m}

How much disk I/O has been used over 60 minutes as a percentage of total disk I/O available.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_io_utilization_percent_average_60m`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="IO utilization in percent 60 minute average metric metadata" caption-side="top"}

### IOPS read & write total count for an instance
{: #ibm_databases_for_mongodb_disk_iops_read_write_total}

How many input/output operations per second your deployment is performing.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_iops_read_write_total`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="IOPS read & write total count for an instance metric metadata" caption-side="top"}

### Max allowed memory for an instance
{: #ibm_databases_for_mongodb_memory_limit_bytes}

The maximum amount of memory available to your deployment.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_memory_limit_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Max allowed memory for an instance metric metadata" caption-side="top"}

### Oplog gigabyte per hour
{: #ibm_databases_for_mongodb_oplog_gb_per_hour}

The gigabytes of oplog per hour the primary generates

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_gb_per_hour`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 13: Oplog gigabyte per hour metric metadata" caption-side="top"}

### Oplog used bytes
{: #ibm_databases_for_mongodb_oplog_used_bytes}

The total amount of space used by the oplog in bytes.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 14: Oplog used bytes metric metadata" caption-side="top"}

### Oplog used bytes percent of total
{: #ibm_databases_for_mongodb_oplog_used_bytes_percent}

The total used oplog space in percent

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_used_bytes_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Table 15: Oplog used bytes percent of total metric metadata" caption-side="top"}

### Oplog window hours
{: #ibm_databases_for_mongodb_oplog_window_hours}

The approximate number of hours available in the oplog.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_oplog_window_hours`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 16: Oplog window hours metric metadata" caption-side="top"}

### Page faults
{: #ibm_databases_for_mongodb_page_faults}

The number of times per second that MongoDB had to request data from disk. Scale RAM to reduce the number of disk requests.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_page_faults`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Page faults metric metadata" caption-side="top"}

### Process resident memory in bytes
{: #ibm_databases_for_mongodb_process_resident_memory_bytes}

Amount of actual physical memory used by the MongoDB process. Resident memory (the number of megabytes resident): In a standard deployment resident is the amount of memory used by the WiredTiger cache plus the memory dedicated to other in-memory structures used by the `mongod` process. By default, `mongod` with WiredTiger reserves 50% of the total physical memory on the server for the cache and at a steady state, WiredTiger tries to limit cache usage to 80% of that total. For example, if a server has 16GB of memory, WiredTiger will assume it can use 8GB for cache and at steady state should use about 6.5GB. To learn more about the WiredTiger Storage Engine, reference the [MongoDB documentation](/docs/databases-for-mongodb?topic=databases-for-mongodb-performance#wiredtiger-cache-and-memory).

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_process_resident_memory_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 18: Process resident memory in bytes metric metadata" caption-side="top"}

### Process virtual memory in bytes
{: #ibm_databases_for_mongodb_process_virtual_memory_bytes}

Amount of virtual memory used by the `mongod` process. Generally, virtual memory should be slightly larger than mapped memory. If virtual memory is many gigabytes larger, it indicates that excessive memory is being used by other aspects than the memory mapping of files and is sub-optimal. The most common case of usage of a high amount of memory for non-mapped is excessive connections to the database. Each connection has a thread stack and the memory for those stacks can add up considerably.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_process_virtual_memory_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 19: Process virtual memory in bytes metric metadata" caption-side="top"}

### Replica set member state
{: #ibm_databases_for_mongodb_status}

An integer between 0 and 10 that represents the replica state of the current member.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_status`|
| `Metric Type` | `gauge` |
| `Value Type`  | `count` |
| `Segment By` | `Service instance` |
{: caption="Table 20: Replica set member state metric metadata" caption-side="top"}

### Replication lag
{: #ibm_databases_for_mongodb_replica_lag}

The replication lag in seconds

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_replica_lag`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance` |
{: caption="Table 21: Replication lag metric metadata" caption-side="top"}

### Total disk space for an instance
{: #ibm_databases_for_mongodb_disk_total_bytes}

Represents the total amount of disk available to your deployment

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_total_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Total disk space for an instance metric metadata" caption-side="top"}

### Used CPU for an instance
{: #ibm_databases_for_mongodb_cpu_used_percent}

How much CPU is used as a percentage of total CPU available. Only for deployments that have dedicated CPU.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_cpu_used_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance` |
{: caption="Used CPU for an instance metric metadata" caption-side="top"}

### Used disk space for an instance
{: #ibm_databases_for_mongodb_disk_used_bytes}

How much disk your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_disk_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Used disk space for an instance metric metadata" caption-side="top"}

### Used memory for an instance
{: #ibm_databases_for_mongodb_memory_used_bytes}

How much memory your deployment is using.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_databases_for_mongodb_memory_used_bytes`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Used memory for an instance metric metadata" caption-side="top"}

## Attributes for Segmentation
{: #attributes}

### Global Attributes
{: #global-attributes}

The following attributes are available for segmenting all of the metrics listed above

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated, or local. |
| `Location` | `ibm_location` | The location of the monitored resource - this may be a region, data center, or global. |
| `Resource` | `ibm_resource` | The resource being measured by the service - typically a identifying name or GUID. |
| `Resource Type` | `ibm_resource_type` | The type of the resource being measured by the service. |
| `Scope` | `ibm_scope` | The scope is the account, organization, or space GUID associated with this metric. |
{: caption="Global Attributes Metadata" caption-side="top"}

### Additional Attributes
{: #additional-attributes}

The following attributes are available for segmenting one or more attributes as described in the reference above.  Please see the individual metrics for segmentation options.

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Service instance` | `ibm_service_instance` | The service instance segment identifies the instance the metric is associated with. |
| `Service instance name` | `ibm_service_instance_name` | The service instance name provides the user-provided name of the service instance, which isn't necessarily a unique value depending on the name provided by the user. |
| `Resource group` | `ibm_resource_group_name` | The resource group where the service instance was created. |
{: caption="Additional Attributes Metadata" caption-side="top"}

