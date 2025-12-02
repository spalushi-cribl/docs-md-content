# Cribl.Cloud


If you select a [Cribl.Cloud](https://cribl.cloud/) deployment, Cribl assumes responsibility for hosting and managing your Cribl Stream infrastructure.

Cribl.Cloud comes in both free and paid versions. By upgrading to a [Cribl.Cloud Enterprise plan](cloud-enterprise), you can implement a **hybrid deployment** of any complexity.

In hybrid deployments, the Leader resides in Cribl.Cloud, and manages Stream Workers and Edge Nodes regardless of their location.
Your data processing configuration can consist of any combination of Workers and Edge Nodes: Cribl-managed, in private cloud instances that you manage, or in your data centers.

![Standard/free versus Enterprise/hybrid deployment](hybrid-vs-std-cloud-cropped.39b9045a57.png)
{scale="70%" border="true"}

> For an overview of additional features available on Enterprise plans, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

## Why Use Cloud Deployment?

Cribl.Cloud is designed to simplify deployment, and to provide certain advantages over using your own infrastructure, in exchange for some current [restrictions](cloud-vs-self-hosted) (because Cribl will manage some configuration on your behalf). Cribl.Cloud offers these advantages, among others:

- Full Cribl Stream power with no responsibility to install or manage software for the Leader Node.
  Cribl.Cloud is fully hosted and managed by Cribl, so you can launch a configured instance within minutes.
- Access to [Cribl Search](/search/) to search, explore, and analyze machine data in place.
- Access to [Cribl Lake](/lake/), a data lake solution for long-term, full-fidelity data storage.
- Access to [Workspaces](/stream/workspaces), where you can isolate parallel Cribl Leaders with separate access controls and other configurations.
- Automated delivery of upgrades and new features.
- Leaders and Cribl-managed Worker Nodes encrypt data at rest (including configuration and sample files) at the disk level.
- Free, up to 1 TB/day of data throughput (data ingress + egress) for all new accounts.
- Quick expansion of your Cribl.Cloud deployment beyond the free tier's limits by purchasing credits toward metered billing. Pay only for what you use.

> For a more-detailed comparison of Cribl.Cloud versus customer-managed ("on-prem") deployments of Cribl Stream software on your own infrastructure, see [Cribl.Cloud vs. Self-Hosted](cloud-vs-self-hosted).
>
> If you're new to Cribl Stream, please see [Cribl Stream Basic Concepts](basic-concepts) and the [Getting Started Guide](getting-started-guide) for help onboarding. Cribl.Cloud always runs in Distributed mode – for details about this, see [Simplified Distributed Architecture](cloud-vs-self-hosted#distributed).
>
{.box .info}

## Cloud Pricing {#pricing}

Beyond the [free tier](cloud-initial-setup#free), an optional paid Cribl.Cloud account – whether Standard or Enterprise – offers direct support, plus expanded daily data throughput according to your needs. From your Cribl.Cloud Organization, select **Go Enterprise** to submit an inquiry about upgrading your free account, and Cribl will respond.

You'll pay only for what you use – the data you send to Cribl Stream, and the data sent to external destinations. However, data sent to your AWS S3 storage is always free. For details, see [Pricing](https://cribl.io/pricing/).
