# Cribl.Cloud Billing


In the Cribl.Cloud portal, you can check the plan of your Cribl.Cloud Organization,
track data usage, and preview and download invoices.

For more information about Cribl.Cloud billing and pricing, 
see our [Cribl Pricing](https://cribl.io/pricing/) page.

## Check Cribl.Cloud Plan and Credits

To check the plan that your Organization is running:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Plan and Invoices**.

Here you can see a bar of purchased, used, and available **Credits** on your account.
Color-coding breaks down usage by infrastructure versus processing (data throughput).

![Credit usage bar](billing-plan-bar-4.9.png)
{border="true"}

Under **Plan**, you can see the details if your Cribl.Cloud plan, including its period and start date.

## Check Data History

**Monthly Usage History** offer details about your credits usage in the current and prior months.
Here, you can break out usage on the following:

- [Cribl Search](/search/about) 
- [Cribl Lake](/lake/about)
- Hybrid Workers (ingest billing only)
- Groups of Cribl-managed Workers in Cribl.Cloud (with ingest versus infrastructure breakouts per Group)
- Connected Environments GB in (on-prem Leaders passing usage data to Cribl.Cloud). See the
  article on [Connect On-Prem Leaders to Cribl.Cloud Billing](cloud-connections) for more information.

![Plan and Invoices](se-cloud-billing-credits-4.9.1.png)
{border="true"}

To check detailed information about data usage:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Usage**.

Select a product to view its graphs (including any Connected Deployments),
and adjust the duration of time for which data was processed
over a selectable trailing period of 7 days to 1 year.
For more information on Connected Deployments, see [Connecting On-Prem Leaders to Cribl.Cloud Billing](cloud-connections).

| Stream | Search | Lake | Edge |
|--------|--------|------|------|
|<div style="width: 250px">The **Stream** graph shows credits usage for **Data in**, **Data out**, and **Infrastructure**. Using the **Processed Data** drop-down, you can filter the aggregate display down to individual Cribl-managed Groups in Cribl.Cloud, or to all customer-managed (hybrid) Groups.</div> | The **Search** graph shows billed **Search Compute** in CPU hours and **Total Compute Costs**.| The **Lake** graph shows **Total Usage** and **Data Usage**.| The **Edge** graph shows **Data Ingest** and **Data Egress**.|

- The trend line shows daily averages, and you can hover over data points to pop
  out details.
- At the right side of each graph is a total for the selected period. 

![Billing > Usage tab](se-cloud-billing-metrics-4.9.png)
{border="true"}

## Download Invoices

You can download your monthly Cribl.Cloud invoices as CSV or JSON files.
Then, use them in external tools to help with budget planning and analytics or store them for historical purposes.

You can only download finalized invoices, not draft ones.

To download an invoice:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Plan and Invoices**.
1. Under **Monthly Usage History**, hover over **•••** in the **Actions** column of the invoice you want to download.
1. Select **Download CSV** or **Download JSON**.
