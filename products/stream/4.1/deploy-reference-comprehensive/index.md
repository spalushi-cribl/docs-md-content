# Comprehensive Reference Architecture


This reference architecture shows one Cribl Stream deployment possibility for sending data to a diverse mix of destinations, such as data centers, while minimizing data egress costs. The multiple Worker Groups might be organized functionally, or by geographic location.

![Comprehensive reference architecture](ref-arch-comprehensive.4a21a93e62.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-comprehensive.svg) or [draw.io](/downloads/ref-arch-comprehensive.drawio) format. Download Cribl stencils [here](https://assets.ctfassets.net/xnqwd8kotbaj/2OnWVALZxgnpdWhiTV40Ey/c68757ee18aa7ef411e9ef12a5fd0db7/Cribl-Stencil-Pack-ALL.zip).
>
{.box .success}

## General Sizing Considerations

Size based on data throughput (in+out), and on number of incoming TCP connections.

## Destination Considerations

Adjust Destinations' **Advanced Settings** > **Max connections** setting (where available) to limit the load on receivers. E.g.: For a [Splunk Load Balanced ](destinations-splunk-lb)Destination, if you have 5 Worker Groups sending data to the same Splunk indexer cluster, with 25 indexers, set **Max connections** to between `5`-`10`. 

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.
