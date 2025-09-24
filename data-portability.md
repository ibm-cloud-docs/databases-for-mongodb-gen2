---

copyright:
  years: 2024
lastupdated: "2025-09-24"

keywords:

subcollection: databases-for-mongodb

---

{{site.data.keyword.attribute-definition-list}}


# Understanding data portability for {{site.data.keyword.databases-for-mongodb}}
{: #data-portability}


[Data portability](#x2113280){: term} involves a set of tools, and procedures that enable customers to export the digital artifacts that would be needed to implement similar workload and data processing on different service providers or on-prem software. It includes procedures for copying and storing the service customer's content, including the related configuration used by the service to store and process the data, on customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud}} services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.

The customer then is responsible for the use of the exported data and configuration for the purpose of data portability to other infrastructures.
This can involve:

- The planning and execution for setting up alternate infrastructure on on different cloud providers or on-prem software that provide similar capabilities to the IBM services.
- The planning and execution for the porting of the required application code on the alternate infrastructure, including the adaptation of customer's application code, deployment automation, and so on.
- The conversion of the exported data and configuration to format required by the alternate infrastructure and adapted applications.

For more information about your responsibilities when using {{site.data.keyword.databases-for-mongodb}}, see [Shared responsibilities for {{site.data.keyword.databases-for-mongodb}}](/docs/databases-for-mongodb?topic=databases-for-mongodb-responsibilities-cloud-databases).

## Data export procedures
{: #data-portability-procedures}

### Exporting data
{: #data-portability-exporting-data}

{{site.data.keyword.databases-for-mongodb}} provides mechanisms to export your content that has been uploaded, stored, and processed using the service.

To export your MongoDB data, use Mongo's [mongodump](https://www.mongodb.com/try/download/database-tools){: external} facility, which comes bundled with the command line tools in the download.

You can export entire databases or collections within databases by following the steps in the [MongoDB documentation](https://www.mongodb.com/docs/database-tools/mongodump/#mongodb-binary-bin.mongodump){: external}.

### Config file
{: #data-portability-config-file}

MongoDB stores its configuration in a file called `mongod.conf`. The {{site.data.keyword.databases-for-mongodb}} file has the following parameters when a database instance is provisioned:

```text
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: <data path>
  journal:
    enabled: true
#  engine:
#  mmapv1:
  wiredTiger:
    engineConfig:
      cacheSizeGB: <this is set as (memory-1)/2>


# network interfaces
net:
  port: <port>
  maxIncomingConnections: 65536
  bindIp:<MEMBER_IP>
  tls:
    mode: requireTLS
    certificateKeyFile: tls.pem
    CAFile: trusted_ca_bundle.crt
    # only enable TLS1_2
    disabledProtocols: TLS1_0,TLS1_1,TLS1_3
    allowConnectionsWithoutCertificates: true
    allowInvalidHostnames: false
security:
  clusterAuthMode: sendX509
  authorization: enabled
  keyFile: repl.key
  javascriptEnabled: true

#operationProfiling:

replication:
  replSetName: replset
  oplogSizeMB: 2048
  enableMajorityReadConcern: true


setParameter:
  opensslCipherConfig: ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384

```

## Exported data formats
{: #data-portability-data-formats}



The data exported will be in Mongo's [BSON format](https://www.mongodb.com/resources/languages/bson){: external}.

The exported data can be uploaded to any other MongoDB instance using the [mongorestore](https://www.mongodb.com/docs/database-tools/mongorestore/) facility, which also comes bundled in the above tools download.

## Data ownership
{: #data-ownership}

All exported data are classified as Customer content and therefore apply to them the full customer ownership and licensing rights, as stated in [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/terms/?id=Z126-6304_WS).
