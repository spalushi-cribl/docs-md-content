# Transfer Data Between Workspaces or Environments


Cribl Stream provides you with the ability to transfer data between Cribl
Worker Nodes that report to the **same** Leader or different Leaders, via the [Cribl
TCP](sources-cribl-tcp) and [Cribl HTTP](sources-cribl-http) Source and
Destination pairs. 

You can enable data sharing across Worker Nodes that do not share a Leader as long as both
Organizations have [**active, paid Cribl.Cloud plans**](cloud-billing) or [**active, paid licenses**](licensing#license-quotas).

We designed this workflow to support scenarios where you can't or don't want to
create a direct connection between Leader Nodes, such as:

|Use case|Description|
|--------|-----------|
|**High/Low Security Environments**|Isolated and protected network environments that handle sensitive data (for example, air-gapped networks managed by Cribl Edge) sending data to an external Cribl Stream processing tier.|
|**Cribl Stream to Cribl Stream**|Send data between Cribl Stream environments or Workspaces that exist within the overall Cribl system that you own and pay for.|
|**Decentralized Management**|Cribl Edge environments managed by separate Leaders forwarding data to a central Cribl Stream instance.|

## Key Aspects of Data Transfer Between Environments

- The sending and receiving Organizations do not need to share a Leader.  
- The Cribl.Cloud Workspaces must be set up in the same Organization or the
  Leaders must have the exact same on-prem license key installed.  
- A single ingest charge means we only count data towards the ingest volume of
  the Cribl Organization that is **sending** the data.  
- A [shared key mechanism](#how-we-secure-the-data-transfer) validates and
  establishes trust between the environments.
- The transfer must utilize either the Cribl TCP or Cribl HTTP
  Source/Destination pair.

## Limitations

You **can't** transfer data across Organizations in these scenarios:

- On-prem Leader to Cribl.Cloud Leader.
- Workers on 4.11.1 and older communicating with Workers on 4.12.0 and newer.  
- Free tier Workers communicating between themselves within the same Workspace.  

### How We Secure the Data Transfer

A shared-key mechanism validates and establishes trust between Leaders. The
shared key will apply when the Leader of the sending environment has the exact
same license key installed as the receiving environment, or when any Cribl.Cloud
environment has Workspaces set up in the same Organization.

## Set up Sources and Destinations to Transfer Data

### Prerequisites / Before You Begin

Ensure the following criteria are met before configuring:

- Both of the environments for which you want to transfer data have an active
  billing contract or paid license. 
- You have Leader networking configured to allow cross-environment communication
  between Worker Nodes.
- You have a Source configured to collect data (events, logs, metrics, files,
  etc). This Source will send the data to the Cribl TCP or Cribl HTTP
  Destination on the receiving Organization.
- On Cribl-managed Worker Nodes in Cribl.Cloud, make sure that TLS is
  either disabled on both the Cribl HTTP Source and the Cribl HTTP Destination
  it's receiving data from, or enabled on both. Otherwise, no data will flow. In
  the Source, TLS is enabled by default. 

### Choose Your Source and Destination Pair

You can send data between Organizations using one of these Source and
Destination pairs:

- Cribl TCP Destination and Cribl TCP Source.
- Cribl HTTP Destination and Cribl HTTP Source.

For more information about these two options and how to decide which one is
right for your Organization, read [Interoperation Protocols](https://docs.cribl.io/reference-architectures/reference-arch-full-suite/#protocols).

### Configure Cross-Workspace Data Collection
 
>This guide will cover the Cribl TCP setup, but the general workflow is the same
>for the Cribl HTTP Source and Destination.
{.box .success}

This workflow assumes you already have data ingested in Cribl Stream, and you want
to move the data to a Worker Node in another Organization:

1. Configure the [Cribl TCP Source](sources-cribl-tcp) in the Organization where
    you want to **receive** data.
1. Configure the [Cribl TCP Destination](destinations-cribl-tcp) in the second
   Organization. This Destination will **send** data to the receiving Organization.

      - In the **Cribl Endpoint** field, enter the Leader hostname in the **Address**
        field, in one of these formats:   
 
          - Cribl.Cloud: `https://<your-workspace-name>.cribl.cloud`  
          - On-prem: `http://localhost:<port>`
 
1. If your setup meets the criteria (see [Key Aspects of Data
   Transfer](#key-aspects-of-data-transfer-between-environments) for a
   refresher), you will see data flowing into the Cribl TCP Source from Workers
   that are not connected to the same Leader. Neat!

## Troubleshooting
If you experience any issues when transferring data between Workspaces or Organizations,
make sure you can access both environments. Cribl Support will need to engage
all involved Organizations in any support case or call.

   
