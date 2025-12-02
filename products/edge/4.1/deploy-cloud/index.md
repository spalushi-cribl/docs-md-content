# Launch Guide


The fast alternative to downloading and self-hosting Cribl Edge software is to launch Cribl.Cloud. This SaaS version, whether free or paid, places the Leader and the Edge Node in Cribl.Cloud, where Cribl assumes responsibility for managing the infrastructure.

By upgrading to a [Cribl.Cloud Enterprise plan](#enterprise), you can implement a hybrid deployment of any complexity. In hybrid deployments, the Leader (the control plane) resides in Cribl.Cloud, while the Workers that process the data (the data plane) can reside in any combination of Cribl-managed Workers/Edge Nodes, on-prem or private cloud instances that you manage, and your data centers.

![Standard/free versus Enterprise/hybrid deployment](hybrid-vs-std-cloud-cropped.39b9045a57.png)
{scale="90%" border="true"}

> For an overview of additional features available on Enterprise plans, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

## Why Use Cloud Deployment? (Advantages) {#advantages}

Cribl.Cloud is designed to simplify deployment, and to provide certain advantages over using your own infrastructure, in exchange for some current restrictions (because Cribl will manage some configuration on your behalf):

- Tap Cribl Edge’s power, with no responsibility to install or manage software. Cribl.Cloud is fully hosted and managed by Cribl. so you can launch a configured instance within minutes.
- Automated delivery of upgrades and new features.
- Encrypted data at rest (configuration, sample files, etc.) at the disk level for Leader and Cribl-managed Worker instances. 
- Free, up to 1 TB/day of data throughput (data ingress + egress) for all new accounts.
- Quickly expand your Cribl.Cloud deployment beyond the free tier's limits by purchasing credits toward metered billing. Pay only for what you use.



## Getting Started

Your first step is to sign up on the Cribl.Cloud portal (see [Registering a Cribl.Cloud Portal](#register) below), to create your Cribl.Cloud [Organization](#organization).

Your Organization will display a dedicated [Portal](#workspaces), a network and access boundary that isolates your Cribl resources from all other users. Each Cribl.Cloud account provisions a separate AWS account. Your [instances](#managing) of Cribl Stream, Cribl Edge, and Cribl Search are deployed inside a virtual private cloud (VPC) in this account.

{{< id `free` >}}
The Portal will initially be on a **free** Cribl.Cloud plan. Certain throughput and administration limits apply to a free account. When you need more capacity and/or options, it's easy to upgrade to a [paid](#pricing) or [Enterprise](#enterprise) plan – just click the **Go Enterprise** button at the top of your Portal.

> As of November 2022, the Cribl.Cloud Suite is listed on [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-gwiqiktubqhhk). When you're ready for a paid plan, you can use your Enterprise Discount Program (EDP) credits here to run Cribl products, billed through your AWS account – with no need for a separate procurement process.
> 
> As of June 2022, Cribl [completed](https://cribl.io/news/cribl-achieves-soc2-certification/) its SOC 2 (Service Organization Control 2) Type II security compliance attestation.
>
{.box .info}

## About Cribl Edge (and This Document) {#baseline}

If you're new to Cribl Edge, please see our [Basic Concepts](basic-concepts) page and [Getting Started Guide](/stream/getting-started-guide) for orientation. The current topic focuses on a Cloud deployment's differences from other deployment options – referred to below as "Cribl Edge binaries" or "customer-managed deployments."

> Cribl.Cloud always runs in distributed mode – see [Simplified Distributed Architecture](#distributed) below for details.
>
{.box .success}

## Registering a Cribl.Cloud Portal {#register}

Ready to take the red pill? The next few sections explain how to register and manage a Cribl.Cloud instance.

First, if you haven't already [signed up on Cribl.Cloud](#getting-started): 

1. Start at: [https://cribl.cloud/signup/](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink) 
1. Select the **New User? Free signup** option, and register with your work email address.
1. Use the verification code from Cribl's email to confirm your registration.
1. On the **Create Organization** page, optionally enter an **Organization Name** (a friendly alias for the randomly generated ID that Cribl will assign to your Organization).
1. Select an AWS Region to host your Cribl.Cloud Leader and Cribl-managed Workers. Cribl currently supports either the `US West (Oregon)` or  `US East (Virginia)` Region.
1. Bookmark your Cribl.Cloud portal page, for all that follows.

![Create Organization page – selecting a host Region](se-cloud-select-region.e84cbcb679.png)
{scale="50%"}

Note that each user can register only one active Organization. For details, see notes on [Member Roles](#member-roles).

> ##### {{< id `troubleshooting` >}} Troubleshooting Resources
>
> [Cribl University](https://cribl.io/university/)'s Troubleshooting Criblet on [How to Log Into Cribl.Cloud](https://university.cribl.io/#/online-courses/b4f1d236-8ebd-4f38-9f9c-d9b301294994) walks you through the whole registration and login flow. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
>
{.box .info}

### Select Organization Page {#select-org}

When you own or are a Member of multiple Cribl.Cloud Organizations, the **Select Organization** splash page – displayed after you sign in – enables you to select which Organization you want to work with. 

![Select Organization interstitial page](select-org-page.b70c392a5d.png)

Click any tile's `\/` accordion to reveal a [detailed description](#details-tab), if provided. Click the appropriate tile (or its open accordion's **Dashboard** button) to configure that Organization.

![Organization tile's details and controls](org-tile-details.6303dbf5ed.png)
{scale="60%"}

You can click **Leave** if you want to remove yourself as a Member of another Owner's Organization. This option requires confirmation – proceed only if you're sure! (You won't see this button on Organizations that you own.)

## Exploring the Cribl.Cloud Portal {#exploring}

Now that you're here – explore the furniture. The Cribl.Cloud portal's top navigation allows you to navigate among the following pages/links: 

- [Portal](#workspaces) (**Cribl.Cloud** logo)
- [Messages](#messages)
- [Learning](#learning)
- [Software](#download)
- [Account](#account) (including [Organization](#organization) details)

### Portal Page (Cribl.Cloud Logo) {#workspaces}

When you log into the Cribl.Cloud portal, you'll land here. The main events here are the **Manage Stream**, **Manage Edge**, and **Explore [Search]** buttons. Click these to launch (respectively) [Cribl Stream](/stream/), [Cribl Edge](/edge/), or [Cribl Search](/search/) in a new tab.


![Cribl.Cloud portal](cloud-portal-overview-tab.458c519218.png)
{border="true"}

However, the surrounding page offers lots more useful information:
- On the page body, you'll find links to multiple Cribl resources – documentation, support (Community Slack and bug reporting), free Sandbox training, and blog posts. 
- In the [Overview strip](#overview) just below the top black menu, you'll find detailed configuration information about your Cloud Organization. 
- By clicking the top nav's [⚙️ **Network Settings**](#config) link, you can check and manage connectivity details – data Sources, access control, and trust relationships – for your Cribl-managed Cloud Workers.

#### Overview Strip and Access Details (Group Details) Modal {#overview}

From left to right, this upper strip displays the following config details: 

**Org ID**: Domain at which you access the associated Cribl.Cloud Organization.

**Updated**: Date on which Cribl last pushed an infrastructure change (notably including changes to the **Egress IPs** listed below).

**Version**: The version of Cribl Stream/Edge applications deployed to your Organization and its Cribl-managed Workers. 

**Region**: The AWS Region where you're running Cribl applications. (Cribl.Cloud currently supports either the **us‑west‑2** or **us‑east‑1** Region.)

**Access Details**: Click this button to open a modal whose right column provides a consolidated, read-only display of the following **Stream Worker Group Details**. Some of these options are configured on different tabs, as noted below.

- **Worker Group**: Use this drop-down to select any Group of Cribl-managed Workers that you've configured (including `default`). The remaining fields on the right will display details specific to that Group.

- **Provision Now**: This button will replace all the fields listed below when a Group is dormant. Click the button when you're ready to provision infrastructure for the Group. After a lag, the Group will be ready to process data, and this modal's remaining fields will populate.

![Access Details modal for an unprovisioned Group](cloud-access-details-deprov.be294dac46.png)
{border="true"}

- **Trust**: Role ARN for Workers in this Group. You configure these ARNs on the [Trust Tab](#trust-tab).

- **Ingress IPs**: The IPv4 ingress addresses associated with your Cribl.Cloud Organization's endpoints (global to all Groups). These addresses will remain constant, so you can build firewall rules around them.

- **Public ingress address**: Each Group's domain for inbound data. This address prepends the Group name to the Organization's global domain name. It does not append ports per data type – you can obtain these from the [Data Sources](#data-sources-tab) tab. 

- **Egress IPs**: Your Cribl.Cloud Organization’s current public IP addresses. These addresses are Group-specific and also dynamic; Cribl will occasionally update them when we need to rescale core infrastructure. Cribl.Cloud currently does not offer static egress IPs.

![Access Details modal for a provisioned Group](cloud-access-details.ed1d2d538c.png)
{border="true"}

> Configuring Stream Groups (beyond the `default` Group) requires an Enterprise plan. For details about creating and provisioning Groups, see [Cribl.Cloud Worker Groups](/stream/deploy-distributed#cloud-groups).
> 
> The **Access Details** modal's left side displays Organization-wide access details, including the **Cribl.Cloud URL** of your Org's Leader/control pane. You'd use this URL for certain API calls and certain [Collection](/stream/collectors) operations coordinated by the Leader. Use the right-side details to configure data flow through individual Groups.
>
{.box .info}

#### Network Settings Page {#config}

Clicking the top nav's ⚙️ **Network Settings** link opens a page with connectivity details, spread across three upper tabs: **Data Sources**, **ACL**, and **Trust**.

##### Data Sources Tab {#data-sources-tab}

The **Data Sources** tab lists ports, protocols, and data ingestion inputs that are open and available to use. Use the **Group** drop-down to filter these details per Group of Cribl-managed Workers in the Stream app. Return to this tab to copy Ingest Addresses (endpoints) as needed. For details, see [Available Ports and TLS Configurations](#ports-certs).

##### Trust Tab {#trust-tab}

The **Trust** tab provides **Worker ARN**s (Amazon Resource Names) that you can copy and paste to attach a Trust Relationship to an AWS account's IAM role. Use the **Group** drop-down to display the ARN for any Group of Cribl-managed Stream Workers. 

Attaching a Trust Relationship enables the `AssumeRole` action, providing cross-account access. For usage details, see the [AWS Cross-Account Data Collection](/stream/usecase-aws-x-account#account-b-configuration) topic's **Account B Configuration** section.

> This option applies only to your Cribl-managed Workers. You cannot use this technique to enable access to hybrid Workers on customer-managed Cribl Edge instances.
>
{.box .warning}

##### ACL Tab {#acl-tab}

This **Access Control List** defines Rules (IPv4 CIDR ranges) to restrict data sent to your data sources. The Rules you define here are global to all your Cribl-managed Groups of Stream Workers.

The default `0.0.0.0/0` rule (modifiable) imposes no limits. Click `+` to add more rules, or click `X` to remove rules. End a rule with `/32` to specify a single IP address, or with `/24` to enable a whole CIDR block from `x.x.x.0` to `x.x.x.255`.

Click **Save** after adding, modifying, or removing rules. Each change takes up to 5 minutes to propagate. Cribl.Cloud will display an `ACL update in progress...` banner, notifying you that rules edits are temporarily disabled to prevent conflicts. A successful update proceeds silently – you will not see a confirmation message.

> The **ACL** options apply only to your Cribl-managed Workers. You cannot use this technique to set access rules on [hybrid Workers](#hybrid) running in customer-managed Cribl Edge instances.
>
{.box .warning}

### Cribl Edge UI Access {#stream}

Clicking the **Manage Stream** or **Manage Edge** button opens (respectively) your Stream or Edge Leader in a new browser tab. All of the application's Cloud-supported features are available from this landing page.

### Messages Drawer {#messages}

Clicking the top nav's **Messages** link opens the **Message Center** right drawer. Here, you will find Cribl.Cloud status and update notifications from Cribl, with **Unread** messages above the **Read** group.

### Learning Page {#learning}

Clicking the top nav's **Learning** link opens the **Learning** page, which provides links to everything you need to learn about Cribl Edge in order to goat forth and do great things:

- Sandboxes (free, interactive tutorials on fully hosted integrations).
- Documentation.
- Product and plans overview (pricing comparison).
- Cribl events (including future and archived Webinars).
- Concept/demo videos.

### Software Page {#download}

If you prefer to take the blue pill, this page offers download links for Cribl Edge, Cribl Edge, and [AppScope](https://appscope.dev/) software. You can download either binaries or Docker containers (hosting Ubuntu 20.04), to install and manage on your own hardware or virtual machines.

### Account Menu {#account}

This menu offers a self-explanatory **Sign Out** link, and an **Organization Selection** submenu (fly-out) that works like the [Select Organization](#select-org) page: click its links to traverse to other Organizations. For an Organization's owner only, it also includes a link to the [Organization](#organization) page.

![Account tab](cloud-account-tab.88c5a7a261.png)
{border="true"}

### Organization Page {#organization}

Displayed only to an Organization's owner, this page offers [Details](#details-tab), [Members](#members-tab), and (where applicable) [Billing](#billing-tab) and [SSO](#sso-tab) tabs along its top.

#### Details Tab {#details-tab}

The **Organization** > **Details** tab offers these controls to make your Cribl.Cloud deployment more recognizable than its randomly generated **Organization ID** (displayed at the top):

**Alias**: Optionally, enter a "friendly" name for your Organization. Upon signing in, Members will see this alias above the Organization ID on the [Select Organization](#select-org) page.

**Description**: Optionally, use this field to add further details about your Organization. On the [Select Organization](#select-org) page, Members can view these details by expanding the Organization's tile.

**Opt in to beta features**: If displayed, this toggle enables access to new options that Cribl has not yet made generally available. As with all beta features, expect some instability in exchange for advancing to the cutting edge of your Cloud.

Click **Save** to immediately apply your changes. 

![Organization Details tab](org-details.1c9341d61d.png)
{border="true"}

#### Members Tab {#members-tab}

The **Organization** > **Members** upper tab provides access to [inviting and managing other users](#multi-user).

#### Billing Tab {#billing-tab}

The **Organization** > **Billing** upper tab is displayed only to owners of an Organization on a paid license plan. It provides [Plan](#plan) and [Usage](#metrics) left tabs.

##### Plan & Invoices Tab {#plan}

The **Plan & Invoices** left tab displays a mercury bar of purchased, used, and available **Credits** on your account. Color-coding breaks down usage by infrastructure versus processing (data throughput).

Below that is an expandable **Plan** details section. Expandable **Monthly Usage History** rows offer details about your credits usage in the current and prior months. Here, you can break out usage on Cribl Search, on hybrid Workers (ingest billing only), and on Groups of Cribl-managed Workers (with ingest versus infrastructure breakouts per Group).

![Billing > Plan & Invoices tab](se-cloud-billing-credits.4cd85cbdbd.png)
{border="true"}

> Credits carry over across billing periods, as long as you renew your Cribl.Cloud plan.
>
{.box .success}

##### Usage Tab {#metrics}

The **Usage** left tab provides nested tabs for **Stream** versus **Search**. (If you're keeping tabs, this is a third level of selectable tabs.) 

The **Stream** tab graphs credits usage for **Data in**, **Data out**, and **Infrastructure**  over a selectable trailing period of 7 days to 1 year. The trend line shows daily averages, and you can hover over data points to pop out details. 

At the right side of each graph is a total for the selected period. On the left drop-down, you can filter the aggregate display down to individual Cribl-managed Groups, or to all hybrid Groups.

![Billing > Usage tab](se-cloud-billing-metrics.a63ad8f114.png)
{border="true"}

The **Search** tab displays billed compute CPU hours. It provides a similar graph, with averages over time and a total per period. You'll find the same options for time granularity.

#### SSO Tab {#sso-tab}

This tab appears on an [Enterprise plan](#enterprise), enabling you to configure federated authentication to your Cribl.Cloud Organization from an OIDC or SAML identity provider. For details, see [Cribl.Cloud SSO Setup](cloud-sso).

## Managing Cribl.Cloud {#managing}

Once you've registered on the portal, here's how to access Cribl.Cloud:

1. Sign in to your Cribl.Cloud portal page.
1. Select the Organization to work with.
1. From the portal page, select **Manage Stream**, **Manage Edge**, or **Explore [Search]**.
1. The selected application's UI will open in a new tab or window – ready to goat!

Note the **Cribl.Cloud** link at the Cribl.Cloud home page's upper left, under the **Welcome!** message. You can click this link to reopen the Cribl.Cloud [portal page](#workspaces) and all its resources. 

![Cribl.Cloud link takes you back to the portal](tenant-id.a42a9016d3.png)
{border="true"}

## Inviting and Managing Other Users {#multi-user}

From the **Organization** > **Members** tab, an Organization's owner can invite new users to join the Organization, assign access Roles to new and existing Members, remove pending invites, and remove existing Members.

![Organization > Members tab: Managing Invites and Members](cloud-members-ent.ab4e204554.png)
{border="true"}

### Inviting Members {#inviting-members}

Click **Invite Member** to open the modal shown below. Enter the **Email address** of the new user you want to invite, assign them a **Role** (explained just below), and then click **Invite** to send the invitation.

![Invite User modal](cloud-invite-ent.55a7994c84.png)

### Member Roles {#member-roles}

Each Role that you can assign to Members confers a [default Role](roles#default-roles) within the Organization's hosted applications. Here are the Roles, their corresponding permissions, and who can assign each:

Member Role | Cribl Edge Role | Options/Restrictions
--- | --- | ---
`Admin`  | `admin` | Any Organization owner can assign.
`Editor` | `editor_all` | Assignable only with Enterprise plan.
`Read-Only` | `reader_all` | Assignable only with Enterprise plan.
`Owner` | `admin` | Any Organization owner can share or transfer.
`Project User` | `project_user` | Assignable only with Enterprise plan. Provides access to Stream [Projects](/stream/projects).

Note that: 

-  Without an Enterprise plan, a Cribl.Cloud Organization's Owner can assign the `Owner` or `Admin` Role to other users. (For all available Enterprise features, see [Pricing](https://cribl.io/pricing/).) 

- An Enterprise plan is also required for expanded role-based access control – i.e., the ability to manage the `Editor`, `Read‑Only`, and `Project User` Roles shown above.

- Only an Organization's Owner can manage the Organization's [Details tab](#details-tab).

- An Organization's Owner Role can be shared among multiple users. (Among other use cases, this provides a way to gradually transfer primary ownership.) Also, a user can be invited to the Owner Role on multiple Organizations.

- However, each user – as defined by their email address – can [register](#register) only one active Organization, and only if they are not already the Owner of a different Organization. 

- You assign Roles per individual user, when you [invite them](#inviting-members) to your Organzation. Cribl.Cloud does not currently support globally predefining or assigning group Roles, as with on-prem Cribl Edge. However, Admins can change users' Roles after those users join their Organization.

> ##### Cribl.Cloud Roles Rule Cribl Edge Access {{< id `roles-rules` >}}
>
> When you assign a Cribl.Cloud Member Role, it is mapped to a Cribl Edge Role as described above. However, these users will **not** be visible as [local users](local-users) within the UI of Cribl Edge Cloud instances managed by Cribl. 
> 
> Also, within these instances' UI: Modifying Roles **not** mapped above will have no effect, and adding local users will have no effect.
>
{.box .warning}

### Responding to Invites

At the address you entered, the new Member receives an email with an **Accept Invitation** link to either sign into their existing Cribl.Cloud account, or else sign up to create an account and its credentials. 

After signing in, they'll have access to your Organization and Cribl Edge instance at the Role level you've specified.

### Managing Invites

While an invite is pending, the **Organization** > **Members** tab offers you these options to deal with commonly encountered issues:

- **Reinvite**: If your invited Member didn't receive your invitation email, you can click this button to resend it.

- **Copy Link**: If emails aren't getting through at all, click this button to copy and share a URL that will take the invitee directly to the signup page. This target page encapsulates the same identity, Organization, and Role you specified in the original email invite.

- **Remove**: This is for scenarios where you need to revoke a pending invite. (You sent someone a duplicate invite, your invitee is spending too much time in space to be a productive collaborator, etc.) After clicking this button, you'll see a confirmation dialog.

After 7 days, if an invite has been neither accepted nor revoked, it expires. In this case, it is removed from the **Members** tab.

![Managing Invites](cloud-members-invites-cropped.49dc973485.png)
{border="true"}

### Managing Members

Once a user has accepted an invite, the **Organization** > **Members** tab offers you these options to modify their membership in your Organization:

- **Edit**: Switch this Member to a different [Role](#member-roles). (The **Edit** option is displayed only if you have an Enterprise plan.)

- **Remove**: Remove this Member from your Organization. After clicking this button, you'll see a confirmation dialog. (Proceeding will not affect this user's access to any other Cribl.Cloud Organizations they might own or be Members of.)

## Cloud Pricing {#pricing}

Beyond the [free tier](#free), an optional paid Cribl.Cloud account – whether Standard or Enterprise – offers direct support, plus expanded daily data throughput according to your needs. At the top of your Cribl.Cloud portal, select **Go Enterprise** to submit an inquiry about upgrading your free account, and Cribl will respond.

You'll pay only for what you use – the data you send to Cribl Edge, and the data sent to external destinations. However, data sent to your AWS S3 storage is always free. For details, see [Pricing](https://cribl.io/pricing/).

## Differences from Self-Hosted Cribl Edge {#diff}

A Cribl.Cloud deployment can differ from an on-prem/customer-managed Cribl Edge deployment in the following ways. Keep in mind all these differences as you navigate Cribl Edge's current UI, in-app help (including tooltips), and documentation. 

#### Simplified Administration {#admin}

Cribl.Cloud has been designed with options to accommodate everyone – from first-time evaluators, to [Enterprise](#enterprise) customers managing a worldwide network of private-cloud, public-cloud, and/or data-center deployments.

Cribl.Cloud's free offering is designed to help you launch Cribl Edge – and to start processing data – as quickly and easily as possible. Cribl manages many features on your behalf, allowing for a streamlined Settings left nav.

![Cribl.Cloud Leader's Global Settings navigation](cloud-settings.3ae1b5461d.png)
{scale="30%" border="true"}

Below are the key options streamlined out of the free Cloud offering. Bear in mind that upgrading to an [Enterprise plan](#enterprise) will make many of these options configurable:

#### Simplified Distributed Architecture {#distributed}

Cribl.Cloud is preconfigured as a distributed deployment for [Cribl Stream](/stream/deploy-distributed) or [Cribl Edge](/edge/fleet-management). With a Free or Standard plan, allows only a single Fleet. 

Compared to self-hosted Cribl Edge, the **Settings** > **Worker Processes** and **Settings** > **Distributed Settings** links are omitted.

With an Enterprise plan, Cribl always provides at least two Workers, and will scale up further Workers as needed to meet your peak load. With an Enterprise plan, you also have the option to configure additional [hybrid](#hybrid) Edge Nodes and Fleets.

#### Git Preconfigured

Without an Enterprise plan, the **Settings** > **Global Settings** > **System** > **Git Settings** section is omitted. A local `git` client is preconfigured in your Cribl.Cloud portal. On Cribl.Cloud's top nav, use the **Global Config** link (branched icon) to commit/push changes to `git`. Select **Deploy** to deploy your committed changes. Cribl.Cloud does not support Git remote repos. 

#### Automatic Restarts and Upgrades

Without an Enterprise plan, the **Settings** > **Controls** and **Settings** > **Upgrade** links are omitted. Cribl handles restarts and version upgrades automatically on your behalf.

#### Simplified Access Management and Security {#access}

In Cribl.Cloud, you can manage access control for your Organization by clicking **Account** > **Organization** and selecting the [**Members** tab](#multi-user). The options on this tab will vary depending on your plan.

If you have a Cribl.Cloud Enterprise plan, you can use the Key Management Service (KMS), which maintains the keys Cribl Edge uses to encrypt secrets on Fleets and Edge Nodes. Go to **Settings** > **Security** > **KMS** to configure KMS. 

If you add an Enterprise Plan, Cloud and [hybrid](#hybrid) Leaders support Local and Google SSO [authentication](/stream/authentication), along with OpenID Connect (OIDC) and SAML [federated authentication](cloud-sso). Cribl.Cloud does not currently support LDAP.

Role-based access control (RBAC) is simplified in Cribl.Cloud. For details, see [Member Roles](#member-roles).

#### Transparent Licensing

The top nav's **Settings** > **Global Settings** > **Licensing** link is omitted. Your license is managed by your parent Cribl.Cloud portal, where you can check credits and usage history on the [Billing tab](#billing-tab).

#### Other Simplified Settings

Cribl is gradually narrowing the limitations listed in this section, as Cribl.Cloud gains feature parity with on-prem deployments:

- The Script Collector is available only on [hybrid](#hybrid), customer-managed Workers. (This feature is currently not available on Cribl-managed Workers.)
- The System State Source is unavailable on Cribl-managed Workers. It is available on Cribl-managed Edge Nodes, however.
- The AppScope Source's **Filter Settings** are unavailable on Cribl-managed Workers. It is available on Cribl-managed Edge Nodes, however. 
- The top nav's **Settings** > **Global Settings** > **Scripts** link is omitted from Cribl.Cloud, which currently does not support configuring or running shell scripts on hybrid or Cribl-managed Edge Nodes.
- The File System Collector and Filesystem Destination are available only on hybrid Edge Nodes.
- [Persistent Queues](persistent-queues#configuring) can be configured on both hybrid and Cribl-managed Edge Nodes, with an [Enterprise plan](deploy-cloud#enterprise). On hybrid Edge Nodes, you can freely define the **Max queue size**, based on the disk space you provision. On Cribl‑managed Edge Nodes, each Source or Destination's queue is allocated a maximum of 1 GB disk space per Worker Process. (Given this automatic configuration, Cribl-managed Sources and Destinations expose only limited PQ controls.)
- [File-based Destinations](destinations#non-streaming-destinations) support staging directories only on hybrid (not Cribl-managed) Edge Nodes.
- The Tee Function is available only on hybrid (not Cribl-managed) Edge Nodes.

#### Support Options

At **Settings** > **Diagnostics**, you can generate diagnostic bundles and send them directly to Cribl Support. Currently, you cannot download diags. For all support options, see [Get Product Help](/support/).

#### Available Ports and TLS Configurations {#ports-certs}

To get data into Cribl.Cloud, your Cribl.Cloud portal provides several Sources and ports already enabled for you, plus 11 additional TCP ports (`20000`-`20010`) that you can use to add and configure more Cribl Edge [Sources](sources). 

The Cribl.Cloud portal's **Data Sources** tab displays the pre‑enabled Sources, their endpoints, the reserved and available ports, and protocol details. For each existing Source listed here, Cribl recommends using the preconfigured endpoint and port to send data into Cribl Edge.

![Available ports and TLS certificates](cloud-ports-TLS-certs.aeb0247cfd.png)
{border="true"}

##### TLS Details {#tls}

TLS encryption is pre-enabled for you on several Sources, also indicated on the Cribl.Cloud portal's **Data Sources** tab. All TLS is terminated by individual Nodes. 

To enable TLS settings for additional Sources, use these configuration settings:
- **Private key path:** `/opt/criblcerts/criblcloud.key`
- **CA certificate path**: `/opt/criblcerts/criblcloud.crt`
- **Minimum TLS version**: `TLSv1.2`

Currently, Cribl.Cloud does not enable you to import your own certificates for mutual TLS authentication. Cribl.Cloud uses TLS to provide encryption in the wire, but leaves authentication at the protocol layer – e.g., Splunk HEC or S2S tokens, Kafka authorization, etc.



> ##### Cribl HTTP and Cribl TCP Sources/Destinations {{< id `free-ingress` >}}
>
> Use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http), and/or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp), to relay data between Edge Nodes connected to the same Leader. This traffic does not count against your ingestion quota, so this routing prevents double-billing. (For related details, see [Exemptions from License Quotas](licensing#exemptions).)
>
{.box .success}

#### Simplified Source, Collector, and Destination Configuration

Several commonly used Sources are preconfigured for you within Cribl.Cloud's UI, and are ready to use.

The [Exec Source](sources-exec) is unavailable on Cribl-managed Workers, but is available on [hybrid Workers](#hybrid).

The [Cribl Internal](sources-cribl-internal) Source's CriblLogs option is unavailable in Cribl-managed Stream instances, but it is available in Cribl Edge, and in hybrid Workers' Stream instances. The Cribl Internal > CriblMetrics option is available in all of the above combinations. 



> In a preconfigured Source's configuration, never change the **Address** field, even though the UI shows an editable field. If you change these fields' value, the Source will not work as expected.
> 
> After you create a Source and deploy the changes, it can take a few minutes for the Source to become available in Cribl.Cloud's load balancer. However, Cribl Edge will open the port, and will be able to receive data, immediately.
>
{.box .warning}

## Enterprise Cloud {#enterprise}

With a Cribl.Cloud Enterprise plan, you have the same options and flexibility as with an Enterprise license for an on-prem Cribl Edge distributed deployment – and more. (See [Pricing](https://cribl.io/pricing/) for comparisons between Cloud plans and on-prem licenses.)

These options include configuring and managing multiple [Worker Groups](/stream/deploy-distributed#worker-groups) or [Fleets](/edge/fleet-management), [Notifications](notifications), Google SSO authentication, and [Role](roles)-based access control to Cribl Edge resources.

> For other Enterprise features, see [Pricing](https://cribl.io/pricing/).
>
{.box .info}

Cribl.Cloud Enterprise also adds: 

- Full control of [Member Roles](#member-roles) on your Cribl.Cloud Organization.
- The hybrid deployment option, described just below.
- The Leader resides in Cribl.Cloud, with access to diverse Worker deployments. Cribl manages the Leader's availability.

### Hybrid Deployment {#hybrid}

The diagrams below show the comparative flexibility of a Cribl.Cloud hybrid deployment. The Leader (control plane) resides in Cribl.Cloud, while the Workers that process the data can be in any combination of the following environments: 

- In Cribl.Cloud, managed by Cribl. 
- In public or private cloud instances that you manage.
- On-prem in your data centers.

![Enterprise hybrid deployment, with control plane and Cribl-managed Workers in Cribl.Cloud](se-cloud-hybrid-arch-edge.cf56e2621f.png)
{scale="70%" border="true"}

![Enterprise hybrid deployment, with only control plane in Cribl.Cloud](hybrid-cloud-headless.00640a45ba.png)

As the footprint of your operations grows or changes, this flexibility makes it easy to reconfigure Cribl Edge in tandem. You can rapidly expand Cribl Edge observability into new cloud regions – and replace monitored hardware data centers with cloud instances – all while maintaining one centralized point of control. 

You can also add Workers or Edge Nodes, and reassign them to different Fleets, by easily auto-generating [stream](/stream/deploy-workers#ui) or [Edge](/edge/managing-edge-nodes#bootstrap) command-line scripts within Cribl Edge's UI.

### Hybrid Requirements {#hybrid-req}

A hybrid deployment imposes these configuration requirements:
- Hybrid Workers (meaning, Workers that you deploy on-prem, or in cloud instances that you yourself manage) must be assigned to a different Fleet) than the Cribl-managed `default` Group – which can contain its own Edge Nodes.
- All Edge Nodes' hosts must allow outbound communication to the Cribl.Cloud Leader's port 4200 at `https://main-<Organization-name>.cribl.cloud:4200`, to enable configuration and workload management by the Leader.
- On all Edge Nodes' hosts, firewalls must allow outbound communication on port 443 to the Leader, and on port 443 to `https://cdn.cribl.io`.
- All Edge Nodes require connectivity to `https://cdn.cribl.io/telemetry/`. For details on testing this connectivity, on the metadata transmitted to Cribl, and on how we use that data, see [Telemetry Data](licensing#telemetry-data).
- If this traffic must go through a proxy, see [System Proxy Configuration](proxy-config) for configuration details.
- To verify your Leader's Region and public URL, open the [Access Details](#overview) modal.

Note that you are responsible for data encryption and other security measures on Edge Node instances that you manage.

### Adding (Bootstrapping) Workers

To add Workers to your Cloud hybrid deployment, Cribl recommends that you use the script outlined in [Bootstrap Workers from Leader](/stream/deploy-workers#add-bootstrap). Hosts for the new Workers must open the same ports (4200 and 443) listed in [Hybrid Requirements](#hybrid-req).

You have three options for generating the script, outlined in these subsections of the Bootstrap topic linked above:
- Auto-generate it from the [Leader's UI](/stream/deploy-workers#ui).
- Make a `GET` [API request](/stream/deploy-workers#api-spec) to the Leader.
- [curl](/stream/deploy-workers#curl) the same API request.

> In Cribl Edge, you access all these bootstrap options via the [Manage Edge Nodes](/edge/managing-edge-nodes#workers-tab) page's **Add/Update Edge Node** control.
>
{.box .success}

### Hybrid Cribl HTTP/​Cribl TCP Configuration {#hybrid-pairs}

If you use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http) pair, or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp) pair, to relay data between Edge Nodes connected to the same Leader, configuring hybrid Workers demands particular care:
- The Edge Nodes that host each pair's Destination and Source must specify exactly the same Leader Address. Otherwise, token verification will fail – breaking the connection, and preventing data flow.
- Configure hybrid Workers by logging directly into their UI, then selecting **Settings** > **Global Settings** > **Distributed Settings**. Make sure the **Mode** is set to **Managed Worker** or **Managed Edge** (which might require a restart). 
- Then select the **Leader Settings** left tab, and ensure a consistent entry in the **Address** field.
- In Cloud hybrid deployments, the Leader's Address format is `main‑<your‑Org‑ID>.cribl.cloud`. When configuring a hybrid Worker, use that format in the **Address** field.
