# Deploy Hybrid Workers and Edge Nodes

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

Hybrid deployments combine Cribl.Cloud Government's centralized management with the security of your on-premises infrastructure. This setup allows for operational efficiency while meeting compliance needs.

In a hybrid architecture, the Cribl.Cloud Government control plane handles configuration, monitoring, and orchestration. Meanwhile, your Worker Groups and Fleets operate within your agency's infrastructure. This design ensures sensitive data stays within your controlled environment, even as you benefit from cloud-based management.

For details, see [Hybrid Deployment](/stream/cloud-enterprise/#hybrid).

## FIPS Considerations for Cribl Stream and Cribl Edge

When deploying Cribl Stream and Cribl Edge in FedRAMP environments, ensure **FIPS Mode is properly configured** on all Worker Groups and Fleets. This step is essential for maintaining cryptographic compliance with Federal standards.

For detailed instructions on configuring FIPS-compliant cryptographic operations, refer to the following tutorials:

* [FIPS Mode for Cribl Stream](/stream/fips-mode/)
* [FIPS Mode for Edge (Linux)](/edge/fips-mode/)
* [FIPS Mode for Edge (Windows)](/edge/fips-mode-win/)

> The linked guides provide detailed deployment instructions. Cribl.Cloud Government offers comparable features to our commercial platform, and these guides are applicable to both environments.
>
{.box .info}

## Cribl Stream Deployment Guides

| Title | Description |
|-------|-------------|
| [Bootstrap Workers from Leader](/stream/deploy-workers) | Add a new Worker Node.|
| [Docker Deployment](/stream/deploy-docker) | Deploy Cribl Stream Worker Groups using Docker containers. |
| [Kubernetes Worker Deployment](/stream/deploy-kubernetes) | Deploy and manage Cribl Stream Worker Groups in Kubernetes clusters with automated scaling and management. |

## Cribl Edge Deployment Guides

| Title | Description |
|-------|-------------|
| [Deployment Planning](/edge/deploy-planning/) | Planning guide for Cribl Edge deployments including architecture, sizing, and infrastructure considerations. |
| [Installing Cribl Edge on Linux](/edge/deploy-single-instance/) | Installation and configuration guide for deploying Cribl Edge on Linux systems. |
| [Installing Cribl Edge on Windows](/edge/deploy-windows/) | Installation and setup instructions for deploying Cribl Edge on Windows environments. |
| [Installing Cribl Edge on MacOS](/edge/deploy-macos) | Installation and configuration guide for deploying Cribl Edge on MacOS. |
| [Run Cribl Edge in a Docker Container](/edge/deploy-running-docker/) | Deploy Cribl Edge using Docker containers for flexible, portable edge data processing capabilities. |
| [Deploy Edge via Kubernetes](/edge/deploy-running-kubernetes/) | Orchestrate Cribl Edge deployments in Kubernetes environments for scalable, managed edge processing. |
