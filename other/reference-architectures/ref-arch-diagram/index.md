# High-Level Example Architecture


This diagram details the product connections and serves as the practical blueprint for deployment. To feature the Cribl Search and Cribl Lake capabilities, the setup assumes a Cribl.Cloud Leader serves as the control plane. The architecture is hybrid, incorporating Stream Worker Groups, Edge Nodes, and customer-managed components deployed across on-premises or private cloud environments.

![Full-suite reference architecture](reference-architecture-full-suite.png)
{scale="90%" border="true"}

> Download an editable version of this diagram in [SVG](/downloads/reference-architecture-full-suite.svg) or [draw.io](/downloads/reference-architecture-full-suite.drawio) format. Download Cribl stencils [here](https://cribl.io/wp-content/uploads/2022/07/Cribl-Stencil-Pack-ALL.zip).
>
{.box .success}

## High-Level Architecture Diagram Overview

The Cribl.Cloud reference architecture provides a unified, composable framework for managing the entire observability data lifecycle — from ingestion and real-time processing to scalable storage and integrated analysis — spanning both cloud and hybrid deployment models.

The architecture is centrally governed by the control plane, which manages all Worker Groups, supporting both Cribl-managed SaaS and Hybrid Stream configurations to meet diverse operational and cost requirements.

Telemetry from various Sources (Push, Pull, and Cribl Edge Nodes) flows into Cribl Stream for further processing. Cribl Stream then routes this data to multiple Destination types: Cribl Lake for scalable, cost-optimized object storage; generic data lakes; or real-time analytics tools (SIEM/monitoring platforms). 

If you plan to send data to the same cloud provider from which it originated, consider using [hybrid Workers](/stream/cloud-enterprise#hybrid) to minimize costs.

Finally, Cribl Search unifies the analysis layer, enabling efficient query and retrieval across all supported datasets stored in Cribl Lake and other compatible destinations.

