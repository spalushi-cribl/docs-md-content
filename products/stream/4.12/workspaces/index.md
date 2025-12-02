# Workspaces


Workspaces offer a multi-tenancy capability, enabling you to create multiple isolated instances in your Cribl.Cloud Organization.
This lets you strengthen your security, and fulfill compliance and isolation requirements.

Each Workspace offers a dedicated virtual private cloud (VPC) that acts as a separate environment within your Organization.
Workspaces do not share product configurations, resources, or data flows, and each has a separate Leader Node.
They do share a layer of access control ([Organization-level Permissions](permissions#organization) and [SSO configuration](sso-cloud)),
as well as API credentials, licenses, and billing.

A centralized interface allows you to manage all your Workspaces
and control access for [Members](members#invite-members) and [Teams](teams) to individual Workspaces.

> Workspaces require Cribl.Cloud with certain plan tiers. For details, see [Pricing](https://cribl.io/pricing/).
{.box .cloud}

An example use case scenario for creating multiple Workspaces
is setting up separate environments for different business units in an enterprise.
Data management, security, development, and any other units can have their own federated Workspaces,
with completely separate members and permissions lists.
This way, they can work in isolation without risk of interference and with increased focus.

## Limitations

You can [create](workspaces-configuring#add-new-workspace) up to 5 Workspaces per Organization.

If you need more Workspaces in your Organization (up to 50),
contact Cribl Support to discuss extension options.

> Workspaces require Cribl.Cloud with certain plan tiers. For details, see [Pricing](https://cribl.io/pricing/).
{.box .cloud}

## View Workspace Details

Workspaces provide a set of information that you can use when configuring data flow through their Worker Nodes.

### View Access Details

To view detailed access information of your Workspace, in the sidebar, select **Workspace** and then **Access**.

This page presents a summary of information about your Cribl.Cloud Organization,
as well as the **Cribl.Cloud URL** for the current Workspace.

#### Cribl.Cloud URL

**Cribl.Cloud URL** is a static address associated with the load balancer that is in front of the Leader.
Customer-managed (hybrid) Worker Nodes will connect to this address on port 4200,
while the Leader UI is served from this address on port 443.

You can use this URL for certain API calls and certain [Collection](/stream/collectors) operations coordinated by the Leader.

#### Static External IPs for the Leader {#leader-nlb-ips}

The **Leader NLB IPs** field in **Workspace** > **Access** lists the IPs for the Leader Network Load Balancers associated with the Workspace.

Typically, only 2 of the 3 addresses will be active (returned by DNS) at any time,
while the inactive address is swapped in during infrastructure maintenance events.

You can add all those IPs to your firewall allowlist to ensure that the load balancers are accessible across your hybrid deployment.

#### Stream Worker Group Details

**Stream Worker Group Details** provides information about Cribl-managed Worker Groups in Cribl.Cloud, if any are provisioned.

If no Worker Group is provisioned, you can select **Provision Now** to provision infrastructure for the Group.
After a lag, the Group will be ready to process data, and this modal's remaining fields will populate.

For a provisioned Worker Group, you see the following information:

| Field | Description |
| ----- | ----------- |
| **Trust** | Role ARN for Workers in this Worker Group. You configure these ARNs on the [Trust tab](#get-arn). |
| **Ingress IPs** | The IPv4 addresses of Worker Groups' load balancers, also used when receiving data from Push Sources. These addresses will remain constant, so you can build firewall rules around them.<br><br>Three Ingress IPs are provided for each Worker Group, one on each Availability Zone, assuming the Group has at least three Workers. In a Worker Group of fewer than three Workers, one of the IPs will be inactive. |
| **Public Ingress address** | Each Group's domain for inbound data. This address prepends the Group name to the Organization's global domain name. It does not append ports per data type – you can obtain these from the [Data Sources](#view-data-sources) tab. |
| **Egress IPs** | Your Cribl.Cloud Organization’s current public IP addresses. These addresses are Group-specific and also dynamic: Cribl will occasionally update them when we need to rescale core infrastructure. |

**Public Ingress address** and **Egress IPs** are used for both outbound connections from the Workers and Pull Sources.

Because individual **Ingress IPs** can become inactive,
Cribl recommends that you send your data to the **Public Ingress address** instead of directly to IPs.

> Egress IPs are not static.
>
> If you need a static egress IP, [contact](/support/) Cribl Support.
{.box .success}

![Access Details modal for a provisioned Group](cloud-access-details-4.9.png)
{scale="60%" border="true"}

> Configuring Stream Groups (beyond the `default` Group) requires certain plan tiers. For details, see [Pricing](https://cribl.io/pricing/).
> For details about creating and provisioning Groups, see [Cribl.Cloud Worker Groups](/stream/cloud-workers).
>
{.box .info}

### View Data Sources

To view detailed access information about a Workspace's Data Sources, in the sidebar, select **Workspace** and then **Data Sources**.

The **Data Sources** page lists ports, protocols, and data ingestion inputs
that are open and available to use, including pre-enabled Sources.
Use the **Group** drop-down to filter these details per Cribl-managed Worker Group in Cribl.Cloud.
For details, see [Available Ports and TLS Configurations](cloud-vs-self-hosted#ports-certs).

For each existing Source listed here, Cribl recommends using the preconfigured endpoint and port to send data into Cribl Stream.

### Get ARN

To get the ARN (Amazon Resource Name) for your Workspace, in the sidebar, select **Workspace** and then **Trust**.

You can copy and paste the **Worker ARN**s listed in the **Trust** page to attach a Trust Relationship to an AWS account's IAM role. Use the **Group** drop-down to display the ARN for any Group of Cribl-managed Stream Worker Groups in Cribl.Cloud. 

Attaching a Trust Relationship enables the `AssumeRole` action, providing cross-account access. For usage details, see the [AWS Cross-Account Data Collection](/stream/usecase-aws-x-account#account-b-configuration) topic's **Account B Configuration** section.

> This option applies only to your Cloud Workers. You cannot use this technique to enable access to customer-managed [hybrid Workers](cloud-enterprise#hybrid).
>
{.box .warning}

### Set Up ACL

To set up Access Control List (ACL) Rules, in the sidebar, select **Workspace** and then **Access Control List**.

ACL Rules (IPv4 CIDR ranges) let you restrict data sent to your data sources.
The Rules you define here are global to all Cribl-managed Worker Groups in Cribl.Cloud in the current Workspace.
You can set up a maximum of 6 ACL Rules.

The default `0.0.0.0/0` rule (modifiable) imposes no limits.
End a rule with `/32` to specify a single IP address, or with `/24` to enable a whole CIDR block from `x.x.x.0` to `x.x.x.255`.

Select **Save** after adding, modifying, or removing rules. Each change takes up to 5 minutes to propagate.
Cribl.Cloud will display a banner, notifying you that rules edits are temporarily disabled to prevent conflicts.
A successful update proceeds silently – you will not see a confirmation message.

> The **ACL** options apply only to Cribl-managed Workers. You cannot use them to set access rules on customer-managed [hybrid Workers](cloud-enterprise#hybrid).
>
{.box .warning}
