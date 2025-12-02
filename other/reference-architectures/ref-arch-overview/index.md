# Reference Architecture Overview


Before we dive into the details, let's explore the fundamental building blocks of a Cribl deployment.

## System Architecture Model

Cribl products use a distributed architecture with three key operational planes:

- **Management Plane**: Handles organizational boundaries and administrative isolation.
- **Control Plane**: Manages configuration distribution and component coordination.
- **Data Plane**: Processes, transforms, and delivers your data.

This three-plane model guides network segmentation, security policy design, and operational access across all deployment scenarios.

For details, see [The Cribl Three-Plane Model](core-arch-concepts). 

### Component Integration Overview

The Cribl data solution comprises four primary products. Understanding the role and connectivity of each component is essential for designing network topology, planning data flow, and establishing operational procedures.

- **Cribl Stream**: This is the centralized processing engine, using stateless Worker Nodes coordinated by Leaders.
- **Cribl Edge**:  A data collection (intelligent) agent installed directly on hosts to process data at the Source.
- **Cribl Search**: A query federation layer that works with stored data without needing to move it.
- **Cribl Lake**: Object storage optimized for observability data, with built-in integration for Cribl Stream and Search.

Understanding how these components integrate is important in designing your network topology, planning data flow, and establishing operational procedures.

For details, see [Product Suite Interoperation](ref-arch-products). 

## Key Architectural Considerations

Here are the primary architectural considerations that will drive your design decisions:

- **Network Architecture**: How components communicate, what ports are needed, and how data flows—all of which impact deployment and security.
- **Scaling Behavior**: How components scale up or out, including their resource use and performance under load.
- **Security Integration**:  Authentication, encryption, and access control methods that fit with your enterprise security.
- **Operational Access**: How you manage configurations, integrate monitoring, and troubleshoot issues to keep production running.

For details, see [Overview of Deployment Architecture](arch-deploy-intro). 

## Architecture Decision Framework

This reference architecture helps you tackle the core decisions when implementing Cribl:
- Where to place components based on your network and data sources. For details, see [Worker Group and Fleet Placement](worker-group-placement). 
- Strategies for scaling based on throughput and latency needs. For details, see [General Sizing Considerations](arch-onprem-planning#general-sizing-considerations). 
- How to implement security for compliance and operations. For details, see [Securing On-Prem and Hybrid Deployments.](arch-onprem-security) and [Cribl.Cloud/Customer Responsibilities](arch-cloud-security-responsibility).
- Designing your network for multi-environment deployments. For details, see [On-Prem Architecture Planning](arch-onprem-planning) and [Cribl.Cloud Architecture Overview](arch-cloud-overview).

