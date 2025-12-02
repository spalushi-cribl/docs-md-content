# Required Ports in Cribl.Cloud


Understanding the ports used across Cribl.Cloud is the key to clarifying your firewall rules. This functional view illustrates the connections for data flow, management traffic, and communication channels between the Cribl.Cloud Leader, Cribl Stream, and Cribl Edge components.

Before deploying any Hybrid Worker Groups or Edge Fleets, coordinate with your network team to ensure the necessary firewall rules and Network Security Groups (NSGs) are configured.

![Ports in Cribl.Cloud](ref-arch-ports.png)
{scale="90%" border="true"}

This Ports in Cribl.Cloud diagram illustrates the port-level connectivity across Cribl’s architecture for Cribl.Cloud and hybrid deployments. The Cribl.Cloud Leader Node acts as the central control plane, directing traffic to the managed Worker Groups, while Cribl Edge facilitates data collection closer to the Source, and Hybrid Workers handle the processing workload.

All listed ports are mandatory for system operation and data flow, with two exceptions: Direct User UI Access to Hybrid Worker Groups (Port #6) and direct User UI Access to Edge Nodes (Port #12) are optional. Users can access these UIs centrally through the Cribl.Cloud Leader Node.

## Cribl Architecture Connectivity: Port-by-Port Breakdown

| \# | Protocol/Port | From | To | Purpose |
| :---: | :---: | :---: | :---: | :---- |
| **1** | 443 HTTPS | Leader Node | ai.cribl.cloud | **AI Service Exchange:** The Leader Node initiates secure communication with external Cribl AI services and receives data back. |
| **2** | 443 HTTPS | Users | Leader Node | **Management UI Access:** Users access the Leader Node UI securely for configuration and monitoring. |
| **3** | 4200 TCPS/TLS | Hybrid Stream Worker Group(s) |Leader Node  | **Inter-Component Data Flow**: The primary data channel for Hybrid Workers to receive configuration/data from the Leader Node. |
| **4** | 4200 HTTPS| Hybrid Stream Worker Group(s) | Leader Node |  **Control/Data Channel:** Management communication and secure data transfer to Hybrid Workers from the Leader Node. |
| **5** | 443 HTTPS | Hybrid Stream Worker Group(s) |  Leader Node | **Registration and Status Reporting:** Hybrid Workers check in and report operational status back to the Leader Node. |
| **6** | 9000 HTTP/S| User | Hybrid Stream Worker Group(s) | **UI Access**: Optional user access to Cribl Stream UI. |
| **7** | 443 HTTPS | CDN (`cdn.cribl.io`) | Hybrid Stream Worker Group(s) | **Configuration/Content Retrieval:** Cribl Stream retrieves configuration and update files from the Cribl Content Delivery Network |
| **8** | 10200 Cribl TCP/Cribl HTTP | Cribl Edge | Hybrid Stream Worker Group(s) | **Data Forwarding (Cribl HTTP):** Cribl Edge forwards collected event data to the Hybrid Workers for processing, using the standard Cribl HTTP protocol. |
| **9** | 10200 Cribl TCP/Cribl HTTP | Cribl Edge | Cribl-Managed Stream Worker Group(s) | **Data Forwarding (Cribl HTTP):** Cribl Edge forwards collected event data to the Cribl-Managed Workers for processing, using the standard Cribl HTTP protocol. |
| **10** | 443 HTTPS | Cribl Edge | CDN (`cdn.cribl.io`) | **Configuration/Content Retrieval:** Cribl Edge retrieves configuration and update files from the Cribl Content Delivery Network. |
| **11** | 443 HTTPS | Cribl Edge | Leader Node | **Edge Control/Status:** Cribl Edge Nodes check in and report operational status back to the Leader Node. |
| **12** | 4200 HTTPS | Cribl Edge | Leader Node |  **Inter-Component Data Flow** The primary data channel for the Edge Nodes to receive configuration/data from the Leader Node. |
| **13** | 9420 HTTP/S| Users | Cribl Edge | **UI Access**: Optional user access to Cribl Edge UI. |



For details, see [Cribl Stream Ports](/stream/ports/) and [Cribl Edge Ports](/edge/ports/).