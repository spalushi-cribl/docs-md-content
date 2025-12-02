# Connect On-Prem Leaders to Cribl.Cloud Billing


>For full access to Connected Environments, upgrade Cribl Edge to 4.8.2 or newer.
{.box .warning}

If you have an on-prem Cribl Edge environment and a Cribl.Cloud account on [certain plan/license tiers](https://cribl.io/pricing/), you can use the **Connected Environments** 
option to establish a connection between the on-prem Leader and the Cribl.Cloud Leader. Then, use the **Connections** page in your Cribl.Cloud
account to manage credit consumption for all of your on-prem environments from
one Cribl.Cloud interface. This feature is called Universal Subscription.

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

## Limitations of the Universal Subscription Feature

Connecting environments has some usage limitations:

- The Universal Subscription feature is available on Cribl.Cloud with certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
- Single-instance deployments of Cribl Edge aren't eligible for a Universal Subscription.
- Your Distributed deployment's on-prem Leader must be running Cribl Edge 4.7.x or newer.

## Before You Begin Connecting On-Prem Leaders

There are a few tasks you must complete before you can start connecting your
on-prem Leader to Cribl.Cloud:

### Create a Cribl.Cloud Account

To use Connected Environments, you must have a [Cribl.Cloud](https://cribl.io/cribl-cloud/try-cribl-cloud) 
account on a [certain plan/license tier](https://cribl.io/pricing/). See our documentation on [Initial Cribl.Cloud Setup](cloud-initial-setup).

### Decide How You Want to Connect to the Leader

There are three ways to enable a Cribl.Cloud to on-prem Leader connection:

- From the Cribl.Cloud account in **Connections**, **Connected Environments**.
- From the on-prem Leader in **Distributed Settings**, **Cloud Connection**.
- Using the command line or environment variable, from either the Cribl.Cloud Leader
   or on-prem Leader.

The setup instructions in this article will cover each of the connection options.

### Purchase Cribl.Cloud Enterprise Plan with Credits

Universal Subscription gives you the option to manage an existing on-prem license with Cribl.Cloud credits.

To do this, contact your sales team, who will help you convert your license to credits.

## Add Connected Environments in Cribl.Cloud

To connect existing on-prem Cribl Edge Distributed deployments to Cribl.Cloud, select an on-prem Leader that is running Cribl Edge 4.7.x or newer, then use the following steps.

>If you have multiple environments, such as staging and production enviroments
>when using GitOps, you must connect each environment individually.
{.box .info}

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Workspace**, and then select **Connections**.
1. Select **Configure New Environment** and fill out the following fields:

   |Field|Description|Example|
   |-----|-----------|-------|
   |<div style="width: 150px">Environment name</div>|A name for your environment. This name will display on the **Connected Environments page**. You can search for and filter environments by name.|Goat Farm East Production|
   |Workspace hostname/ID|<div style="width: 150px">Hostname of your Cribl.Cloud Workspace.</div>This field is read-only.|`example.goat-farm-prod.cribl.cloud`|
   |Workspace port number|Port number that the Workspace Leader uses to communicate with the on-prem Leader.|`4200`|
   |Workspace token|The auth token of the Cribl.Cloud Leader. This field is read-only and is automatically populated.|`a12bcDEFgHiJKLmnOpQRstuvw3x4z56`|
   |Script type|The format of the script to connect to the on-prem Leader: <ul><li>Environment variable</li><li>CLI</li></ul>|**Environment variable**: <pre><br>CRIBL_DEPLOYMENT="Goat Farm West Production" <br>CRIBL_CLOUD_WORKSPACE_URL="tls://a12bcDEFgHiJKLmnOpQRstuvw3x4z56@example.goat-farm-prod.cribl.cloud:4200"</pre> **CLI**: <pre><br>./cribl cloud-workspace <br>-d "Goat Farm West Production" <br>-H example.goat-farm-prod.cribl.cloud <br>-p 4200 <br>-u a12bcDEFgHiJKLmnOpQRstuvw3x4z56</pre>|

1. If you're using the CLI option, follow these steps, in order:
    - Click **Copy Script**.
    - Open a command-line window, in the environment whose Leader you want to connect to Cribl.Cloud.
    - `cd` into the `$CRIBL_HOME/bin` subdirectory.
    - Paste the script results onto the command line, and run the script.

1. If you're using the environment variable, copy its
   output, and set it when (or before) you run `cribl start`. How you set this variable depends on your deployment.

 > For example, if your Cribl Edge deployment is running in a container, set the environment variable in the container via run call or configuration.

 > If you're running your own server, you can set environment variables for the `cribl` user, or you can declare the variable along with the `start` command. For details, see our [CLI Reference](cli-reference#cloud-workspace).

## Enable the On-Prem Leader Connection to Cribl.Cloud

You can connect a Cribl Edge Leader to an existing Enterprise Cribl.Cloud Organization.

1. In the sidebar, select **Settings**.
1. Navigate to **Global**, then select **Distributed Settings**.
1. Select **Cloud Connection**.
1. Toggle **Join Cribl.Cloud Workspace** to `Yes`.
1. Fill in the required fields:

   |Field|Description|
   |-----|-----------|
   |**Connection URL**|The **Cribl.Cloud URL** to connect to this Leader. In your Cribl.Cloud Organization, locate this field value on the sidebar's **Workspace** > **Access** tab.|
   |**Connection port**|The port for the Cribl.Cloud Leader. Default is `4200`.|
   |**Connection auth token**|The authentication token for the Cribl.Cloud Leader.|

1. Select **Save**.
1. In the Cribl Subscription Services Agreement modal, select **Agree** to accept the agreement.
1. In the confirmation modal, select **Yes** to save changes and restart.

    >Once you connect an on-prem Leader to a Cribl.Cloud Organization, the license
    >type changes to a Cribl.Cloud billing account and the **Licensing** page will not
    >be accessible. To return to using an on-prem license, toggle **Join Cribl.Cloud
    >Workspace** to `No`, select **Save**, and select **Yes** in the confirmation modal
    > to save changes and restart.
    {.box .info}

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

