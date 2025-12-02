# Cribl.Cloud vs. Self-Hosted


A Cribl.Cloud deployment can differ from an on-prem/customer-managed Cribl Edge deployment.
The following documentation lists the main ways in which the Free tier of Cribl.Cloud diverges from a customer-managed deployment.
Keep in mind all these differences as you navigate Cribl Edge's current UI, in-app help (including tooltips), and documentation. 

## Simplified Administration {#admin}

Cribl.Cloud has been designed with options to accommodate everyone – from first-time evaluators, to [Enterprise](cloud-enterprise) customers managing a worldwide network of private-cloud, public-cloud, and/or data-center deployments.

Cribl.Cloud's free offering is designed to help you launch Cribl Edge – and to start processing data – as quickly and easily as possible. Cribl manages many features on your behalf, allowing for a streamlined Settings left nav.

Below are the key options streamlined out of the free Cloud offering. Bear in mind that upgrading to an [Enterprise plan](cloud-enterprise) will make many of these options configurable:

### Simplified Distributed Architecture {#distributed}

Cribl.Cloud is preconfigured as a distributed deployment for [Cribl Stream](/stream/deploy-distributed) or [Cribl Edge](/edge/fleet-management). With a Free or Standard plan, allows only a single Fleet. 

Compared to self-hosted Cribl Edge, the **Settings** > **Worker Processes** and **Settings** > **Distributed Settings** links are omitted.

With an Enterprise plan, Cribl always provides at least two Workers, and will scale up further Workers as needed to meet your peak load. With an Enterprise plan, you also have the option to configure additional [hybrid](cloud-enterprise#hybrid) Edge Nodes and Fleets.

### Git Preconfigured

Without an Enterprise plan, the **Settings** > **Global Settings** > **System** > **Git Settings** section is omitted. A local `git` client is preconfigured in your Cribl.Cloud portal. On Cribl.Cloud's top nav, use the **Global Config** link (branched icon) to commit/push changes to `git`. Select **Deploy** to deploy your committed changes. Cribl.Cloud does not support Git remote repos. 

### Automatic Restarts and Upgrades

Without an Enterprise plan, the **Settings** > **Controls** and **Settings** > **Upgrade** links are omitted. Cribl handles restarts and version upgrades automatically on your behalf.

### Simplified Access Management and Security {#access}

In Cribl.Cloud, you can manage access control for your Organization by clicking **Account** > **Organization** and selecting the [**Members** tab](cloud-managing). The options on this tab will vary depending on your plan.

If you have a Cribl.Cloud Enterprise plan, you can use the Key Management Service (KMS), which maintains the keys Cribl Edge uses to encrypt secrets on Fleets and Edge Nodes. Go to **Settings** > **Security** > **KMS** to configure KMS. 

If you add an Enterprise Plan, cloud and [hybrid](cloud-enterprise#hybrid) Leaders support Local and Google SSO [authentication](/stream/authentication), along with OpenID Connect (OIDC) and SAML [federated authentication](cloud-sso). Cribl.Cloud does not currently support LDAP.

Permission- and Role-based access control (RBAC) is simplified in Cribl.Cloud. For details, see [Member Permissions](cloud-managing#member-roles).

### Transparent Licensing

The top nav's **Settings** > **Global Settings** > **Licensing** link is omitted. Your license is managed by your parent Cribl.Cloud portal, where you can check credits and usage history on the [Billing tab](cloud-portal#billing-tab).

## Other Simplified Settings

Cribl is gradually narrowing the limitations listed in this section, as Cribl.Cloud gains feature parity with on-prem deployments.

Available only on [hybrid](cloud-enterprise#hybrid), customer-managed Edge Nodes:

- [Script Collector](/stream/collectors-script)
- Staging directory support in [File-based Destinations](destinations#streaming-and-non-streaming-destinations)
- [Tee Function](tee-function)
- [File System Collector](/stream/collectors-filesystem)
- [Filesystem Destination](destinations-fs)

Available only on self-hosted deployments:

- Top nav's **Settings** > **Global Settings** > **Scripts**
  (Cribl.Cloud currently does not support configuring or running shell scripts on hybrid or Cribl-managed Edge Nodes.)

Available on Cribl-managed Edge Nodes, but unavailable on Cribl-managed Workers:

- [System State Source](sources-system-state)
- AppScope Source's **Filter Settings** 

[Persistent Queues](persistent-queues-configuring) can be configured on both hybrid and Cribl-managed Edge Nodes, with an [Enterprise plan](cloud-enterprise).
On hybrid Edge Nodes, you can freely define the **Max queue size**, based on the disk space you provision.
On Cribl‑managed Edge Nodes, each Source or Destination's queue is allocated a maximum of 1 GB disk space per Worker Process.
(Given this automatic configuration, Cribl-managed Sources and Destinations expose only limited PQ controls.)

## Support Options

At **Settings** > **Diagnostics**, you can generate diagnostic bundles and send them directly to Cribl Support. Currently, you cannot download diags. For all support options, see [Get Product Help](/support/).

## Available Ports and TLS Configurations {#ports-certs}

To get data into Cribl.Cloud, your Cribl.Cloud portal provides several Sources and ports already enabled for you, plus 11 additional TCP ports (`20000`-`20010`) that you can use to add and configure more Cribl Edge [Sources](sources). 

### TLS Details {#tls}

[TLS encryption](securing-tls-cloud) is also pre-enabled for you on several Sources, also indicated on the Cribl.Cloud portal's **Data Sources** tab.

> ##### Cribl HTTP and Cribl TCP Sources/Destinations {{< id `free-ingress` >}}
>
> Use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http), and/or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp), to relay data between Edge Nodes connected to the same Leader. This traffic does not count against your ingestion quota, so this routing prevents double-billing. (For related details, see [Exemptions from License Quotas](licensing#exemptions).)
>
{.box .success}

## Simplified Source, Collector, and Destination Configuration

Several commonly used Sources are preconfigured for you within Cribl.Cloud's UI, and are ready to use.

The [Exec Source](sources-exec) is unavailable on Cribl-managed Workers, but is available on [hybrid Workers](cloud-enterprise#hybrid).

The [Cribl Internal](sources-cribl-internal) Source's CriblLogs option is unavailable in Cribl-managed Stream instances, but it is available in Cribl Edge, and in hybrid Workers' Stream instances. The Cribl Internal > CriblMetrics option is available in all of the above combinations. 




