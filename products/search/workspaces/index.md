# Workspaces


Use Workspaces to create isolated environments within your Cribl.Cloud Organization.

---

Workspaces offer a multi-tenancy capability, enabling you to create multiple isolated instances in your Cribl.Cloud
Organization. This lets you strengthen your security, and fulfill compliance and isolation requirements.

Each Workspace offers a dedicated virtual private cloud (VPC) that acts as a separate environment within your Organization.
Workspaces do not share product configurations, resources, or data flows, and each has a separate Leader Node.
They do share a layer of access control ([Organization-level Permissions](/stream/permissions#organization) and [SSO configuration](/stream/sso-cloud)),
as well as API credentials, licenses, and billing.

A centralized interface allows you to manage all your Workspaces
and control access for [Members and Teams](members-and-teams) to individual Workspaces.

An example use case scenario for creating multiple Workspaces is setting up separate environments for different business
units in an enterprise. Data management, security, development, and any other units can have their own federated
Workspaces, with completely separate Members and Permissions lists. This way, they can work in isolation without risk of
interference and with increased focus.

## Limitations

You can [create](workspaces-configuring#add-new-workspace) up to five Workspaces per Organization.

If you need more Workspaces in your Organization (up to 50), contact Cribl Support to discuss extension options.


> Workspaces require certain a Cribl.Cloud plan. For details, see [Pricing](https://cribl.io/pricing/).
{.box .info}

## View Workspace Details

Workspaces provide a set of information that you can use when configuring data flow through their Edge Nodes.

### View Access Details

To view detailed access information of your Workspace, in the sidebar, select **Workspace** and then **Access**.

This page presents a summary of information about your Cribl.Cloud Organization,
as well as the **Cribl.Cloud URL** for the current Workspace.

#### Cribl.Cloud URL

**Cribl.Cloud URL** is a static address associated with the load balancer that is in front of the Leader.
Hybrid Stream Workers will connect to this address on port 4200,
while the Leader UI is served from this address on port 443.

You can use this URL for certain API calls and certain [Collection](/stream/collectors) operations coordinated by the Leader.

#### Static External IPs for the Leader {#leader-nlb-ips}

The **Leader NLB IPs** field in **Workspace** > **Access** lists the IPs for the Leader Network Load Balancers associated with the Workspace.

Typically, only 2 of the 3 addresses will be active (returned by DNS) at any time,
while the inactive address is swapped in during infrastructure maintenance events.

You can add all those IPs to your firewall allowlist to ensure that the load balancers are accessible across your hybrid deployment.

### Get ARN

To get the ARN (Amazon Resource Name) for your Workspace, in the sidebar, select **Workspace** and then **Trust**.

You can copy and paste the **Worker ARN**s listed in the **Trust** page to attach a Trust Relationship to an AWS account's IAM role. Use the **Group** drop-down to display the ARN for any Group of Cribl-managed Stream Worker Groups in Cribl.Cloud. 

Attaching a Trust Relationship enables the `AssumeRole` action, providing cross-account access. For usage details, see the [AWS Cross-Account Data Collection](/stream/usecase-aws-x-account#account-b-configuration) topic's **Account B Configuration** section.

> This option applies only to your Cribl-managed Stream Workers in Cribl.Cloud. You cannot use this technique to enable access to customer-managed [hybrid Workers](cloud-enterprise#hybrid).
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

> The **ACL** options apply only to Cribl-managed Stream Workers in Cribl.Cloud. You cannot use them to set access rules on customer-managed [hybrid Workers](cloud-enterprise#hybrid).
>
{.box .warning}
