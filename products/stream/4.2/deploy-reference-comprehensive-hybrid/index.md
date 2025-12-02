# Comprehensive Hybrid Cloud Reference Architecture


This reference architecture shows one Cribl Stream deployment possibility for sending data to a mix of analytics tools and data lakes. Data is processed by a mix of Cribl-managed/PaaS (platform as a service) and [hybrid](deploy-cloud#hybrid) Worker Groups, all coordinated by a Cloud Leader with failover. The multiple Worker Groups are organized functionally, by Destination type. One Cribl-managed Group is reserved for on-demand [replay](collectors) from data lakes.

![Comprehensive hybrid Cloud reference architecture](ref-arch-comprehensive-hybrid.dee3a3f3fc.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-comprehensive-hybrid.svg) or [draw.io](/downloads/ref-arch-comprehensive-hybrid.drawio) format. Download Cribl stencils [here](https://assets.ctfassets.net/xnqwd8kotbaj/2OnWVALZxgnpdWhiTV40Ey/c68757ee18aa7ef411e9ef12a5fd0db7/Cribl-Stencil-Pack-ALL.zip).
> 
> This architecture shows Cribl.Cloud Leaders with a mix of Cribl-managed and customer-managed (hybrid) Workers. For a fully customer-managed (on-prem) alternative, see [Comprehensive Reference Architecture](deploy-reference-comprehensive).
>
{.box .success}

## Division of Labor

[Cribl.Cloud](deploy-cloud) maintains high-availability Leader infrastructure on your behalf.

Cribl.Cloud also maintains secure copies of the Leader's configurations for all Worker Groups (Cribl-managed/PaaS and hybrid).

Even on Cribl-managed/PaaS Worker Groups, configuring and administering Pipelines, Routes, etc., remains your responsibility.

## General Sizing Considerations

Size based on data throughput (in+out), and on number of incoming TCP connections.

## Destination Considerations

On Cribl-managed/PaaS Groups, enable compression on all Destinations that support it, to minimize data egress volume and costs.

Adjust Destinations' **Advanced Settings** > **Max connections** setting (where available) to limit the load on receivers. E.g.: For a [Splunk Load Balanced ](destinations-splunk-lb)Destination, if you have 5 Worker Groups sending data to the same Splunk indexer cluster, with 25 indexers, set **Max connections** to between `5`-`10`. 

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.
