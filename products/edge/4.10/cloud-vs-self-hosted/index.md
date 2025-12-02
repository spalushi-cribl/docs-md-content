# Cribl.Cloud vs. Self-Hosted


A [Cribl.Cloud](deploy-cloud) deployment differs in several ways from a customer-managed (on-prem) deployment of Cribl Edge software on your own infrastructure. Keep in mind these differences as you choose how to launch Cribl Edge. Also consider these differences as you navigate the product's UI, in-app help (including tooltips), and documentation.

## Cloud-Only Features {#cloud-only}

Certain Cribl products and features are available **only** on Cribl.Cloud:

- [Cribl Search](/search/), an application for searching, exploring, and analyzing machine data in place and at API endpoints.
- [Cribl Lake](/lake/), a data lake solution for long-term, full-fidelity data storage.
- [Workspaces](/stream/workspaces), an option for isolating parallel Cribl Leaders with separate access controls and other configurations.

For details about how all Cribl products interoperate to manage data, see [Cribl Reference Architecture, Full-Suite](/suite/reference-arch-full-suite).

## Simplified Administration {#admin}

Cribl.Cloud has been designed with options to accommodate everyone – from first-time evaluators to [Enterprise](cloud-enterprise) customers who manage a worldwide network of private-cloud, public-cloud, and/or data-center deployments.

Cribl.Cloud's Free offering is designed to help you launch Cribl Edge – and to start processing data – as quickly and easily as possible. Upgrading to a paid Standard or [Enterprise plan](cloud-enterprise) provides expanded deployment and configuration options.

> For a comparison of features in Free, Standard, and Enterprise Cribl.Cloud plans, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

### Simplified Distributed Architecture {#distributed}

Cribl.Cloud is preconfigured as a Distributed deployment for [Cribl Stream](/stream/deploy-distributed) or [Cribl Edge](/edge/fleet-management). A Free or Standard plan allows only a single Fleet, and some [Distributed Settings](setting-up-leader-and-worker-nodes) cannot be configured.

