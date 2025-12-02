# Cribl.Cloud Portal


The Cribl.Cloud portal is the place where you get an overview of and manage your organization,
configure network settings, and control user access and billing information.

For more information about Cribl.Cloud billing and pricing, 
see our [Cribl Pricing](https://cribl.io/pricing/) page.

## Select Organization Page

When you own or are a Member of multiple Cribl.Cloud Organizations, the Organization selection page
– displayed after you first sign in, or sign back in after a logout – enables you to choose which Organization you want to work with. 
You can later switch your organization via the [Account](#account) menu.

![Select Organization interstitial page](select-org-page.0b87ae4c53.png)
{scale="80%"}

Click any tile's `...` button to reveal an options menu. Here you can check who is the owner of the Organization.
You can also click **Leave Organization** if you want to remove yourself as a Member of another Owner's Organization. This option requires confirmation – proceed only if you're sure! (You won't see this button on Organizations that you own.)

## Managing Cribl.Cloud

Once you've registered on the portal, here's how to access Cribl.Cloud:

1. Sign in to your Cribl.Cloud portal page.
1. Select the Organization to work with.
1. From the portal page, select **Manage Stream**, **Manage Edge**, or **Explore [Search]**.
1. The selected application's UI will open in a new tab or window – ready to goat!

Note the **Cribl.Cloud** link at the Cribl.Cloud home page's upper left, under the **Welcome!** message. You can click this link to reopen the Cribl.Cloud [portal page](#workspaces) and all its resources.

## Exploring the Cribl.Cloud Portal

Now that you're here – explore the furniture. The Cribl.Cloud portal's top navigation allows you to navigate among the following pages/links: 

- [Portal](#workspaces) (**Cribl.Cloud** logo)
- [Network Settings](#config)
- [Messages](#messages)
- [Learning](#learning)
- [Software](#download)
- [Account](#account) (including [Organization](#organization) details)

## Portal Page

When you log into the Cribl.Cloud portal, you'll land here. The main events here are the **Manage Stream**, **Manage Edge**, and **Explore [Search]** buttons. Click these to launch (respectively) [Cribl Stream](/stream/), [Cribl Edge](/edge/), or [Cribl Search](/search/) in a new tab.


![Cribl.Cloud portal](cloud-portal-overview-tab.ab1830f4fe.png)
{border="true"}

However, the surrounding page offers lots more useful information:
- On the page body, you'll find links to multiple Cribl resources – documentation, support (Community Slack and bug reporting), free Sandbox training, and blog posts. 
- In the [Overview strip](#overview) just below the top black menu, you'll find detailed configuration information about your Cribl.Cloud Organization. 
- By clicking the top nav's [⚙️ **Network Settings**](#config) link, you can check and manage connectivity details – data Sources, access control, and trust relationships – for your Cribl-managed Cribl.Cloud Workers.

### Overview and Access Details

From left to right, this upper strip displays the following config details: 

**Organization ID**: Domain at which you access the associated Cribl.Cloud Organization.

**Version**: The version of Cribl Stream/Edge applications deployed to your Organization and its Cribl-managed Workers. 

**Region**: The AWS Region where you're running Cribl applications. Cribl.Cloud currently supports the following Regions:

  - **us‑west‑2**
  - **us‑east‑1**
  - **us-east-2**
  - **eu-central-1**
  - **eu-west-2**
  - **ap-southeast-2**
  - **ca-central-1**

**Access Details**: See your Organization's details.

The left column repeats the Organization, region, and version information, with one addition:

**Cribl.Cloud URL**: Static address associated with the load balancer that is in front of the Leader.
Hybrid Worker Nodes will connect to this address on port 4200, while
the Leader UI is served from this address on port 443.

The right column provides a consolidated, read-only display of the following **Stream Worker Group Details**. Some of these options are configured on different tabs, as noted below.

- **Worker Group**: Use this drop-down to select any Group of Cribl-managed Workers that you've configured (including `default`). The remaining fields on the right will display details specific to that Group.

- **Provision Now**: This button will replace all the fields listed below when a Group is dormant. Click the button when you're ready to provision infrastructure for the Group. After a lag, the Group will be ready to process data, and this modal's remaining fields will populate.

![Access Details modal for an unprovisioned Group](cloud-access-details-deprov.a1806470de.png)
{border="true"}

- **Trust**: Role ARN for Workers in this Group. You configure these ARNs on the [Trust tab](#trust-tab).

- **Ingress IPs**: The IPv4 addresses of Worker Nodes' load balancers, also used when receiving data from Push Sources.
These addresses will remain constant, so you can build firewall rules around them. 



- **Public Ingress address**: Each Group's domain for inbound data. This address prepends the Group name to the Organization's global domain name. It does not append ports per data type – you can obtain these from the [Data Sources](#data-sources-tab) tab. 

- **Egress IPs**: Your Cribl.Cloud Organization’s current public IP addresses. These addresses are Group-specific and also dynamic: Cribl will occasionally update them when we need to rescale core infrastructure.
The addresses are used for both outbound connections from the Workers and Pull Sources.

> Egress IPs are not static.
>
> If you need a static egress IP, [contact](/support/) Cribl Support.
{.box .success}

![Access Details modal for a provisioned Group](cloud-access-details.694467c817.png)
{border="true"}

> Configuring Stream Groups (beyond the `default` Group) requires an Enterprise plan. For details about creating and provisioning Groups, see [Cribl.Cloud Worker Groups](/stream/deploy-distributed#cloud-groups).
>
{.box .info}

The **Access Details** modal's left side displays Organization-wide access details, including the **Cribl.Cloud URL** of your Org's Leader/control pane. You'd use this URL for certain API calls and certain [Collection](/stream/collectors) operations coordinated by the Leader. Use the right-side details to configure data flow through individual Groups.

## Network Settings

Clicking the top nav's ⚙️ **Network Settings** link opens a page with connectivity details, spread across three upper tabs: **Data Sources**, **Trust**, and **ACL**.

### Data Sources

The **Data Sources** tab lists ports, protocols, and data ingestion inputs that are open and available to use, including pre-enabled Sources. Use the **Group** drop-down to filter these details per Group of Cribl-managed Workers in the Stream app. Return to this tab to copy Ingest Addresses (endpoints) as needed. For details, see [Available Ports and TLS Configurations](cloud-vs-self-hosted#ports-certs).

For each existing Source listed here, Cribl recommends using the preconfigured endpoint and port to send data into Cribl Stream.

### Trust

The **Trust** tab provides **Worker ARN**s (Amazon Resource Names) that you can copy and paste to attach a Trust Relationship to an AWS account's IAM role. Use the **Group** drop-down to display the ARN for any Group of Cribl-managed Stream Workers. 

Attaching a Trust Relationship enables the `AssumeRole` action, providing cross-account access. For usage details, see the [AWS Cross-Account Data Collection](/stream/usecase-aws-x-account#account-b-configuration) topic's **Account B Configuration** section.

> This option applies only to your Cribl-managed Workers. You cannot use this technique to enable access to hybrid Workers on customer-managed Cribl Stream instances.
>
{.box .warning}

### ACL

This **Access Control List** defines Rules (IPv4 CIDR ranges) to restrict data sent to your data sources. The Rules you define here are global to all your Cribl-managed Groups of Stream Workers.

The default `0.0.0.0/0` rule (modifiable) imposes no limits. Click `+` to add more rules, or click `X` to remove rules. End a rule with `/32` to specify a single IP address, or with `/24` to enable a whole CIDR block from `x.x.x.0` to `x.x.x.255`.

Click **Save** after adding, modifying, or removing rules. Each change takes up to 5 minutes to propagate. Cribl.Cloud will display an `ACL update in progress...` banner, notifying you that rules edits are temporarily disabled to prevent conflicts. A successful update proceeds silently – you will not see a confirmation message.

> The **ACL** options apply only to your Cribl-managed Workers. You cannot use this technique to set access rules on [hybrid Workers](cloud-enterprise#hybrid) running in customer-managed Cribl Stream instances.
>
{.box .warning}

## Messages

Clicking the top nav's **Messages** link opens the **Message Center** right drawer. Here, you will find Cribl.Cloud status and update notifications from Cribl, with **Unread** messages above the **Read** group.

## Learning

Clicking the top nav's **Learning** link opens the **Learning** page, which provides links to everything you need to learn about Cribl Stream in order to goat forth and do great things:

- Sandboxes (free, interactive tutorials on fully hosted integrations).
- Documentation.
- Product and plans overview (pricing comparison).
- Cribl events (including future and archived Webinars).
- Concept/demo videos.

## Software

If you want to try an on-prem installation, this page offers download links for Cribl Stream, Cribl Edge, and [AppScope](https://appscope.dev/) software. You can download either binary installation files or Docker containers (hosting Ubuntu 20.04), to install and manage on your own hardware or virtual machines.

## Account

Manage your Cribl.Cloud account with these submenu items. Hover over **Account** 
to reveal more options: 

- **Profile** allows you to update your personal information.
- **Organization** provides details about the current [organization](#organization) (for an Organization's Owners and Admins only).
- **Organization Selection** submenu (fly-out) works like the
  [Select Organization](#select-org) page – select a link to traverse to other
  Organizations.
- **Log Out** signs you out of the account you're on, and takes you back to the login screen.

### Organization

Displayed to an Organization's Owners and Admins, this page offers information about your
Organization's details, Members, and (where applicable)
billing and SSO, organized into tabs along the top of the page.

Access to the **Billing** tab includes the **Plan & Invoices** and **Usage** left tabs.

#### Details Tab

You can add these optional details to make your Cribl.Cloud deployment more
recognizable than its randomly generated **Organization ID**:

|Field name|Description|
|----------|-----------|
|**Alias** |A "friendly" name for your Organization. Upon signing in, Members will see this alias above the Organization ID on the [Select Organization](#select-organization-page) page.|
|**Description**|Add further details about your Organization.|
|**Opt in to beta features**| If displayed, this toggle enables access to new options that Cribl has not yet made generally available. As with all beta features, expect some instability in exchange for advancing to the cutting edge of your Cribl.Cloud.|

Click **Save** to immediately apply your changes.

#### Members

This tab provides access to [inviting and managing other users](cloud-managing).

#### API Management

This tab provides access to existing API credentials and the ability to [create new ones](/api/#criblcloud).

#### Billing

This tab is displayed only on a paid license plan. It provides [Plan](#plan--invoices) and [Usage](#usage) left tabs:

##### Plan & Invoices

The **Plan & Invoices** left tab displays a mercury bar of purchased, used, and available **Credits** on your account. Color-coding breaks down usage by infrastructure versus processing (data throughput).

Below that is an expandable **Plan** details section. Expandable **Monthly Usage History** rows offer details about your credits usage in the current and prior months. Here, you can break out usage on Cribl Search, on hybrid Workers (ingest billing only), and on Groups of Cribl-managed Workers (with ingest versus infrastructure breakouts per Group).

![Billing > Plan & Invoices tab](se-cloud-billing-credits-4.6.png)
{border="true"}

##### Usage

The **Usage** left tab provides nested tabs for **Stream**, **Search**, **Edge**, and **Lake**. (If you're keeping tabs, this is a third level of selectable tabs.) 

Select a product to view its graphs, and adjust the duration of time for which
data was processed over a selectable trailing period of 7 days to 1 year.

|**Stream**|**Search**|**Lake**|**Edge**|
|----------|----------|--------|--------|
|<div style="width: 250px">The **Stream** tab graphs credits usage for **Data in**, **Data out**, and **Infrastructure**. Using the **Processed Data** drop-down, you can filter the aggregate display down to individual Cribl-managed Groups, or to all hybrid Groups.</div> | The **Search** tab graphs billed **Search Compute** in CPU hours and **Total Compute Costs**.| The **Lake** tab graphs **Total Usage** and **Data Usage**.| The **Edge** tab graphs **Data Ingest** and **Data Egress**.|

The trend line shows daily averages, and you can hover over data points to pop
out details.

At the right side of each graph is a total for the selected period. 

![Billing > Usage tab](se-cloud-billing-metrics-4.6.png)
{border="true"}

#### SSO Tab

This tab appears on an [Enterprise plan](cloud-enterprise), enabling you to configure federated authentication to your Cribl.Cloud Organization from an OIDC or SAML identity provider. For details, see [Cribl.Cloud SSO Setup](cloud-sso).


