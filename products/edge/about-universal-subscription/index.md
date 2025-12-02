# Simplify Billing with Universal Subscription


>This feature requires an Enterprise Plan.
{.box .cloud}

If you have multiple instances of Cribl Edge on-prem, you're likely managing
multiple licenses. Instead of letting license management run your life, you can
take advantage of the [Universal Subscription](https://cribl.io/pricing/plan/?product=universal-subscription) feature. 
Universal Subscription lets you manage an existing on-prem license with Cribl.Cloud credits. 

To get started, connect your on-prem Leader to your Cribl.Cloud Leader to
send billing data payloads. Once your environments are connected, you can use
the [FinOps Center](cloud-billing) to get a unified, all-product view of your Cribl Edge usage
and spending.

This replaces an on-prem license, and gives you the flexibility to maintain
your on-prem deployments without managing multiple licenses and tracking
spend across environments.

> An on-prem Cribl Edge environment consists of at least one Leader Node (the control plane) and one or more Fleets.
{.box .info}

Connecting your on-prem Leader license to Cribl.Cloud billing allows you to:

- Monitor credit consumption for each of your on-prem Cribl Edge environments
  from one user interface.
- Create a billing- and usage-based connection between the Cribl Edge on-prem
  Leader and the Cribl.Cloud Leader, which receives metrics
  from the on-prem Leader. For information about the data payloads, visit [Data Payloads for Connected Environments](cloud-connected-payloads).

## Configure Connected Environments

For the setup procedure, go to [How to Connect On-Prem Leaders to Cribl.Cloud](cloud-connected-env-how-to),
then come back here to explore how to view credit and usage data for your on-prem Connected Environments.

>Full access to Connected Environments requires you to upgrade Cribl Edge to 4.8.2 or newer.
{.box .warning}

## Explore Credit Consumption for Connected Environments

Once you set up a Connected Environment, you can view the incoming usage data
and credit consumption for your on-prem and Cloud deployments of Cribl Edge. You
must be an Organization Owner to view this data.

For more information on Cribl.Cloud billing, go to the [FinOps Center](cloud-billing) page.

### View Usage Data

1. In the sidebar, select **Organization**, then **FinOps Center**.
1. Select **Usage Overview**.
1. Go to **Per Product Consumption** and expand either Cribl Edge or Cribl Stream.
1. Find **Connected < Product > GBs In**.

### View Credit Consumption

1. In the sidebar, select **Organization**, then **FinOps Center**.
1. On the **FinOps Center** submenu, select **Invoices**.
1. Select a month to expand the cost items. Select **Stream** or **Edge**.
1. View **Connected < Product > GBs In** usage and cost.

Here, you will see each on-prem environment that is connected to this Cribl.Cloud
Organization, and the credits each environment is consuming.

## Limitations of the Universal Subscription Feature

Connected Environments has some usage limitations:

- The Connected Environments feature is available only on Enterprise Cribl.Cloud plans. For  Enterprise plan details, see [Pricing](https://cribl.io/pricing/).
- Single-instance deployments of Cribl Edge aren't eligible for a Universal Subscription.
- The Distributed deployment on-prem Leader must be running Cribl Edge 4.8.2 or newer.



