# Cribl.Cloud


The fast alternative to downloading and self-hosting Cribl Edge software is to launch [Cribl.Cloud](https://cribl.cloud/). This SaaS version, which comes in both a free and a paid version, places the Leader and the Edge Node in Cribl.Cloud, where Cribl assumes responsibility for managing the infrastructure.

By upgrading to a [Cribl.Cloud Enterprise plan](cloud-enterprise), you can implement a hybrid deployment of any complexity. In hybrid deployments, the Leader (the control plane) resides in Cribl.Cloud, while the Workers that process the data (the data plane) can reside in any combination of Cribl-managed Workers/Edge Nodes, on-prem or private cloud instances that you manage, and your data centers.

![Standard/free versus Enterprise/hybrid deployment](hybrid-vs-std-cloud-cropped.39b9045a57.png)
{scale="90%" border="true"}

> For an overview of additional features available on Enterprise plans, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

## Why Use Cloud Deployment? {#advantages}

Cribl.Cloud is designed to simplify deployment, and to provide certain advantages over using your own infrastructure, in exchange for some current [restrictions](cloud-vs-self-hosted) (because Cribl will manage some configuration on your behalf).
Cribl.Cloud offers, among others:

- Full Cribl Edge power with no responsibility to install or manage software. Cribl.Cloud is fully hosted and managed by Cribl, so you can launch a configured instance within minutes.
- Access to [Cribl Search](/search/) to search, explore, and analyze machine data in place.
- Automated delivery of upgrades and new features.
- Encrypted data at rest (configuration, sample files, etc.) at the disk level for Leader and Cribl-managed Worker instances. 
- Free, up to 1 TB/day of data throughput (data ingress + egress) for all new accounts.
- Quick expansion of your Cribl.Cloud deployment beyond the free tier's limits by purchasing credits toward metered billing. Pay only for what you use.

> If you're new to Cribl Edge, please see our [Basic Concepts](basic-concepts) page and [Getting Started Guide](/stream/getting-started-guide) for orientation. This Cribl.Cloud documentation focuses on a Cloud deployment's differences from other deployment options – referred to as "customer-managed deployments."
> 
> Cribl.Cloud always runs in distributed mode – see [Simplified Distributed Architecture](cloud-vs-self-hosted#distributed) for details.
>
{.box .info}

## Cloud Pricing {#pricing}

Beyond the [free tier](cloud-initial-setup#free), an optional paid Cribl.Cloud account – whether Standard or Enterprise – offers direct support, plus expanded daily data throughput according to your needs. From your Cribl.Cloud Organization, select **Go Enterprise** to submit an inquiry about upgrading your free account, and Cribl will respond.

You'll pay only for what you use – the data you send to Cribl Edge, and the data sent to external destinations. However, data sent to your AWS S3 storage is always free. For details, see [Pricing](https://cribl.io/pricing/).
