# FinOps Center



The Cribl FinOps Center is designed to simplify the process of tracking your
product usage and credit consumption. You can also download
finalized invoices and export them to external spend analysis products.

With the FinOps Center, you can:

- Explore credit usage across all Cribl products, including credit refunds and
  rollovers.
- Narrow down usage to a specific time frame using the custom time picker.
- View contract renewal date against usage.
- Get answers to questions you have about your bill.
- Easily comprehend both your total usage and cost for all the Cribl products
  you own, including Connected Environments and hybrid Fleets.

For more information about Cribl.Cloud billing and pricing, 
see our [Cribl Pricing](https://cribl.io/pricing/) page.

## Access Billing Information {#access-billing}

The **FinOps Center** page is the single source of truth for 
your Cribl.Cloud Organization billing and usage data. 

1. Log in to Cribl.Cloud.
1. In the sidebar, select **Organization**, then **FinOps Center**.

    ![Organization sidebar menu](finops-org.png)
    {border="true" scale="25%"}
  
>If you don't see **Organization** in the sidebar, select the Organization icon ![](icon-organization.png)
>from the top navigation bar and choose your Organization.
{.box .success}

## Explore the FinOps Center {#explore}

Select the **Usage Overview** tab for a holistic view.

![Explore the Cribl FinOps Center](finops-center-explore-2510.png)
{border="true" scale="85%"}

|Callout|Area|Description|
|-------|----|-----------|
|A      |**Total credits consumption**| View an overview of your credits for the current active contract. Expand the arrow to reveal a credit ledger, including any credits that rolled over from your last contract period.| 
|B      |**Cumulative credit consumed**| View a column chart containing your cumulative credit consumption across all products.|
|C      |**Contract renewal indicator**| The vertical grey line indicates the point at which your Cribl contract was renewed. Because credits are consumed on a per-contract basis, you might see a drop in consumed credits directly after the renewal indicator, even if it is mid-month.|
|D      |**Total acquired credits**| The horizontal red line indicates total acquired credits in relation to consumed credits, including refunds and rollovers.|
|E      |**Date range picker**| Use the date range selector to view usage and cumulative credit consumption in a specific date and time period.|
|F      |**Credit usage**| View a column chart containing credit usage by product. Hover over each column to view the number of credits consumed for the time period indicated on the x-axis.|
|G      |**Consumption by product**| View a donut chart of your credit consumption by product for the selected date range, expressed as a percentage of your total credit allotment for the active contract period.|
|H       |**Per product consumption**| Find detailed, per-product credit consumption information in this section. Expand a product name for the single product usage view.| 
|I      |**Sparklines** | View usage changes at-a-glance with the per-product sparkline chart and month-over-month trend.|  

## View Product-Specific Usage and Credit Consumption

To further understand how your Organization is using Cribl and spending credits,
you can select a product or offering from the top menu bar. 

{{% tabs "stream" "stream" "Stream" "edge" "Edge" "search" "Search" "lake" "Lake" "guard" "Guard" %}}
{{% tab-item "stream" %}}
On the Stream tab, you'll find these helpful billing metrics:
- The total credit consumption by Cribl Stream, including the current
  month and total monthly average.
- Credit consumption and 3-month consumption forecasting based on
  the current credit consumption in this Organization.
- Credit consumption by environment type, including hybrid, cloud, and Connected Environments.
- Cribl Stream usage in GB of data ingested, visualized by environment type.
- Cribl Stream usage per Workspace in GB of data ingested.
- The 10 Worker Groups with the most data usage in GB.
{{% /tab-item %}}
{{% tab-item "edge" %}}
On the Edge tab, you'll find:
- The total credit consumption by Cribl Edge, including the current month
  and total monthly average.
- Credit consumption and 3-month consumption forecasting based on
  the current credit consumption in this Organization.
- The amount of dropped bytes against your total ingest. Dropped bytes don't count
  against your credit allowance.
- Cribl Edge usage per Workspace in GB.  
{{% /tab-item %}}
{{% tab-item "search" %}}
On the Search tab, you'll find:
- The total credit consumption of your Cribl Search instance, including the current
  month and total monthly average.
- Credit consumption and 3-month consumption forecasting based on
  the current credit consumption in this Organization.
- Cribl Search usage in CPU hours.
- Cribl Search usage per Workspace in CPU hours.
{{% /tab-item %}}
{{% tab-item "lake" %}}
On the Lake tab, you'll find:
- The total credit consumption of your Cribl Lake instance, including the current
  month and total monthly average.
- Credit consumption and 3-month consumption forecasting based on
  the current credit consumption in this Organization.
- Lakehouse credit consumption and 3-month consumption forecast.
- Cribl Lake usage in GB stored data.
- Cribl Lake usage per Workspace in GB stored.
{{% /tab-item %}}
{{% tab-item "guard" %}}
On the Guard tab, you'll find:
- The total credit consumption of Cribl Guard, including the current
  month and total monthly average.
- Cribl Guard usage in GB of data scanned.
- Cribl Guard usage per Workspace in GB of data scanned.
{{% /tab-item %}}
{{% /tabs %}}

## Understand Your Monthly Invoice {#invoice}

Your invoice offers details about the usage and costs you have incurred in the
current and previous months for your products, [Cribl Guard](/guard/about), and
[Connected Environments](cloud-connections), along with a cost item breakdown
for more granularity.

![Monthly invoice information](finops-invoices.png)
{border="true" scale="75%"}

To view your monthly Cribl invoice, make sure you’re logged in to your
Cribl.Cloud account and you’ve selected your Organization. 

1. From the sidebar, select **FinOps Center**.
1. The **Invoices** tab is where you’ll find your monthly invoices. 

    - Each invoice is ordered by month, most recent to oldest.
    - The invoice status will be **Draft** for the current month until your
      billing period ends, as defined in your contract.
    - You can download **Final** invoices as JSON or CSV using the Actions ![](actions-dropdown.light.svg) menu.

1. To view the details of each monthly invoice, expand the cost item. 

Here, you’ll see the breakdown of credits consumed by product and infrastructure: 

|<div style="width: 100px">Product</div>|Cost item|
|-------|-------------------|
|Cribl Stream|<ul><li>Cloud Worker Group Net Data Ingest</li><li>Hybrid Workers GBs Received</li><li>Connected Stream GB In (on-prem Leaders that are passing usage data to Cribl.Cloud)</li></ul>|
|Cribl Edge|<ul><li>Edge Node GBs Ingress</li><li>Connected Edge GBs In</li></ul>|
|Cribl Search|<ul><li>Search Subscription (flat-rate pricing)</li><li>Search Total Compute (CPU x Hours)</li></ul>|
|Cribl Lake|Cribl Lake Total Storage (dataset size volume per GB on an assumed 30 day data retention schedule)|
|Infrastructure|Cribl.Cloud Worker Group infrastructure usage in hours, per MB/s.|
|Guard|Cribl Guard GB scanned. See [Cribl Guard documentation](/guard/guard-configure#how-youre-billed-for-cribl-guard) for more information.|

### Download Invoices {#download}

You can download your monthly Cribl.Cloud invoices as CSV or JSON files.
Then, use them in external tools to help with budget planning and analytics or
store them for historical purposes.

>You can only download invoices with a **Final** status. **Draft** invoices can't
>be downloaded.
{.box .success}

To download an invoice:

1. In the sidebar, select **Organization**, then **FinOps Center**.
1. Select the **Invoices** tab.
1. Hover over Actions ![](actions-dropdown.light.svg) for the invoice you want to download.
1. Select **Download CSV** or **Download JSON**.

## Frequently Asked Questions {#faq}

### How Is Usage Calculated? {#usage-calculated}

For Cribl Stream and Cribl Edge, your total spend equates to data ingest plus the infrastructure
resources needed for processing your data. We charge on ingress (data in), but you never pay for
egress (data out), so you can send your processed data to as many receivers as you’d like.

For Cribl Lake, usage equates to the amount of compressed data stored.
And for Cribl Search, usage equates to the amount of compute resources used to deliver search
results. We charge in compute hours, and most searches take less than a second.

### How Is Cost Calculated? {#cost-calculated}

In Cribl.Cloud, one credit equals one US dollar. Each of our four products 
(Cribl Stream, Cribl Edge, Cribl Lake, Cribl Search) consume credits at a different rate. 
For a detailed explanation, see our [Cribl Pricing Guide](https://cribl.io/pricing/) or
contact Cribl Sales.

Here’s a quick summary of Cribl.Cloud credit consumption:

|<div style="width: 100px">Product</div>|General Calculation|
|-------|-------------------|
|Stream |Ingest pricing (infrastructure pricing based on region): <ul><li>0.32 Credits/GB per Cloud Worker</li><li>0.26 Credits/GB per Hybrid Worker</li></ul>|
|Edge|Ingest pricing: <ul><li>0.21 Credits/GB per Edge Node</li></ul>|
|Search|Compute pricing: <ul><li>1 Credit per CPU hour</li></ul>If a Search is entirely satisfied by one or more Lakehouses, you are not billed Search CPU hours.|
|Lake|Storage pricing based on 30-day retention: <ul><li>.05 Credits/GB if Cloud Worker managed by Cribl</li><li>.02 Credits/GB if self-managed</li><li>Lakehouse is billed on how long the Lakehouse has been running. The price per month depends on the Lakehouse capacity you purchased.</li></ul>|
|Lakehouse|<ul><li>[Lakehouse billing](https://cribl.io/pricing/lake/?product=lakehouse) begins as soon as Cribl provisions the Lakehouse, shortly after you select **Save**.</li><li>You're billed based on how long the Lakehouse has run.</li><li>Price per month depends on the Lakehouse capacity you purchased.</li></ul>For more details about Lakehouse provisioning, see our [Lakehouse](/lake/lakehouse) docs.|
|Guard|<ul><li>Cribl Guard billing begins as soon as the Cribl Guard Function starts ingesting data in the Pipeline and scanning for sensitive information.</li><li>You're billed credits per GB scanned.</li><li>You can start accruing charges immediately after enabling **Protect** on a Destination from the Cribl Guard homepage.</li></ul> See [Configure Cribl Guard](/guard/guard-configure) for more details.| 

### Where Can I View Connected Environment Consumption On My Bill? {#connected-env}

You can view Connected Environment consumption and usage in a few places. 

First, make sure you're logged in to your Cribl.Cloud account.

View credit cost for Connected Environments:

1. Navigate to **Organization**, then **FinOps Center**. 
1. In the **Month** column, expand a finalized invoice, 
   then select **Stream** or **Edge**. 
1. If you have Connected Environments cost, you'll see it listed here as
   **Connected Stream GBs In** or **Connected Edge GBs In**.

View usage for Connected Environments:

1. Navigate to **Organization**, then **FinOps Center**.
1. Select **Stream** or **Edge** in the top menu.
1. Scroll to **Stream Usage (GB)** or **Edge Usage Per Workspace (GB)**.

### How Often Is Credit Data Refreshed in Cribl.Cloud? {#data-refresh}

Cribl.Cloud refreshes credit data every five minutes to provide you with the
most accurate information about your usage and cost.