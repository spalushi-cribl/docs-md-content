# Manage Cribl.Cloud Worker Groups


Using Cribl.Cloud's in-app controls, you [provision](#provisioning-and-managing-infrastructure) Cribl-managed Worker Nodes in Cribl.Cloud to match the throughput capacity you need. With [certain plan/license tiers](https://cribl.io/pricing/), you have the option to add and configure multiple Cribl-managed and/or customer-managed (hybrid) Groups.

## Cloud Worker Group Providers {#providers}

Cribl-managed Worker Groups in Cribl.Cloud can be based in one of two providers: AWS or Azure.

Select the provider based on, for example, where your primary Sources and Destinations are located.
This will help you reduce transit costs and reduce latency.

The supported regions are:

| AWS Region name       | AWS Region       | Azure Region Name    | Azure Region         |
| --------------------- | ---------------- | -------------------- | -------------------- |
| Australia East        | `ap-southeast-2` | Australia East       | `australiaeast`      |
| Canada Central        | `ca-central-1`   | Canada Central       | `canadacentral`      |
| Germany West Central  | `eu-central-1`   | Germany West Central | `germanywestcentral` |
| UK South              | `eu-west-2`      | UK South             | `uksouth`            |
| US East (N. Virginia) | `us-east-1`      | East US              | `eastus`             |
| US East (Ohio)        | `us-east-2`      | East US 2            | `eastus2`            |
| US West (Oregon)      | `us-west-2`      | West US 2            | `westus2`            |

## Create a Cloud Worker Group

All Cribl Organizations start out with one Worker Group. With an Enterprise plan,
you can create additional Cloud Worker Groups:

1. On the top bar, select **Products**, and then select **Stream**.
1. Select **Add Worker Group**.
1. In the **New Worker Group** modal, in **Worker Group type**, select your preferred provider: `AWS` or `Azure`.

   > You can create up to 10 Cloud Worker Groups per [Workspace](workspaces).
   > If you need to increase this limit, contact Cribl Support.
   {.box .info #group-limit}

1. Select the Azure or AWS **Region** where you want to locate the Worker Group.
1. Enter a **Worker Group name**.
1. Select **Enable teleporting to Workers** if you want to enable authenticated access to the Worker Node's UI [from the Leader](/stream/setting-up-leader-and-worker-nodes#teleporting).
1. Select **Provision infrastructure** if you want Cribl.Cloud to automatically create Worker Nodes.
1. The **URL/ARN name mapping** is an auto-generated, unique identifier for your Cloud Worker Groups. It follows stricter naming rules than the Group name.
1. Optionally, enter a **Description**. 
1. You can also add **Tags** to help organize Worker Groups into logical categories. Then, you can search by tags on the **View all Worker Groups** interface. 
1. Use the **Sizing calculator** to determine the recommended **Worker Group Size**. For details, see [Using the Sizing Calculator](#calc).
1. Select the **Worker Group Size** based on the recommendation. This setting controls the number of Worker Nodes that will be [provisioned](#provisioning-and-managing-infrastructure), assuming a 1:2 ingest:egress ratio. 
1. Confirm with **Save**.

> For general guidance about capacity required for your deployment, see our [Planning and Sizing](deploy-planning) topics.
>
{.box .success}

![Adding and sizing a new Cloud Worker Group](st-manage-groups-cloud-4.9.png)
{scale="60%" border="true"}

### Azure Cloud Workers and Resources in the Same Region

If you are using an Azure-based Cribl-managed Worker Group located in the same Azure region
as another resource (for example, an Azure-based Source or Destination),
the two resources may not be able to communicate.
This may happen if network rules on the Azure resource are enabled
by setting **Public network access** to **Enabled from selected virtual networks and IP addresses**
on Azure portal's **Storage Account** > **Security + networking** page.

This is due to how [intra-region cloud routing](https://learn.microsoft.com/en-us/azure/storage/common/storage-network-security?tabs=azure-portal#grant-access-from-an-internet-ip-range) occurs in Azure over the CSP backbone network leveraging resource private IPs.

You can resolve this by moving the Worker Group to another Azure region.
However, this will incur additional [egress fees](https://azure.microsoft.com/en-us/pricing/details/bandwidth/).

### Use the Sizing Calculator {#calc}

The Sizing calculator is an in-built feature within Cribl Stream Cloud that aids users in determining the appropriate size for their Worker Groups, either during creation or resizing processes. This tool helps you make informed decisions based on your specific data volume and processing needs.

When creating a new Worker Group, the **Sizing calculator** section is within the Worker Group configuration modal. When resizing an existing Worker Group, navigate to the **Worker Group Settings** > **Worker Group Configuration** section for the desired Worker Group.

To generate the recommended sizing:

1. Use the slider to enter the anticipated **Daily inbound data volume**  in `TB`. 
1. Use the slider to enter the anticipated **Daily outbound data volume** in `TB`.
1. Select your **Processing load** based on data transformation complexity. **Low** for simple tasks like [Masking](mask-function) or [Renaming](rename-function), **Medium** for functions like [Rollups](rollup-metrics-function) or [Aggregations](aggregations-function), and reserve **High** for resource-intensive operations like [Regex extraction](regex-extract-function) or custom code.
2. Select **Recommend Sizing**. The tool will highlight a recommended **Worker Group Size** tier. You can choose a different tier from the available options if your needs differ slightly from the recommendation. 
   
### Provision and Manage Infrastructure

If you select **Provision Infrastructure**, the infrastructure will be provisioned about 30 minutes later.
You will receive an in-portal notification confirming the change.

You can change Worker Group configuration later, including modifying the **Worker Group Size**,
which may change the number of provisioned Worker Nodes.

### Control Costs {#cloud-costs}

Cribl will draw down your Cribl.Cloud credits to cover the base cost of maintaining the infrastructure,
as well as your normal cost for ingested data.

You can minimize the costs by clearing **Provision infrastructure**.
You can do so either when creating the Worker Group, or at any later time when you are ingesting no data.
With this setting, the Worker Group will go dormant, and your base infrastructure cost for this Worker Group will be `0`.
In this state, the Worker Group will contain no Worker Nodes and will ingest no data.
> When you deprovision a Worker Group, Stream retains the configuration so you can reprovision it if needed. However, any persistent queue data on the Worker Nodes will be permanently deleted. 
>
{.box .warning}