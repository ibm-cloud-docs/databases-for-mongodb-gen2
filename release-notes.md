---

copyright:
  years: 2019, 2025
lastupdated: "2025-09-24"

keywords: databases-for-mongodb release notes

subcollection: databases-for-mongodb

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #mongodb-relnotes}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-mongodb_full_notm}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 28 August 2025
{: #databases-for-mongodb-28aug2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} version 8 is preferred
:  {{site.data.keyword.databases-for-mongodb_full}} version 8 is now available. If you use a previous version of {{site.data.keyword.databases-for-mongodb_full}}, you can migrate to version 8, which is the next available version.

## 25 June 2025
{: #databases-for-mongodb-25jun2025}
{: release-note}

In-place major verion upgrade is available for Standard plan
:   In-place major verion upgrade is available for the *Standard plan*. This is a major improvement for IBM {{site.data.keyword.databases-for}} end-of-life experience. The in-place major verion upgrade option offers  the following benefits:

   * Database connection parameters are retained after upgrade.
   * It is faster than the [Backup & restore](https://cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=ui#upgrading-restoring-from-backup) method.
   * Application stays connected with the database during the upgrade.
   * You can choose the time of the upgrade.

For more information, see [In-place major version upgrade](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=ui#upgrading-in-place).

You can continue to use the [Backup & restore](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=ui#upgrading-restoring-from-backup) method for {{site.data.keyword.databases-for-mongodb}} Standard plan and Enterprise plan.

## 10 March 2025
{: #databases-for-mongodb-10mar2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} {{site.data.keyword.satelliteshort}} plan is deprecated
:   {{site.data.keyword.satellitelong}} is now deprecated due to changes in market expectations, client fit, and lack of adoption. As of March 10 2025, all documentation relating to Satellite has been removed, as well as the ability to select {{site.data.keyword.databases-for-mongodb}} {{site.data.keyword.satelliteshort}} plan in the Cloud console.

## 28 February 2025
{: #databases-for-mongodb-28feb2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}}: Deprecation of BI Connector from {{site.data.keyword.databases-for-mongodb_full}} EE on March 31, 2025
:  After March 31, 2025, the {{site.data.keyword.databases-for-mongodb_full}} Enterprise (EE) feature [BI Connector](https://cloud.ibm.com/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics&interface=api#mongodbee-analytics-connector-bi) will no longer be available for deployment as an analytics add-on. All existing deployments of BI Connector analytics add-on will be disabled after this date.

## 01 February 2025
{: #databases-for-mongodb-01feb2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} version 7 is preferred
:  {{site.data.keyword.databases-for-mongodb_full}} version 7 is now available. Customers using previous version of {{site.data.keyword.databases-for-mongodb_full}} can migrate to version 7, which is the next available version.

## 31 January 2025
{: #databases-for-mongodb-31jan2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} end of life announcement: Version 6 reaches end of life on July 30, 2025
:  All {{site.data.keyword.databases-for-mongodb_full}} instances on deprecated versions that are still active will be upgraded in-place to the next major version. However, there are no SLAs for this forced migration.

We recommend that you upgrade by following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version. For more information, see [Upgrading to a new Major Version](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.


## 15 November 2024
{: #databases-for-mongodb-15nov2024}
{: release-note}

{{site.data.keyword.databases-for}} logs and events are now available on {{site.data.keyword.logs_full}}
: {{site.data.keyword.databases-for}} has onboarded {{site.data.keyword.logs_full_notm}}, a scalable logging service that persists logs and provides users with capabilities for querying, tailing, and visualizing logs. Customers are expected to use {{site.data.keyword.logs_full_notm}} to review their database logs and events starting **November 15, 2024**. For more information, see [Set up logging and monitoring](/docs/databases-for-mongodb?topic=databases-for-mongodb-getting-started-cdb-logging-monitoring) and [About IBM Cloud Logs](/docs/cloud-logs?topic=cloud-logs-about-cl).

## 16 September 2024
{: #databases-for-mongodb-16sept2024}
{: release-note}

Private endpoints as new default
:  To ensure best possible security for your databases, private endpoints are now the default in the {{site.data.keyword.cloud}} console. CLI and Terraform now require the endpoint type to be provided as part of creating an instance.

## 10 July 2024
{: #databases-for-mongodb-10july2024}
{: release-note}

Point In Time Recovery (PITR) speed enhancements for the Enterprise plan
:  Point in Time Recovery allows database administrators greater granularity when restoring a backup to a particular point in time, as a means to recover from data corruption or accidental deletion. Speed is key when recovering from a disaster. The latest release provides enhancements to the speed of restore when using PITR functionality. By enhancing the backend processes that deal with these restores, these are now 3-4 times faster than before. Actual restore times for individual database deployments are a function of the size of the data under management in the deployment. PITR is only available on the Enterprise plan.

## 1 May 2024
{: #databases-for-mongodb-01may2024}
{: release-note}

New hosting models
:  You can choose between two hosting models: Isolated Compute and Shared Compute. Isolated Compute is a secure single-tenant offering for complex, highly performant enterprise workloads. Shared Compute is a flexible multi-tenant offering for dynamic, fine-tuned, and decoupled capacity selections. For more information, see [Hosting models](/docs/cloud-databases?topic=cloud-databases-hosting-models){: external}.

## 18 January 2024
{: #databases-for-mongodb-18jan2024}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} End of life announcement: Version 5 reaches end of life on September 25, 2024
:  All {{site.data.keyword.databases-for-mongodb_full}} instances on deprecated versions that are still active will be upgraded in-place to the next major version. We recommend that you upgrade following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version. For more information, see [Upgrading to a new Major Version](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.

## 15 December 2023
{: #databases-for-mongodb-15dec2023}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} version 6 released
:  [{{site.data.keyword.databases-for-mongodb}} version 6](https://www.mongodb.com/blog/post/mongodb-6-0-now-available){: external} is now available. For more information, see [Upgrading to a new Major Version](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.

## 22 November 2023
{: #databases-for-mongodb-22nov2023}
{: release-note}

Monitoring Integration documentation updated
:  Monitoring Integration documentation now lists metrics for all {{site.data.keyword.databases-for}} services. For more information, see [Monitoring Integration](/docs/cloud-databases?topic=cloud-databases-monitoring){: external}.

## 13 November 2023
{: #databases-for-mongodb-13nov2023}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Plans documentation
:  {{site.data.keyword.databases-for-mongodb}} Plans documentation created. For more information, see [{{site.data.keyword.databases-for-mongodb}} Plans](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodb-plans){: external}.

## 12 October 2023
{: #databases-for-mongodb-12oct2023}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} end of life dates
:  End of life announcement: Version 4.2 reaches end of life July 2023. Version 4.4 reaches end of life 26 April 2024. Version 5 reaches end of life September 2024. All {{site.data.keyword.databases-for-mongodb}} instances on deprecated versions that are still active will be upgraded in-place to the next major version. We recommend that you upgrade following our [backup and restore process](/docs/cloud-databases?topic=cloud-databases-dashboard-backups){: external} before the EOL date of your version.

For more information, see [Upgrading to a new Major Version](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading){: external}.

## 30 May 2023
{: #databases-for-mongodb-30may2023}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} version 5 Preferred
:  {{site.data.keyword.databases-for-mongodb}} version 5 is the preferred version. For more information, see [Upgrading to a new Major Version](/docs/databases-for-mongodb?topic=databases-for-mongodb-upgrading&interface=ui).

## 22 May 2023
{: #databases-for-mongodb-18may2023}
{: release-note}

Setting up disk alerts for disk utilization tutorial
:  In this tutorial, you use the {{site.data.keyword.cloud_notm}} API and the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external} to set up an alert that emails you whenever the disk utilization of your database exceeds 90%. This specific example creates an alert on a {{site.data.keyword.databases-for-elasticsearch}} deployment, but it is applicable to all the databases in the IBM {{site.data.keyword.databases-for}} catalog. For more information, see [Setting up disk alerts for disk utilization](/docs/databases-for-mongodb?topic=databases-for-mongodb-disk-util-alert-tutorial).

## 12 December 2022
{: #databases-for-mongodb-12dec2022}
{: release-note}

Point-in-time Recovery
:  {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition offers PITR using any timestamp greater than the earliest available recovery point. To discover the earliest recovery point through the API, use the [point-in-time-recovery timestamp endpoint](https://cloud.ibm.com/apidocs/cloud-databases-api/cloud-databases-api-v5#capability). For more information, see [Point-in-time Recovery (PITR)](/docs/databases-for-mongodb?topic=databases-for-mongodb-pitr&interface=ui).

## 19 October 2022
{: #databases-for-mongodb-19oct2022}
{: release-note}

Deploying and Connecting a Cloud Databases Instance Tutorial
:  This tutorial guides you through the process of deploying a {{site.data.keyword.databases-for}} instance and connecting it to a web front end by creating a webpage that allows visitors to input a word and its definition. These values are then stored in a database running on {{site.data.keyword.databases-for}}. You install the database infrastructure by using Terraform and your web application uses the popular Express framework. The application can then be run locally, or by using Docker. For more information, see [Deploying and Connecting a {{site.data.keyword.databases-for}} Instance](/docs/cloud-databases?topic=cloud-databases-create-instance-tutorial).

## 11 October 2022
{: #databases-for-mongodb-11oct2022}
{: release-note}

Protecting {{site.data.keyword.databases-for-mongodb_full}} resources with context-based restrictions
:  Context-based restrictions (CBR) give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.databases-for}} resources can be controlled with CBR and identity and access management (IAM) policies. For more information, see [Protecting Cloud Databases resources with context-based restrictions](/docs/cloud-databases?topic=cloud-databases-cbr).

## 22 June 2022
{: #databases-for-mongodb-22june2022}
{: release-note}

Mapping Global COVID-19 cases with the {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On and Tableau tutorial published
:  The {{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On allows you to run long-running analytical queries or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/){: .external} to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/){: .external}. For more information, see [{{site.data.keyword.databases-for-mongodb}} EE (Enterprise Edition) Analytics Add-On](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics). This tutorial familiarizes you with the Analytics Add-On using Tableau to visualize data in your MongoDB instance.

## 31 May 2022
{: #databases-for-mongodb-31may2022}
{: release-note}

Provision an {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance with Terraform tutorial published
:  In this tutorial, you learn how to use Terraform to provision a Databases for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance that includes the {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On. For more information, see [Provision a Databases for {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition instance with Terraform](/docs/cloud-databases?topic=cloud-databases-tutorial-provision-mongodbee-tf).

## 26 April 2022
{: #databases-for-mongodb-26apr02022}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On Release
:  The {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition Analytics Add-On allows you to execute long-running analytical queries and/or provision a [MongoDB Connector for business intelligence(BI)](https://docs.mongodb.com/bi-connector/current/) to make your query data compatible with BI tools, such as [Tableau](https://www.tableau.com/). For more information, see the documentation [here](/docs/databases-for-mongodb?topic=databases-for-mongodb-mongodbee-analytics).

## 17 September 2021
{: #databases-for-mongodb-17sep02021}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} 4.0 End of Life in April, 2022
:  On April 27, 2022, all {{site.data.keyword.databases-for-mongodb_full}} instances on version 4.0 that are still active will be upgraded in-place to the next major version. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-mongodb-40-end-of-life-in-april-2022).

## 1 December 2020
{: #databases-for-mongodb-01dec2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} 3.6 End of Life in April, 2021
:  On April 27, 2021, all {{site.data.keyword.databases-for-mongodb_full}} instances on version 3.6 that are still active will be upgraded in-place to the next major version. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/databases-for-mongodb-36-end-of-life-in-april-2021).

## 31 August 2020
{: #databases-for-mongodb-31aug2020}
{: release-note}

General Availability of {{site.data.keyword.databases-for-mongodb_full}} Enterprise
:  Introducing IBM Cloud Databases for MongoDB Enterprise. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/powering-up-databases-for-mongodb).

## 26 May 2020
{: #databases-for-mongodb-26may2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}}'s Improved High Availability Architecture
:  At IBM Cloud Data Services, we are tirelessly investigating improvements to our service in order to offer our customers the best possible price-to-performance ratio balanced with data availability and safety. As part of that work, we are changing the default number of data members for {{site.data.keyword.databases-for-mongodb_full}} from two to three.

## 13 April 2020
{: #databases-for-mongodb-13apr2020}
{: release-note}

{{site.data.keyword.databases-for-mongodb_full}} autoscaling
:  We are excited to announce that autoscaling of your deployments based on disk capacity and disk I/O utilization is now available for {{site.data.keyword.databases-for-mongodb_full}} via the UI, API, and CLI. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-databases-portfolio-introduces-autoscaling).

## 6 August 2019
{: #databases-for-mongodb-06aug2019}
{: release-note}

New Regions Available for IBM Cloud Database Services
:  {{site.data.keyword.databases-for-mongodb_full}} is now available to be deployed in Seoul; South Korea; and Chennai, India. See blog post announcement [here](https://www.ibm.com/cloud/blog/announcements/new-regions-available-for-ibm-cloud-database-services).

## 27 February 2019
{: #databases-for-mongodb-27feb2019}
{: release-note}

General Availability of {{site.data.keyword.databases-for-mongodb_full}}
:  {{site.data.keyword.databases-for-mongodb_full}} added to the [IBM Cloud Databases](https://www.ibm.com/cloud/databases) family. See blog post announcement [here](https://www.ibm.com/blog/announcement/generally-available-mongodb-community-on-ibm-cloud-databases/).
