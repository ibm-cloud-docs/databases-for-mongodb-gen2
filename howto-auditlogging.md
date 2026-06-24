---

copyright:
  years: 2026, 2026
lastupdated: "2026-06-24"

keywords: mongodb, databases, logs, audit, enterprise

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise Sharding audit logging
{: #auditlogging}

Audit logging capabilities for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Sharding Edition are provided with {{site.data.keyword.logs_full}}.

## Event types
{: #event-types}

{{site.data.keyword.databases-for-mongodb}} Enterprise Sharding Edition provides audit logging capabilities based on the following event types: 

* `authenticate`
* `createCollection`
* `createDatabase`
* `createIndex`
* `renameCollection`
* `dropCollection`
* `dropDatabase`
* `dropIndex`
* `createUser`
* `dropUser`
* `dropAllUsersFromDatabase`
* `updateUser`
* `grantRolesToUser`
* `revokeRolesFromUser`
* `createRole`
* `updateRole`
* `dropRole`
* `dropAllRolesFromDatabase`
* `grantRolesToRole`
* `revokeRolesFromRole`
* `grantPrivilegesToRole`
* `revokePrivilegesFromRole`

The event types for {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding Edition are fixed and not configurable. 
{: .note}

## Audit logs
{: #audit-logs}

Audit events appear in {{site.data.keyword.logs_full}}.

For more information on {{site.data.keyword.databases-for-mongodb}} Enterprise Sharding Edition audit logging, see the [Logging for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-logging) and the [Getting started with {{site.data.keyword.logs_full}} tutorial](/docs/cloud-logs?topic=cloud-logs-getting-started){: external}.
