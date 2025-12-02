# Cribl Lake 4.14.1


Cribl Lake 4.14.1 provides a new IAM Admin Permission specific to access-control management, clearer metrics about Dataset usage and contained objects, and bug fixes.

## New Features

This release adds the following new capabilities:

### IAM Admin Permission in Cribl.Cloud

The IAM Admin is a new Organization-level Permission that can manage the Organization's SSO configuration and can invite, update, and remove Members, without having access to data engineering functions. This reduces the risk of unauthorized data access or modification. 

### Expanded Workspace Landing Page

Each Workspace landing page is now a dashboard, providing:

- Buttons to quickly access all Cribl products.
- A bar graph showing hourly **Dataset Usage**.
- Health status and throughput of Stream Worker Groups and Edge Fleets.
- Recent actions in your Workspace, and Cribl resources.

![Workspace landing page](workspace-landing-pg.png)
{border="true" scale="80%"}

## Corrections

This release contains the following bug fixes:

ID | Description
-- | ----------- 
LAKE-1095 | In the [Direct Access (HTTP)](direct-access-http) Source, the `__hecToken` internal field has been renamed `__token`, to clarify its usage in monitoring tokens from all HTTP senders.
LAKE-1140 | Restored the missing Lakehouse [**Replication Latency**](lakehouse#monitor) monitoring graph.
LAKE-1165 | In [Cribl.Cloud Government](/fedramp/), the [Storage Locations](/lake/byos#settings) configuration page no longer displays a duplicate **Region** drop-down and no longer offers non-U.S. AWS Regions.
LAKE-1175 | The [Splunk Cloud Direct Access](splunk-cloud) Source no longer displays an incorrect banner saying that your AWS Regions for Cribl.Cloud and Splunk Cloud must match. You can place a DDSS bucket in any Region that Cribl.Cloud supports.
