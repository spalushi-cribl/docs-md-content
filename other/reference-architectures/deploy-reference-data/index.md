# Unified Ingest & Replay Data

This reference architecture shows one possibility for ingesting and replaying diverse data types from multiple senders. The Sources shown at left are just representative examples. The replay workers at right could be hosted on Linux servers, in Kubernetes Pods, etc.

![Unified Ingest & Replay with Cribl Stream](ref-arch-all-in-1.7f15eb9ed1.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-all-in-1.svg) or [draw.io](/downloads/ref-arch-all-in-1.drawio) format. Download Cribl stencils [here](https://cribl.io/wp-content/uploads/2022/07/Cribl-Stencil-Pack-ALL.zip).
>
{.box .success}

## Sizing Considerations

- An all-in one Worker Group works best with fewer than 2000 Sources (agents, syslog senders, and so forth).

- Syslog over TCP should be under 200 GB/day from any one sender.
- Account for up to 50% bursts in traffic.

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
