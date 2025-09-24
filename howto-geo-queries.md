---

copyright:
   years: 2024
lastupdated: "2024-10-04"

keywords: mongodb, geospatial

subcollection: databases-for-mongodb

content-type: tutorial
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}

# Explore the geospatial search capabilities of {{site.data.keyword.databases-for-mongodb}}
{: #mongodb-geo-queries}
{: toc-content-type="tutorial"}
{: toc-completion-time="30min"}

MongoDB's geospatial capabilities allow you to efficiently execute queries where geographical location is the key consideration. Consider, for example, how much of your everyday life is about your location or that of others. Where is the nearest Uber taxi? Where is the nearest Chinese restaurant? Which of my friends is nearest right now? These are all questions that apps like Uber, Google Search, and Whatsapp are constantly answering for their customers.

This tutorial guides you through the steps of importing location-based data about taxis in London into a {{site.data.keyword.databases-for-mongodb}} database, and then querying that data in a variety of ways.

{{site.data.keyword.databases-for-mongodb}} is a paid-for service, so following this tutorial will incur charges.
{: note}

## Before you start
{: #mongodb-geo-queries-before-start}

Before you begin, ensure you have the following:

- An [{{site.data.keyword.cloud_notm}} Account](https://cloud.ibm.com/registration){: external}.
- [Terraform](https://www.terraform.io/){: external} to deploy infrastructure.

## Obtain an API key to deploy infrastructure to your account
{: #mongodb-geo-queries-obtain-key}
{: step}

Follow [these steps](/docs/account?topic=account-userapikey&interface=ui#create_user_key){: external} to create an {{site.data.keyword.cloud_notm}} API key that enables Terraform to provision infrastructure into your account. You can create up to 20 API keys.

For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
{: note}

## Clone the project
{: #mongodb-geo-queries-clone-project}
{: step}

Get a local copy of the code that you need by cloning the public Github repository. Use the following command:

   ```sh
   git clone https://github.com/IBM/ibm-mongodb-geospatial-queries
   ```
   {: pre}

## Deploy an instance of {{site.data.keyword.databases-for-mongodb}} to your account
{: #mongodb-geo-queries-install-infra}
{: step}

In this step you deploy an instance of {{site.data.keyword.databases-for-mongodb}} and obtain key information to connect to it.

1. Navigate into the Terraform folder of the cloned project.

   ```sh
   cd ibm-mongodb-geospatial-queries/terraform
   ```
   {: pre}

1. In that folder, create a document that is named `terraform.tfvars` with the following fields:

   ```json
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

## Upload taxi data
{: #mongodb-geo-queries-upload-data}
{: step}

The previous step produces two elements that you need to gain access to the database:

- The deployment certificate (`cert`). You need this certificate to connect securely to the database. Decode the certificate, save it to a file in the root folder of the project, and export the file name as an environment variable. You can do this on your terminal like this:

   ```sh
   cd ..
   echo "<the_cert_from_the_output>" | base64 --decode > ca.cert
   export MONGO_CA_FILE="./ca.cert"
   ```
   {: pre}

- The `url` to access the deployment. Replace the `$PASSWORD` parameter in that URL with the admin password from your `terraform.tfvars` file. Then, export it as an environment variable from the terminal like this:

   ```sh
   export MONGO_URL="<the_url_from_the_output>"
   ```
   {: pre}

You are ready to upload data. The `import.js` script uses a Node utility called [datamaker](https://www.npmjs.com/package/datamaker){: external} to generate a large amount of random data in the format specified in the `taxi.json` file, and uploads that data into MongoDB. The format is a standard [GeoJSON](https://geojson.org/){: external} format, which is required for MongDB [geospatial queries](https://www.mongodb.com/docs/manual/geospatial-queries/){: external} to work.

The script also creates a geo index on the `geometry` field of the data, which is the one that contains the coordinates of each vehicle. In this case, you will create a [2dsphere](https://www.mongodb.com/docs/manual/geospatial-queries/#geospatial-indexes) index.

To run the script, use the following command:

   ```json
   npm install --save  #install all required node packages, including datamaker and mongodb
   node import.js
   ```
   {: pre}

## Query your data
{: #mongodb-geo-queries-query-data}
{: step}

There are two scripts that you can use to query your data:

1. `find_by_point.js`- With this script you feed it a lat/long pair and it returns the nearest taxi to that location, as in the following example:

    ```json
    node find_by_point.js --longitude="-0.13"  --latitude=50.3
    ```
    {: pre}

    You get a response like this:

    ```json
    {
      _id: new ObjectId('66bc7dd80fbd69a0f41c029b'),
      type: 'Feature',
      geometry: { type: 'Point', coordinates: [ -0.1002, 51.3611 ] },
      properties: {
        driver_name: 'Elisabeth Acuna',
        taxi_id: 'ECJK0AG87JB00991',
        vehicle_type: 'van',
        phone_number: '+352-1224-498-598',
        timestamp: '2024-08-14T09:50:16.884Z'
      }
    }
    ```
    {: pre}

   You can optionally add a `num_taxis` and a `vehicle_type` parameter to get more results, or filter by certain type of car (for example, sedan, van, sevenseater, minibus). See the following example.

    ```json
    node find_by_point.js --longitude="-0.13"  --latitude=50.3 --num_taxis=10 --vehicle_type=minibus
    ```
    {: pre}

   The script uses the [`$near` operator](https://www.mongodb.com/docs/manual/reference/operator/query/near/#mongodb-query-op.-near) of the geospatial capabilities of MongoDB.

2. `find_by_bounding_box.js` - With this script you feed it a bounding box and it returns up to five taxis within that, such as in the following example:

    ```json
    node find_by_bounding_box.js --topleftlat=51.5019647 --topleftlong=-0.1494702 --bottomrightlong=-0.1249548 --bottomrightlat=51.4881758
    ```
    {: pre}

   You can also add the `num_taxis` and `vehicle_type` parameters.

   The script uses the [`$geoWithin` operator](https://www.mongodb.com/docs/manual/reference/operator/query/geoWithin/#mongodb-query-op.-geoWithin) of the geospatial capabilities of MongoDB.

## Conclusion and next steps
{: #mongodb-geo-queries-conclusion}

In this tutorial you used an {{site.data.keyword.databases-for-mongodb}} instance to store data where location is an important factor. This data was stored as GeoJSON object, which allows the data to be indexed in MongoDB geospatial indexes. You can use these indexes to search over data for nearest results to a given point or within geographic boundaries.

You can explore more geospatial search features in the [MongoDB documentation](https://www.mongodb.com/docs/manual/geospatial-queries/#geospatial-queries){: external}.

## Tear down your infrastructure
{: #mongodb-geo-queries-tear-down}

Your {{site.data.keyword.databases-for-mongodb}} incurs charges. After you finish this tutorial, you can remove all the infrastructure by going to the `terraform` directory of the project and using the following command:

   ```sh
   terraform destroy
   ```
   {: pre}
