# Overview of Architectural Concepts


This overview details the foundational architecture, so you can understand the decisions that will impact your Cribl deployment. 

## Architectural Structure

The system is split into three functional planes:

- **Management plane (exclusive to Cribl.Cloud)**: Handles all high-level governance and tenancy. Its services manage customer Organizations, provide access control, provision isolated Workspaces, and manage billing/cost controls.
- **Control Plane (The Leader)**: The central point for configuration and operation. It manages all components, pushes updates, and orchestrates searches across Stream, Edge, Search, and Lake.
- **Data Plane**: Executes all data tasks. This includes Stream Workers (high-volume processing), Edge Nodes (local collection), Cribl Lake (storage), and Search Operators (query compute).

## System Integration
Cribl products are designed to transfer data seamlessly and without incurring duplicate costs:

- **Data in Motion**: Cribl Edge collects, then sends data to Cribl Stream for centralized processing and distribution using optimized, no-cost Cribl protocols.
- **Data at Rest**: Cribl Stream forwards data to Cribl Lake for cost-effective, long-term storage.
- **Analysis**: Cribl Search unifies querying across Cribl Lake and live Edge Nodes for integrated visibility.

## Operational Efficiency
Performance and stability are dictated by two factors:
- **Event Processing**: The time and resources required to process each event are fixed. To maximize efficiency, always perform filtering and data reduction first in the Pipeline. This must precede expensive steps like enrichment or complex transformations to minimize your overall processing load.
- **Monitoring**: Operational health requires tracking internal metrics (data flow, performance, and backpressure like Persistent Queue size). Consider forwarding the data for long-term analysis, which you can then use to inform scaling decisions and validate routing logic.