# Managing Cribl.Cloud Worker Groups


Using Cribl.Cloud's in-app controls, you [provision](#provisioning-and-managing-infrastructure) Cribl-managed ("Cloud") Worker Nodes to match the throughput capacity you need. With an [Enterprise plan](cloud-enterprise), you have the option to add and configure multiple Cribl-managed and/or hybrid (customer-managed) Groups.

## Create a Cloud Worker Group

All Cribl.Cloud Organizations start out with one Worker Group. To create additional Cribl-managed Worker Groups (with an Enterprise plan):

1. On you Cribl Stream front page, select **Add Group**.
1. In the **New Group** modal, switch **Worker Group type** to `Cloud`.
1. Enter a **Group name**.
2. Select **Enable teleporting to Workers** if you want to enable authenticated access to the Worker Node's UI [from the Leader](deploy-distributed#worker-access).
3. Select **Provision infrastructure** if you want Cribl.Cloud to automatically create Worker Nodes.
4. The **URL/ARN name mapping** is an auto-generated, unique identifier for your Cloud Worker Groups. It follows stricter naming rules than the Group name.
5. Optionally, enter a **Description**. 
6. You can also add **Tags** to help organize Worker Groups into logical categories. Then, you can search by tags on the **View all Groups** interface. 
7. Use the **Sizing calculator** to determine the recommended **Worker Group Size**. For details, see [Using the Sizing Calculator](#calc)
8. Select the **Worker Group Size** based on the recommendation. This setting controls the number of Worker Nodes that will be [provisioned](#provisioning-and-managing-infrastructure), assuming a 1:2 ingest:egress ratio. 
9. Confirm with **Save**.

> For general guidance about capacity required for your deployment, see our [Planning and Sizing](deploy-planning) topics.
>
{.box .success}

![Adding and sizing a new Cloud Group](st-manage-groups-cloud.png)
{scale="60%" border="true"}

### Using the Sizing Calculator {#calc}

The Sizing calculator is an in-built feature within Cribl Stream Cloud that aids users in determining the appropriate size for their Worker Groups, either during creation or resizing processes. This tool helps you make informed decisions based on your specific data volume and processing needs.

When creating a new Worker Group, the **Sizing calculator** section is within the Worker Group configuration modal. When resizing an existing Worker Group, navigate to the **Group Settings** > **Group Configuration** section for the desired Worker Group.

To generate the recommended sizing:

1. Use the slider to enter the anticipated **Daily inbound data volume**  in `TB`. 
1. Use the slider to enter the anticipated **Daily outbound data volume** in `TB`.
1. Select your **Processing load** based on data transformation complexity. **Low** for simple tasks like [Masking](mask-function) or [Renaming](rename-function), **Medium** for functions like [Rollups](rollup-metrics-function) or [Aggregations](aggregations-function), and reserve **High** for resource-intensive operations like [Regex extraction](regex-extract-function) or custom code.
2. Click **Recommend Sizing**. The tool will highlight a recommended **Worker Group Size** tier. You can choose a different tier from the available options if your needs differ slightly from the recommendation. 
   
### Provisioning and Managing Infrastructure

If you set **Provision Infrastructure** to **Yes**, the infrastructure will be provisioned about 30 minutes later.
You will receive an in-portal notification confirming the change.

You can change Worker Group configuration later, including modifying the **Worker Group Size**,
which may change the number of provisioned Worker Nodes.

### Controlling Costs {#cloud-costs}

Cribl will draw down your Cribl.Cloud credits to cover the base cost of maintaining the infrastructure,
as well as your normal cost for ingested data.

You can minimize the costs by setting **Provision infrastructure** to `No`.
You can do so either when creating the Worker Group, or at any later time when you are ingesting no data.
With this setting, the Worker Group will go dormant, and your base infrastructure cost for this Worker Group will be `0`.
In this state, the Worker Group will contain no Worker Nodes and will ingest no data.
The configuration is retained, allowing you to easily reprovision it when needed.
