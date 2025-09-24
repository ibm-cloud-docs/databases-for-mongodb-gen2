---

copyright:
   years: 2025
lastupdated: "2025-09-24"
keywords: mongodb, timeseries
subcollection: databases-for-mongodb
content-type: tutorial
account-plan: paid
completion-time: 60min

---

{{site.data.keyword.attribute-definition-list}}	

# Explore time series collections in {{site.data.keyword.databases-for-mongodb}} 
{: #mongodb-timeseries}
{: toc-content-type="tutorial"}
{: toc-completion-time="30min"}

Time series data is a sequence of data points in which insights are gained by analyzing changes over time. Typical use cases are things, such as monitoring or sensor devices that report data (temperature or pressure) at regular intervals. 

MongoDB offers the abiity to define a collection as a time series collection, which allows the data to be stored more efficiently and retrieved faster.

If your use case involves time series data, make use of time series collections to benefit from faster query performance and efficient storage.

In this tutorial you will create a {{site.data.keyword.databases-for-mongodb}} instance, load the same pre-generated data into two different collections, one being a time series collection. You will then run the same query against both collections so that you can see the difference it makes to query execution to have a time series collection. 

{{site.data.keyword.databases-for-mongodb}} is a paid-for service, so following this tutorial will incur charges.
{: note}

## Before you begin
{: #mongodb-timeseries-prerequs}

Before you begin, ensure you have the following:

- An [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} - to deploy infrastructure
- [jq](https://jqlang.github.io/jq/){: external} - to parse configuration data
- [Mongo Database Tools](https://www.mongodb.com/try/download/database-tools){: external} - to upload data to your database using `mongorestore`.
- [MongoDB Shell](https://www.mongodb.com/try/download/shell){: external} - to connect to your database instance and run queries

## Obtain an API key to deploy infrastructure to your account
{: #mongodb-timeseries-obtain-key}
{: step}

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Clone the project
{: #mongodb-timeseries-clone-project}
{: step}

Get a local copy of the code you will need by cloning the public Github repository. Use the following command:

```sh
git clone https://github.com/IBM/ibm-mongodb-timeseries-collections.git
```
{: pre}

## Deploy an instance of {{site.data.keyword.databases-for-mongodb}} to your account
{: #mongodb-timeseries-install-infra}
{: step}

In this step you will deploy an instance of {{site.data.keyword.databases-for-mongodb}} and obtain key information to connect to it.

1. Navigate into the terraform folder of the cloned project.

   ```sh
   cd ibm-mongodb-timeseries-collections/terraform
   ```
   {: pre}

1. In that folder, create a document that is named `terraform.tfvars`, with the following fields:

   ```sh
    ibmcloud_api_key = "<your_api_key_from_step_1>"
    region = "<the IBM region where you will deploy the MongoDB database>"
    admin_password = "<the password of your mongodb admin user>"
   ```
   {: pre}

   The `terraform.tfvars` document contains variables that you might want to keep secret.
   {: important}

1. Install the infrastructure with the following command:

   ```sh
   terraform init 
   terraform apply --auto-approve
   ```
   {: pre}

## Create access credentials
{: #mongodb-timeseries-credentials}
{: step}

In this step you will create some environment variables to access the MongoDB deployment. Use the following command (inside the `terraform` folder):

```
terraform output --json | jq -r .cert.value | base64 --decode > ../mongo.cert
export MONGO_URL=`terraform output --json | jq -r .url.value | sed 's/^.*@//'| sed 's/\/.*$//'` 
export MONGO_URL="replset/$MONGO_URL"
export MONGO_PASSWORD=`terraform output --json | jq -r .password.value`
```
{: pre}

## Upload data
{: #mongodb-timeseries-upload}
{: step}

You are now ready to upload data. Your project contains a zip file (`dump.zip`) with more than a million timestamped records. Each one of them has a reading from a sensor located in London: 

```json
{
   "timestamp":"2024-05-20T09:25:47.645Z",
   "metadata":{
      "location":"London",
      "sensor_id":"3406"
   },
   "temp":18.756,
   "pressure":105.0461,
   "speed":89.4478
}
```

This data is ready to be imported into MongoDB using a tool called [`mongorestore`](https://www.mongodb.com/docs/database-tools/mongorestore/). It uses data exported from another MongoDB instance which is already in {{site.data.keyword.databases-for-mongodb}}'s [BSON](https://www.mongodb.com/resources/languages/bson) format.

From the `terraform` directory of your project, use the following command:

```
cd ..
unzip dump.zip
mongorestore -u admin -p $MONGO_PASSWORD --ssl --sslCAFile mongo.cert --authenticationDatabase admin --host $MONGO_URL dump/
```
{: pre}

## Query your data
{: #mongodb-timeseries-query-data}
{: step}

You are now ready to see the difference that having a timeseries collection can make to the efficiency of your queries.

You can log into your {{site.data.keyword.databases-for-mongodb}} instance with the following command:

```
mongosh -u admin -p $MONGO_PASSWORD --tls --tlsCAFile mongo.cert --authenticationDatabase admin --host $MONGO_URL
```
{: pre}

You are now inside the Mongo shell. To use the uploaded data, type:

```
use timeseriesdb
```
{: pre}

Now you will run the same query on both the time series and non-time series collections. It is a simple query that retrieves the data from a single sensor between two dates. You will add an "explain" function that will show you how the query will be executed by the database. In the Mongo shell type:

```
db.nontsCollection.explain("executionStats").find({"timestamp":{ $gt: new Date("2024-10-23T00:00:00.000Z"), $lt: new Date("2024-10-23T01:10:00.000Z")   }, "metadata.sensor_id": { $eq: "3405"} })

db.tsCollection.explain("executionStats").find({"timestamp":{ $gt: new Date("2024-10-23T00:00:00.000Z"), $lt: new Date("2024-10-23T01:10:00.000Z")   }, "metadata.sensor_id": { $eq: "3405"} })
```
{: pre}

Now, compare the output of both executions. Interpreting most of it is beyond the scope of this tutorial, but if you look at the `docsExamined` parameter, you will see that in the case of the time series collection, the database only had to look at 22 documents to find enough matching documents to return the first page of results. Meanwhile, the same query in a non-time series collection had to examime all 1,253,999 documents in the collection (a full collection scan) to be able to retrieve the same result set.

Clearly, the more documents you have in a non-timeseries collection, the worse your query performance will get. 

A time series collection is automatically indexed by time, so queries on time ranges are very efficient. In addition, time series collections store their metadata values (such as the sensor id in this case) in a [columnar format](https://www.mongodb.com/docs/manual/core/timeseries/timeseries-bucketing/) {: external}, which makes queries on one of those items combined with time even more efficient.

## Conclusion and Next Steps
{: #mongodb-timeseries-conclusion}
{: step}

In this tutorial you used an {{site.data.keyword.databases-for-mongodb}} instance to store time series data, that is data where time is a critical factor. You learned that by storing such data in a time series collection you can run much faster queries because MongoDB stores this type of data more efficiently and can therefore retrieve it much more quickly. You don't even have to create indexes on these fields yourself because the database does it for you.

You can learn more about time series collections in the [MongoDB documentation](https://www.mongodb.com/docs/manual/core/timeseries-collections/)

## Tear dowm your infrastructure
{: #mongodb-timeseries-tear-down}
{: step}

Your {{site.data.keyword.databases-for-mongodb}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the command:

```sh
terraform destroy
```
{: pre}
