---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: mongodb, connection limits, terminating connections, mongodb connection pooling, mongodb managing connections

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing connections
{: #managing-connections}

[Gen 2]{: tag-purple}

{{site.data.keyword.databases-for}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the  [Beta reference](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-icd-gen2-beta).
{: beta}

Connections to your {{site.data.keyword.databases-for-mongodb}} deployment use resources, so it is important to consider how many connections you need when tuning your deployment's performance. Use the following command in the MongoDB shell or a MongoDB client to show the maximum number of allowed connections and the current usage.

```js
  db.serverStatus().connections
```

Sample output:

```json
{
  current: 33,
  available: 65503,
  totalCreated: 21858,
  rejected: 0,
  active: 9,
  threaded: 33,
  exhaustIsMaster: 0,
  exhaustHello: 6,
  awaitingTopologyChanges: 6
}
```

| Field                               | Meaning                                                                                                                |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **`current`**: `33`                 | Total current client connections, including idle ones. This is how many clients are connected right now.               |
| **`available`**: `65503`            | The number of additional client connections MongoDB can accept before hitting the limit (suggesting a max of \~65536). |
| **`totalCreated`**: `21858`         | Total number of connections created since the server started. Helps you understand connection churn.                   |
| **`rejected`**: `0`                 | Number of connection attempts rejected due to limit being reached.                              |
| **`threaded`**: `78`                | Number of threads currently handling connections. Often matches `current`. 
| **`active`**: `23`                  | Number of connections currently running operations — the rest (78 - 23 = 55) are idle.                                  |
| **`exhaustIsMaster`**: `0`          | Related to older server discovery protocols (now replaced). Usually 0.                                                 |
| **`exhaustHello`**: `19`            | Persistent monitoring connections (e.g., from drivers using the **hello** command in monitoring).                      |
| **`awaitingTopologyChanges`**: `19` | Connections waiting for topology changes (e.g., for high availability monitoring in replica sets).                     |


Note:

* `maximum connection` = `current` + `available`
* No rejected connections means that the limit isn't hit.
* By default, MongoDB allows up to 65536 connections. 
* Exceeding the connection limit for your deployment can cause your database to be unreachable by your applications.

## Terminating MongoDB connections
{: #terminate-connections}

To terminate (kill) MongoDB connections, use the following approach, typically through the MongoDB shell:
Run the following command to list active operations, including connections.

```json
  db.currentOp(true).inprog
```
{: .codeblock}

Output:

```json
{
  "client" : "IP:PORT",
  "active" : true,
  "opid" : <opid>,
  ...
}
```
{: .codeblock}

Once you find the `opid` of the operation (for example, `opid`: `12345`), kill it by using the following command:

```json
    db.killOp(12345)
```

* Admin privileges are required to run `currentOp` and `killOp`.
{: note}

## MongoDB connection pooling
{: #connection-pooling}

A connection pool in MongoDB is a cache of reusable connections between your application and the MongoDB server. Rather than opening and closing a new connection for every request (which is expensive), the application maintains a pool of connections and reuses them for multiple operations. One way to prevent exceeding the connection limit and ensure that connections from your applications are being handled efficiently is through connection pooling. The driver manages the creation, reuse, and closing of connections based on demand and configuration.

Node.js (Mongoose or native MongoDB driver):

```json
mongoose.connect(uri, {
  maxPoolSize: 100,      // default is 100
  minPoolSize: 10,
  maxIdleTimeMS: 30000,
  serverSelectionTimeoutMS: 5000
});
```
{: .codeblock}

Python (PyMongo):

```python
client = MongoClient(uri, maxPoolSize=100, minPoolSize=10)
```
{: .codeblock}

Java:

```java
MongoClientSettings settings = MongoClientSettings.builder()
    .applyToConnectionPoolSettings(builder ->
        builder.maxSize(100).minSize(10).maxConnectionIdleTime(30, TimeUnit.SECONDS))
    .build();
```
{: .codeblock}

| Term                                         | Description                                                                      |
| -------------------------------------------- | -------------------------------------------------------------------------------- |
| **maxPoolSize** (or `maxConnectionsPerHost`) | The maximum number of connections the pool can open to the MongoDB server.       |
| **minPoolSize**                              | The minimum number of connections maintained in the pool.                        |
| **waitQueueTimeoutMS**                       | How long the application waits for a connection from the pool when all are busy. |
| **maxIdleTimeMS**                            | How long a connection can remain idle before it is closed.                       |
