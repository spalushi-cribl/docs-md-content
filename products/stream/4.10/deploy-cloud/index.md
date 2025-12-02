# About Cribl.Cloud


In a [Cribl.Cloud](https://cribl.cloud/signup) deployment, Cribl assumes responsibility for hosting and managing your Cribl Stream infrastructure.

{{< video 475959688 >}}

Cribl.Cloud comes in both free and paid versions.
A free Cribl.Cloud account allows up to 1 TB/day of data throughput (data ingress + egress),
but does not have the option to have multiple Worker Groups.
By upgrading to a [Cribl.Cloud Enterprise plan](cloud-enterprise), you can implement a [hybrid deployment](cloud-enterprise#hybrid) of any complexity.

In hybrid deployments, the Leader resides in Cribl.Cloud, and manages Stream Workers and Edge Nodes regardless of their location.
Your data processing configuration can consist of any combination of Workers and Edge Nodes: Cribl-managed, in private cloud instances that you manage, or in your data centers.

![Standard/free versus Enterprise/hybrid deployment](hybrid-vs-std-cloud-cropped.39b9045a57.png)
{scale="70%" border="true"}

> For a comparison of features an capabilities of Cribl.Cloud vs an on-prem deployment, see [Cribl.Cloud vs. Self-Hosted](cloud-vs-self-hosted).
> 
> For an overview of additional features available on Enterprise plans, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

> If you're new to Cribl Stream, please see [Cribl Stream Basic Concepts](basic-concepts) and the [Getting Started Guide](getting-started-guide) for help onboarding. Cribl.Cloud always runs in Distributed mode – for details about this, see [Simplified Distributed Architecture](cloud-vs-self-hosted#distributed).
>
{.box .info}

## Cloud Pricing {#pricing}

Beyond the [free tier](cloud-initial-setup#free), an optional paid Cribl.Cloud account – whether Standard or Enterprise – offers direct support, plus expanded daily data throughput according to your needs. From your Cribl.Cloud Organization, select **Go Enterprise** to submit an inquiry about upgrading your free account, and Cribl will respond.

You'll pay only for what you use – the data you send to Cribl Stream, and the data sent to external destinations. However, data sent to your AWS S3 storage is always free. For details, see [Pricing](https://cribl.io/pricing/).
