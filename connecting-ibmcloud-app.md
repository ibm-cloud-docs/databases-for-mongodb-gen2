---

copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: mongodb, databases, kubernetes

subcollection: databases-for-mongodb-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Connecting an {{site.data.keyword.cloud_notm}} application
{: #mongodb-connecting-ibmcloud-app}

[Gen 2]{: tag-purple}

Applications running in {{site.data.keyword.cloud_notm}} can be bound to your {{site.data.keyword.databases-for-mongodb_full}} deployment.

## Connecting a Kubernetes Service application
{: #mongodb-connecting-kubernetes-app}

There are two steps to connecting a Cloud databases deployment to a Kubernetes Service application. First, your deployment needs a to be bound to your cluster and its connection strings stored in a secret. The second step is configuring your application to use the connection strings.

Before connecting your Kubernetes Service application to a deployment, make sure that the deployment and cluster are both in the same region and resource group.

### Binding your deployment
{: #mongodb-binding-deployment}

1. **Private endpoints**

    To use a private endpoint, create a service key for your database. Kubernetes uses it when binding to the database.

    ```sh
    ibmcloud resource service-key-create <YOUR-PRIVATE-KEY> --instance-name <INSTANCE_NAME_OR_CRN> --service-endpoint private
    ```
    The private service endpoint is selected with `--service-endpoint private`. After that, you bind the database to the Kubernetes cluster through the private endpoint with the `cluster service bind` command.

    ```sh
    ibmcloud ks cluster service bind <YOUR_CLUSTER_NAME> <RESOURCE_GROUP> <INSTANCE_NAME_OR_CRN> --key <YOUR-PRIVATE-KEY>
    ```

2. **Verify** - Verify that the Kubernetes secret was created in your cluster namespace. Running the following command, you get the API key for accessing the instance of your deployment in your account.

    ```sh
    kubectl get secrets --namespace=default
    ```
    More information on binding services is found in the [Kubernetes Service documentation](/docs/containers?topic=containers-service-binding#bind-services).

### Configuring in your Kubernetes app
{: #mongodb-configuring-kubernetes-app}

When you bind your application to Kubernetes Service, it creates an environment variable from the cluster's secrets. Your deployment's connection information lives in `BINDING` as a JSON object. Load and parse the JSON object into your application to retrieve the information your application's driver needs to make a connection to the database.

The [Getting connection strings](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-connection-strings) page contains a reference of the JSON fields.

For more information, see the [Kubernetes Service documentation](https://cloud.ibm.com/docs/containers?topic=containers-service-binding#reference_secret).
