# About Universal Subscription


Connecting your on-prem Cribl Edge to Cribl.Cloud simplifies licensing and billing. Universal Subscription lets you manage all on-prem environment costs from a single Cribl.Cloud interface, eliminating the need for separate on-prem licenses. This streamlines administration and provides a unified view of your Cribl Edge usage and spending. For details, see [Connect On-Prem Leaders to Cribl.Cloud](cloud-connected-env).

![The Connected Environments UI](cloud-connected-environments.png)

This replaces the on-prem license, and gives you the flexibility to maintain
your on-prem deployments without managing multiple licenses and tracking
spend across environments.

> An on-prem Cribl Edge environment consists of at least one Leader Node (the control plane) and one or more Fleets.
{.box .info}

Connecting your on-prem Leader to Cribl.Cloud allows you to:

- Monitor credit consumption for each of your on-prem Cribl Edge environments
  from one user interface.
- Create a connection between the Cribl Edge on-prem Leader and the Cribl.Cloud
  Leader, which receives billing and usage metrics from the on-prem Leader.

For details, see [Connecting On-Prem Leaders to Cribl.Cloud](cloud-connected-env).

## Limitations of the Universal Subscription Feature

Connecting environments has some usage limitations:

- The Universal Subscription feature is available on Cribl.Cloud with certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
- Single-instance deployments of Cribl Edge aren't eligible for a Universal Subscription.
- Your Distributed deployment's on-prem Leader must be running Cribl Edge 4.7.x or newer.

## Explore Credit Consumption for Connected Environments

Now that you have a successful connection to your Cribl.Cloud Organization, you
can view the incoming usage data and credit consumption for your on-prem and
Cloud deployments of Cribl Edge. You must be an Organization Owner to view this data.

For more information on Cribl.Cloud billing, go to the [Cloud Billing and Usage](cloud-portal#check-plans-credits-and-usage) page.

### View Usage Data

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Usage**.
1. Select the product drop-down to choose a **Connected Deployment** of Cribl Edge.

On this page, you can view GB in and GB out for the connected environment.

### View Credit Consumption

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Plan and Invoices**.
1. In **Monthly Usage History**, expand the **Billing Date** and scroll to the **Connected Environments** table.

Here, you will see each on-prem environment that is connected to this Cribl.Cloud
Organization, and the credits each environment is consuming.

