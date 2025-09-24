---

copyright:
  years: 2020, 2025
lastupdated: "2025-09-24"

keywords: mongodb, databases, logs, audit, enterprise

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB Enterprise audit logging
{: #auditlogging}

Audit logging capabilities for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition are provided with {{site.data.keyword.logs_full}}.

## Event types
{: #event-types}

{{site.data.keyword.databases-for-mongodb}} Enterprise Edition provides audit logging capabilities based on the following event types: 

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

The event types for {{site.data.keyword.databases-for-mongodb}} Enterprise Edition are fixed and not configurable. 
{: .note}

## Audit logs
{: #audit-logs}

Audit events appear in {{site.data.keyword.logs_full}}.

For more information on {{site.data.keyword.databases-for-mongodb}} Enterprise Edition audit logging, see the [Logging for {{site.data.keyword.databases-for}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-logging) and the [Getting started with {{site.data.keyword.logs_full}} tutorial](/docs/cloud-logs?topic=cloud-logs-getting-started){: external}.

Audit events had been appearing in {{site.data.keyword.la_full}}, which is being deprecated. For older deployments you can still use [{{site.data.keyword.la_full}}](/docs/log-analysis) until March 2025. You may want to check that and make sure you are migrating to the new logging solution.
{: .note}
