# Standard Configuration Management

Cribl offers [two approaches to configuration management](arch-configuration-management#configuration-management-approaches). The choice between these two approaches depends on your existing workflows and business requirements.

This guide explains the standard approach using the standard, out-of-the-box configuration management features provided by the Cribl control plane. In this approach, the Leader Node acts as the central configuration management hub. You make configuration changes through the Cribl UI or direct API calls to the Leader Node. The Leader tracks changes in its local Git repository and distributes the configuration bundle to Worker Groups or Fleets upon a commit and deploy.

## When to Use the Standard Cribl Control Plane Approach

You should consider using this approach with your initial deployment. It accelerates and simplifies the initial setup and minimizes dependencies because it does not require external Git servers or CI/CD pipelines. Your change management system is operational immediately after installing the software and setting up your initial system architecture.

The overall approach is also easy to understand because configuration, deployment, and monitoring are all managed from a single UI on the Leader Node. This model provides immediate access to version control and rollback features. Because you store the configuration history locally on the Leader, rolling back to a previous, known-good version is a quick action built directly into the workflow.

Over time, if your deployment requires a more scalable, automated configuration management approach, you can adapt your existing deployment to the Pack-based configuration management approach.

While simple and effective for small deployments, system architects should be aware of the inherent limitations of relying solely on the local control plane for configuration management in large, enterprise environments:

- **Limited External Auditability:** The configuration's Git history is internal to the Leader Node. While this provides a basic audit trail for Cribl users, it is decoupled from your company's enterprise-wide version control system, which could impact security and compliance audits.
- **Manual Deployment:** You initiate deployments either manually through the UI or by an automation script making a direct API call to the Leader. For some deployments, this can make scaling difficult over time.
- **Dependency on the Leader Node:** The integrity of your configuration history and the ability to deploy changes relies entirely on the health and backup of the Leader Node. If the Leader Node is lost and the local configuration is not separately backed up, the full history and last committed state could be lost, impacting disaster recovery planning.

> Cribl.Cloud deployments have high-availability redundancy built in for Leader nodes. For on-prem deployments, you need to design your architecture for high availability to mitigate this dependency. See [High Availability Architecture](arch-deploy-ha) for specific recommendations.
>
{.box .info}

If these limitations are blocking factors for your deployment, consider using the [Advanced Configuration Management](arch-config-manage-advanced) approach instead.

## Overview of the Cribl Control Plane Approach

In this approach, the Leader Node acts as the central configuration management hub and is the single authority for all configurations. When you deploy Cribl, it uses a local Git repository to track and version control all configuration changes for that Leader.

You make configuration changes directly through the Cribl UI or via direct API calls to the Leader Node. The Leader distributes the configuration bundle to Worker Groups or Fleets upon a manual commit and deploy. The API can also commit and deploy configuration changes programmatically.

## For More Information

For more information about the standard control plane approach, see:

- [Stream: Version Control](/stream/version-control/)
- [Edge: Manage Fleets of Edge Nodes](/edge/fleet-management/)
