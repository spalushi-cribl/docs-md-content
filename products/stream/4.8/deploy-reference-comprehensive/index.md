# Comprehensive Reference Architecture


This reference architecture shows one Cribl Stream deployment possibility for sending data to a diverse mix of destinations, such as data centers, while minimizing data egress costs. The multiple Worker Groups might be organized functionally, or by geographic location.

![Comprehensive reference architecture](ref-arch-comprehensive.69dca53f30.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-comprehensive.svg) or [draw.io](/downloads/ref-arch-comprehensive.drawio) format. Download Cribl stencils [here](https://assets.ctfassets.net/xnqwd8kotbaj/2OnWVALZxgnpdWhiTV40Ey/c68757ee18aa7ef411e9ef12a5fd0db7/Cribl-Stencil-Pack-ALL.zip).
> 
> This architecture shows customer-managed (on-prem) Worker Groups and primary/standby Leaders. For a Cribl.Cloud-centric alternative, see [Comprehensive Hybrid Cloud Reference Architecture](deploy-reference-comprehensive-hybrid).
>
{.box .success}

## Division of Labor

In this architecture, you manage all components.

You also manage a remote Git repo. You manage commit, push, deploy, and [backup](upgrading#back) operations through the Cribl Leader.

The [Replay](collectors) Worker Group can be on-demand, and provisioned when heavy replays are planned.

## General Sizing Considerations

Size based on data throughput (in+out), and on number of incoming TCP connections.


## Destination Considerations

Adjust Destinations' **Advanced Settings** > **Max connections** setting (where available) to limit the load on receivers. E.g.: For a [Splunk Load Balanced ](destinations-splunk-lb)Destination, if you have 5 Worker Groups sending data to the same Splunk indexer cluster, with 25 indexers, set **Max connections** to between `5`-`10`. 

## Load Balancers

The load balancer in front of the Leader UI can be an Application Load Balancer or a Network Load Balancer.

The load balancer between the Cribl Workers and the Cribl Leader listening on port 4200 must be a Network Load Balancer [or equivalent](deploy-add-second-leader#loadbalancers). 

## Leader High Availability

Configuring Leader [high availability](deploy-add-second-leader) requires a standby server, a load balancer ([details here](deploy-add-second-leader#loadbalancers)) between users and the Cribl UI port, and a Network Load Balancer between the Cribl Leader and Workers.

Leader high availability is optional. Many customers opt instead for a single Leader, and a remote Git repo for scheduled backups. If the Leader becomes unavailable, you can use the Git repo to provision a new Leader, restoring configuration from a backup. 

However, a single Leader is reliable only when Workers are running Cribl Sources that fully leverage a shared-nothing architecture (such as Raw HTTP, Splunk HEC, Elasticsearch API, Amazon S3 Source, etc). Here, Workers can process data even if the Leader is unavailable. A Leader without failover will not reliably support [Collectors](collectors), which require a Leader's ongoing orchestration.

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.