With an Enterprise plan, Cribl always provides at least two Workers, and will scale up further Workers as needed to meet your peak load. With an Enterprise plan, you also have the option to configure additional [hybrid](cloud-enterprise#hybrid) Edge Nodes and Fleets.

### Git Preconfigured {#git}

Without an Enterprise plan, the **Settings** > **Global** > **System** > **Git Settings** section is omitted. However, a local `git` client is preconfigured in your Cribl.Cloud Organization. On the top nav, use the **Version Control** button (with a branched symbol) to commit/push changes to `git`. Select **Deploy** to deploy your committed changes. Cribl.Cloud does not support Git remote repos.

### Automatic Restarts and Upgrades {#auto}

Without an Enterprise plan, the **Settings** > **Controls** link are omitted. Cribl handles Leader and Fleet restarts automatically on your behalf.

### Simplified Access Management and Security {#access}

In Cribl.Cloud, you can manage access control for your Organization by selecting **Organization** in the sidebar and then selecting [**Members & Teams**](members). The options on this tab will vary depending on your plan.

If you have a Cribl.Cloud Enterprise plan, you can use the Key Management Service (KMS), which maintains the keys Cribl Edge uses to encrypt secrets on Fleets and Edge Nodes. Go to **Settings** > **Security** > **KMS** to configure KMS.

If you add an Enterprise Plan, cloud and [hybrid](cloud-enterprise#hybrid) Leaders support Local and Google SSO [authentication](/stream/authentication), along with OpenID Connect (OIDC) and SAML [federated authentication](cloud-sso). Cribl.Cloud does not currently support LDAP.

Permission- and Role-based access control (RBAC) is simplified in Cribl.Cloud. For details, see [Permissions](permissions).

#### Security Features Comparison: Self-Hosted vs. Cribl.Cloud {#security}

The following table outlines the security responsibilities for self-hosted and Cribl.Cloud deployments, highlighting key differences in deployment security, data protection, threat detection, and access control.

| Feature              | Self-Hosted                                          | Cribl.Cloud                                                    |
|-----------------------|-----------------------------------------------------|-----------------------------------------------------------------|
| **Deployment Security** | Customer responsible for securing the deployment environment (network isolation, physical access control, user access management). | Cribl manages the security of the cloud infrastructure. |
| **Configuration Security** | Customer responsible for securing configuration files and tokens. | Cribl manages the security of configuration files and tokens. |
| **Git Configuration Security** | Customer responsible for securing the Git repository. | Cribl manages the security of the Git repository. |
| **Data at Rest**       | Customer responsible for encryption and key management. | Encrypted at rest by using industry standard encryption (e.g., AES-256) for storage services.             |
| **Data in Motion**      | Customer configures encryption (TLS) for data in transit. | Pre-configured TLS for some sources. Can be further configured.     |
| **Threat Detection & Response** | Customer responsibility. Use Cribl Stream security features (limited) and external tools.  | Cribl bolsters its security posture with internal security teams and an external MSSP, ensuring comprehensive protection for its production and corporate environments through threat detection, workload scanning, and vulnerability management. |
| **Patch Management**    | Customer responsible for applying security patches to Cribl Stream and underlying infrastructure. | Cribl manages patching of Cribl.Cloud (Cribl-managed) infrastructure.            |
| **Access Control**      | Customer configures user roles and permissions (RBAC).         | Simplified RBAC with Enterprise plans offering advanced options (SSO, KMS). |
| **Authentication**      | Customer configures authentication methods. | Local and SAML/OIDC IDP with Enterprise plans (additional options).    |
| **Key Management**      | Customer manages encryption keys.                         | Enterprise plans offer Key Management Service (KMS) for key storage. |
| **Compliance**          | Customer responsible for adhering to compliance standards. | SOC 2 Type II compliant and GDPR-compliant (Cribl.Cloud).           |

### Transparent Licensing {#licensing}

The Cribl.Cloud sidebar does not display a **Settings** > **Global** > **Licensing** link, nor does the **Monitoring** > **System** submenu include **Licensing**. Your plan is managed by your Cribl.Cloud Organization, where you can check credits and usage history on the [Billing tab](cloud-portal#check-plans-credits-and-usage).

## Other Simplified Settings {#features}

These features are available only on customer-managed [hybrid](cloud-enterprise#hybrid) Cribl Stream Workers:

- [Script Collector](/stream/collectors-script)
- Staging directory support in [File-based Destinations](destinations#streaming-and-non-streaming-destinations)
- [Tee Function](tee-function)
- [File System Collector](/stream/collectors-filesystem)
- [Filesystem Destination](destinations-fs)

These features are available only in on-prem deployments:

- **Settings** > **Global** > **Scripts** (if [enabled](scripts#enable-scripts) – Cribl.Cloud does not support configuring or running shell scripts).

These features are unavailable on Cribl-managed Stream Workers in Cribl.Cloud:

- [System State Source](sources-system-state)
- AppScope Source's **Filter Settings**

Configuring [persistent queues](persistent-queues) in a Cribl.Cloud deployment requires an [Enterprise plan](cloud-enterprise).
On customer-managed (hybrid or on-prem) Worker Groups and on all Edge Nodes, you can freely define the **Max queue size**, based on the disk space you provision.

However, on Cribl-managed Worker Groups, each Source or Destination's queue is allocated a maximum of 1 GB disk space per Worker Process.
(Given this automatic configuration, Cribl-managed Sources and Destinations expose only limited PQ controls.)



## Available Ports and TLS Configurations {#ports-certs}

To get data into Cribl.Cloud, your Cribl.Cloud Organization provides several Sources and ports already enabled for you, plus 11 additional TCP ports (`20000`-`20010`) that you can use to add and configure more Cribl Edge [Sources](sources).

### TLS Details {#tls}

[TLS encryption](securing-tls-cloud) is pre-enabled for you on several Sources, also indicated on the your Workspace's **Data Sources** tab.

### Cribl HTTP and Cribl TCP Sources/Destinations {{< id `free-ingress` >}}

Use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http), and/or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp), to relay data between Edge Nodes connected to the same Leader.

> This traffic does not count against your ingestion quota, so this routing prevents double-billing. (For related details, see [Exemptions from License Quotas](licensing#exemptions).)
>
{.box .success}

## Simplified Source, Collector, and Destination Configuration

Several commonly used Sources are preconfigured for you within Cribl.Cloud's UI, and are ready to use.

The [Exec Source](sources-exec) is unavailable on Cribl-managed Workers in Cribl.Cloud, but is available on [hybrid Workers](cloud-enterprise#hybrid).

The [Cribl Internal](sources-cribl-internal) Source's CriblLogs option, when used on Cribl-managed [Worker Groups in Cribl.Cloud](/stream/cloud-workers),
contains only logs related to Sources and Destinations.
