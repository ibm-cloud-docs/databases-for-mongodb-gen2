---
copyright:
  years: 2024, 2025
lastupdated: "2025-09-24"

keywords: mongodb, databases, mongo compass

subcollection: databases-for-mongodb

---

# Connect with MongoDB Compass
{: #connecting-mongodb-compass}

[MongoDB Compass](https://www.mongodb.com/docs/compass/current/){: external} is a powerful GUI for querying, aggregating, and analyzing your MongoDB data in a visual environment. Compass is free to use and source available, and can be run on macOS, Windows, and Linux.

## Install and connect to Compass
{: #install-connect-mongodb-compass}

Follow [the instructions](https://www.mongodb.com/try/download/compass){: external} to download and install Compass on your machine.

When you first open MongoDB Compass on the **New connection** page, enter your instance's connection information. All relevant connection information can be found within your instance's **Overview** page.

To connect to your deployment with MongoDB Compass, complete the following steps:

- In **New connection**, enter the **URI**. Copy it from the **Public connections** Endpoint, within your instance's Overview page under the **Endpoints** section.
- Click **>Advanced connection options**.
- In the *Authentication* tab, select *Username/Password*, and enter the credentials that you set for the admin user in your instance's **Settings**.
- Configure the **TLS/SSL** settings.
    1. In your instance's **Overview**, copy the certificate information from **TLS certificate**.
    1. In your instance's **Overview**, download the TLS certificate from the **Certificate authority** section.
    1. In MongoDB Compass, click **Select files** in the *Certificate Authority* field and upload the certificate file to MongoDB Compass.
- (Optional) Give your instance a name.
- Click **Connect** to connect MongoDB Compass to your instance.

## Use MongoDB Compass
{: #using-mongodb-compass}

After you connect to your deployment, you see a basic overview. Included is a simple summary of the cluster and the default databases. In the **Show Connection Info** section, it displays the three data-bearing members in your replica set, the number of databases and collections, and the current MongoDB version. {{site.data.keyword.databases-for-mongodb}} Standard uses the Community version while {{site.data.keyword.databases-for-mongodb}} Enterprise uses the Enterprise version.

Next, you see the default databases for your deployment, which all hold information related to the database instance. 

- `local` holds replication data.
- `config` holds data for cluster operations. 
- `admin` holds user authentication data. 

MongoDB Compass might not have access to all the data in these databases for permissions and security reasons.

Now you can use MongoDB Compass to view any data you and your applications have stored in your deployment. You can also use MongoDB Compass to create new databases, collections, and documents. For more information, see the [MongoDB Compass documentation](https://docs.mongodb.com/compass/current/){: external}.
