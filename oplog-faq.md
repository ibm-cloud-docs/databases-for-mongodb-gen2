---
copyright:
  years: 2023, 2025
lastupdated: "2025-09-24"

subcollection: databases-for-mongodb, oplog, operations log, oplogurl

keywords: operation log

---

{{site.data.keyword.attribute-definition-list}}

# MongoDB operations log (oplog) FAQ
{: #mongodb-faq-oplog}
{: faq}
{: support}

The MongoDB oplog (operations log) is a special capped collection that records all operations that modify the data stored in the database. For more information, see [Replica Set Oplog](https://www.mongodb.com/docs/manual/core/replica-set-oplog/){: external}.
{: shortdesc}

## Creating a user with read-only access to oplog
{: #mongodb-faq-oplog}
{: faq}
{: support}

To access the MongoDB oplog through `oplogUrl`, create an `oplog` user using the {{site.data.keyword.databases-for-mongodb_full}} `admin` user. The `oplog` user can set `&readPreference=primary` so that reads always happen from the primary.

As the `admin` user, use a command like:

```sh
db.createUser({user: "oplogUser", pwd: "PASSWORD", roles: [{role: "read", db: "local"}]})
```
{: pre}

## Set up Mongodb for open source application use
{: #mongodb-faq-oplog-meteor}
{: faq}
{: support}

To set up {{site.data.keyword.databases-for-mongodb}} for use with open-source software, like [Meteor.js](https://www.meteor.com/){: external}, configure your MongoDB connection string.

1. Create a user that can read the oplog.
2. Configure your MongoDB server oplog URL environment variable. For more information, see Meteor's [MONGO_OPLOG_URL](https://docs.meteor.com/cli/environment-variables.html#mongo-oplog-url){: external}.
