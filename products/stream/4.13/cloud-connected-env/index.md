# Learn About Connected Environments


If you have one or more on-prem Cribl Stream environments and Cribl.Cloud
Organization(s), you can configure **Connected Environments**. 

Connected Environments enable you to:



- Take advantage of Cribl.Cloud: infrastructure management capabilities, the
  credit-based billing model, as well as Cribl Lake and Cribl Search solutions.
  For example, you have on-prem environments but you want to use the credit
  billing model, and you want to send Cribl metric data to Cribl Lake/Search.
  From there, you can build dashboards to monitor Cribl throughput and
  performance. 

>For full access to Connected Environments, upgrade Cribl Stream to 4.8.2 or newer.
{.box .warning}

Accomplish this by creating a connection between the on-prem Leader
and the Cribl.Cloud Leader. Then, use the **Connections** page in your
Cribl.Cloud account to manage credit consumption for all of your on-prem
environments from one Cribl.Cloud interface. This feature is called Universal
Subscription.

This section provides instructions for connecting on-prem Cribl Stream Leaders to
Cribl.Cloud.

## Before You Begin Connecting On-Prem Leaders
There are a few tasks you must complete before you can start connecting your
on-prem Leader to Cribl.Cloud:

1. Create a Cribl.Cloud account.

   To set up connected environments, you must have an Enterprise
   [Cribl.Cloud](https://cribl.io/pricing/plan/?product=cloud) account.

2. Decide how you want to connect to the Leader.

   There are two methods you can use to connect an on-prem Leader to a Cribl.Cloud Leader:

   - From the Cribl.Cloud account in **Connections** > **Connected Environments**.
   - From the on-prem Leader in **Distributed Settings** > **Cloud Connection**.

3. Purchase a Cribl.Cloud Enterprise Plan with credits. 

      > Credits serve as the virtual transaction currency in Cribl’s product
      > suite. Each Cribl product has a predefined usage rate, and as you use the
      > products, credits are deducted from your initial pool.
      {.box .success}

### Configuration Considerations

Take these considerations into account for your on-prem Leader, allowlists, security
groups, and Workspaces:

- You must be on Cribl Stream version 4.8.2 or newer.
- **Ports**: Make sure that port `4200` is open on the Leader, with TCP protocol. See [Leader Ports](ports#leader) for more
  information.
- **Proxy**: Connected Environments doesn't support proxied connections.
- **TLS**: The connection between the Cribl.Cloud Leader and the on-prem Leader
  must be encrypted using TLS. When you configure the 
  `CRIBL_CLOUD_WORKSPACE_URL` [environment variable](#use-an-environment-variable), 
  it will start with `tls://`, which enforces TLS.
- **Static IPs**: For IP address allowlist or security group rules, you can find
  the static IPs of your Cribl.Cloud Leader in **Workspace** > **Access** > **Leader NLB IPs**. You 
  can also use the `nslookup` command to obtain static IPs. 
- **Workspaces**: Workspaces are separate infrastructure. Only one on-prem Leader
  can connect to one Cribl.Cloud Leader per Workspace. For allowlists and security groups, ensure you add
  each IP address individually as each Workspace has a unique IP.


