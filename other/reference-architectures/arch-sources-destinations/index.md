# Sources and Destinations


The core of Cribl architecture is built around the concepts of **Sources** and **Destinations**. A Source is any input that sends data to a Cribl Pipeline, and a Destination is any output that receives data from a Cribl Pipeline. 

Understanding the technical considerations for each is fundamental. These represent the architectural boundaries of the data plane. The design choices made for Sources and Destinations affect security, reliability, and performance.

## Data Formats and Protocols

Cribl is designed to be agnostic to data formats and protocols, providing broad compatibility with the diverse tooling found in enterprises.

* **Data Formats:** While Cribl can process raw data, it natively understands and can parse a wide variety of formats, including but not limited to JSON, CSV, key-value pairs, and various log formats. For observability data, Cribl supports metrics and trace formats, as well as open standards like OpenTelemetry (OTel). The choice of data format often depends on the upstream and downstream systems, and the architecture should account for the processing overhead associated with parsing and serialization.

* **Protocols:** Data is transported via numerous protocols, each with its own implications for security, reliability, and performance. Common protocols include:
    * **Syslog (TCP/UDP/TLS):** A standard for message logging. TCP and TLS variants are recommended for their reliability and security.
    * **HTTP/S (including HEC):** The backbone of many data sources, especially for application and service logs. HTTPS is critical for encrypted transport.
    * **TCP/UDP Raw:** For simple, unformatted data streams.
    * **OpenTelemetry Protocol (OTLP):** A vendor-neutral protocol for traces, metrics, and logs, typically transmitted over gRPC or HTTP.

## Authentication and Authorization

Securing data in transit and at rest is paramount. Cribl integrates with existing enterprise security models.

* **Authentication:** Sources and Destinations can be configured to require authentication. For HTTP-based inputs, this is often accomplished via authentication tokens. For other protocols like TCP, mutual TLS (mTLS) can be used to ensure that both the client and server are authenticated. Integration with secrets management systems is recommended for managing sensitive credentials.

* **Authorization:** Role-Based Access Control (RBAC) is used to govern user and system access to Cribl resources. Integration with enterprise identity providers (IdP) via SAML or OIDC allows for centralized management of user permissions.

## The Leader Role in Source and Destination Management

In a Distributed Deployment, the Leader acts as the central control plane for all nodes. From an architectural perspective concerning data flow, its primary role is the centralized configuration and management of all Sources and Destinations.

All Source and Destination configurations are defined on the Leader and then deployed to the appropriate nodes. This model ensures consistency and simplifies administration at scale. For example, adding a new Destination or modifying the TLS settings for a Source is done once on the Leader. The Leader then handles the propagation of these changes to nodes. This architecture removes the need to manage I/O configurations on individual nodes, which is critical for maintaining a reliable and governable infrastructure. The Leader also provides a global view of the health and metrics for all configured Sources and Destinations.

## Topics in This Section

[Source Architecture](arch-sources): This document provides a deep dive into the architectural considerations for data ingestion, categorizing Sources into five distinct types: Collector, Pull, Push, System, and Internal. For each type, it details common use cases and analyzes the critical architectural trade-offs related to deployment, scaling, security, and performance across both on-premise and cloud models.

[Destination Architecture](arch-destinations): This document details architectural patterns for data egress, classifying Destinations as Streaming, Non-Streaming (Batch), or Internal. It explores the design considerations for each type, with a focus on Internal Destinations (Cribl HTTP/TCP, Output Router) that enable tiered, hybrid, and resilient data routing. The document concludes with architectural guidance for common scenarios like load distribution, high availability, and secure cross-site data transport.

[Data Resilience and Workload Architecture](arch-resilience-workload): This document focuses on the cross-cutting architectural controls required to ensure data durability and manage performance. It covers essential concepts such as load balancing, backpressure, persistent queuing (PQ), and latency management. Furthermore, it details how to implement protective limits and failure recovery patterns like Dead Letter Queues (DLQ) and retry/backoff strategies to build a robust and observable data plane.