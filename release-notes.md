---

copyright:
  years: 2026
lastupdated: "2026-10-23"

keywords: databases-for-mongodb release notes

subcollection: databases-for-mongodb-gen2

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}

# Release notes
{: #mongodb-relnotes}

[Gen 2]{: tag-purple}

Use these release notes to learn about the latest updates to {{site.data.keyword.databases-for-mongodb_full_notm}} that are grouped by _date_ or _build number_.
{: shortdesc}

## 21 October 2026
{: #cloud-databases-for-mongodb-gen2-21oct2026}
{: release-note}

Single-node deployments (Beta) now available for {{site.data.keyword.databases-for-mongodb}} 
:  {{site.data.keyword.databases-for-mongodb}} now supports single-node deployments (Beta) for select databases in select regions. Single-node deployments provision a single database member instead of the default two or three-member high-availability configuration and are intended for development, testing, and proof-of-concept workloads. Single-node deployments can be converted to a high-availability deployment without requiring data migration. For more information, see [MongoDB Enterprise Sharding](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-enterprise-sharding-provisioning-single-node&interface=cli).

Customer-defined maintenance windows now available for {{site.data.keyword.databases-for-mongodb}}
:   You can now configure the days and start time for maintenance on your {{site.data.keyword.databases-for-mongodb}} deployment. Customer-defined maintenance windows allow you to schedule maintenance activities during times that align with your operational requirements while ensuring critical security, reliability, and platform updates are applied. For more information, see [Customer-defined maintenance windows](/docs/cloud-databases-gen2?topic=cloud-databases-gen2-maintenance-windows&interface=cli).

## 12 October 2026
{: #databases-for-mongodb-gen2-12oct2026}
{: release-note}

Context-based restrictions (CBR) support now available
:  You can now use context-based restrictions (CBR) to restrict access to {{site.data.keyword.databases-for-mongodb}} resources based on network context, such as IP addresses, VPCs, and {{site.data.keyword.cloud}} services. For more information, see [Managing access with context-based restrictions](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-cbr&interface=ui).

## 7 October 2026
{: #databases-for-mongodb-gen2-07oct2026}
{: release-note}

Enhanced Bring Your Own Key (BYOK) experience in the provisioning UI
 : The provisioning experience now includes an updated encryption configuration component for customer-managed encryption keys through {{site.data.keyword.keymanagementservicefull}}. This update provides a more consistent key management experience during deployment creation. Learn more about [{{site.data.keyword.keymanagementserviceshort}} integration](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-key-protect&interface=ui) or provision a new [{{site.data.keyword.databases-for-mongodb}} Gen 2 deployment](https://cloud.ibm.com/databases/databases-for-mongodb/create) with customer-managed encryption enabled.

## 30 Sep 2026
{: #databases-for-mongodb-gen2-30sep2026}
{: release-note}

The {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding plans are now available in all VPC multizone regions
: You can now deploy {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding Gen 2 plans in all supported {{site.data.keyword.cloud}} VPC multizone regions (MZRs). This release adds support for Toronto (ca-tor), Tokyo (jp-tok), Osaka (jp-osa), and Sao Paulo (br-sao). For more information, see [Overview of Gen 1 and Gen 2](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2#feature-differentiators).

## 17 Sep 2026
{: #databases-for-mongodb-gen2-17sep2026}
{: release-note}


The {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding plans are now available in Dallas and London
: The {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding plans are now available in Dallas (us-south) and London (eu-gb). These regions are in addition to the existing regions of Montreal (ca-mon), Chennai - Airtel (in-che), Mumbai (in-mum), Frankfurt (eu-de), Washington (us-east), Madrid (eu-es), and Sydney (au-syd). For more information, see [Overview of Gen 1 and Gen 2](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2#feature-differentiators).


## 10 Sep 2026
{: #databases-for-mongodb-gen2-10sep2026}
{: release-note}


{{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding plan are now available in Washington, Madrid, and Sydney
: {{site.data.keyword.databases-for-mongodb}} Standard and Enterprise Sharding plans are now available in Washington (us-east), Madrid (eu-es), and Sydney (au-syd). For more information, see [Overview of Gen 1 and Gen 2](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2#feature-differentiators).

## 6 July 2026
{: #databases-for-mongodb-gen2-06jul2026}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Enterprise Sharding plan is now available in Montreal, Mumbai, Chennai, and Frankfurt
: The MongoDB Enterprise Sharding plan is now available in Montreal (ca-mon), Chennai (in-che), Mumbai (in-mum), and Frankfurt (eu-de). For more information, see [MongoDB Enterprise Sharding plan](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-enterprise-sharding&interface=ui).

## 01 June 2026
{: #databases-for-mongodb-gen2-01jun2026}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Gen 2 is now available in Mumbai
: {{site.data.keyword.databases-for-mongodb}} Gen 2 is now also available in Mumbai, in addition to Montreal (ca-mon) and Chennai-Airtel (in-che). For more information, see [Overview of Gen 1 and Gen 2](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2#feature-differentiators).

## 02 March 2026
{: #databases-for-mongodb-gen2-02mar2026}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Gen 2 is now available in Chennai
: {{site.data.keyword.databases-for-mongodb}} Gen 2 is now also available in Chennai - Airtel (in-che), in addition to Montreal (ca-mon). For more information, see [Overview of Gen 1 and Gen 2](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-overview-gen1-gen2#feature-differentiators).

## 26 February 2026
{: #databases-for-mongodb-gen2-26feb2026}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Gen 2 is now generally available (GA)
: {{site.data.keyword.databases-for-mongodb}} Gen 2 is now available, offering the same fully managed {{site.data.keyword.databases-for-mongodb}} engine on newer VPC‑based infrastructure with improved security, networking, and more performant snapshot‑based backups. [Try {{site.data.keyword.databases-for-mongodb}} Gen 2 now](/docs/databases-for-mongodb-gen2?topic=databases-for-mongodb-gen2-provisioning).

## 15 December 2025
{: #databases-for-mongodb-gen2-15dec2025}
{: release-note}

{{site.data.keyword.databases-for-mongodb}} Gen 2 is now available in beta
: Support for Mongo v8: Built on VPC architecture with software-defined networking, delivering enhanced security and flexibility.
: Now available in the Montreal region, giving developers early access to next-generation database innovation.
