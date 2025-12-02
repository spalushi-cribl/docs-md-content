# Understand Your Cribl.Cloud Bill and Plan



In Cribl.Cloud, you can use the **Billing & Usage** page to track your spending and 
credit consumption monthly by product, and download invoices to export them to 
other products for trend analysis. 

Using this guide, you will be able to:

- Explore your credit usage across all Cribl products.
- Get answers to questions you have about your Cribl.Cloud bill.
- Easily comprehend both your total usage and cost for all the Cribl products you own.

For more information about Cribl.Cloud billing and pricing, 
see our [Cribl Pricing](https://cribl.io/pricing/) page.

## Access Your Plan and Billing Information

The **Billing & Usage** page is a single source of truth for 
your Cribl.Cloud Organization billing and usage data. 

1. Log in to Cribl.Cloud.
1. In the sidebar, select **Organization**, then **Billing & Usage**.

    ![Organization sidebar menu](org-billing-usage.png)
    {border="true"}

On this page, you can:

- View your active Cribl.Cloud plan.
- Access, download, and export your invoices in CSV and JSON format.
- Understand your billing data at-a-glance.   
  
>If you don't see **Organization** in the sidebar, select the Organization icon ![](icon-organization.png)
>from the top navigation bar and choose your Organization.
{.box .success}

### View Credits: Purchased, Used, and Available

In the **Credits** area, you can see a progress bar that represents your credits
purchased, used, and available on your account. Color-coding shows you the breakdown
of usage by infrastructure, processing (data throughput), Cribl Search, and Cribl Lake.

![Credit usage bar](billing-plan-bar-4.9.png)
{border="true"}

### View Your Plan

To check the details of the Cribl.Cloud plan that your Organization is running, 
select the caret under **Plan**. The information here includes the plan duration 
period in years and the date it started.


## Understand Your Monthly Invoice

Your monthly invoice offers details about the credits you’ve used in the current 
and previous months, for your products and Connected Environments.

![Monthly invoice information](cloud-invoice.png)
{border="true"}

To view your monthly Cribl.Cloud invoice, make sure you’re logged in to your Cribl.Cloud account
and you’ve selected your Organization. 

1. From the sidebar, select **Billing & Usage**.
1. The **Monthly Usage History** area is where you’ll find your monthly invoices. 

    - Each invoice is ordered by Billing Date, newest to oldest. 
    - The Total Credits Used column is a snapshot of the total credits used by all
      of the products in your plan.
    - You can download an invoice as JSON or CSV from the Actions column.

1. To view a monthly invoice, select the plus [+] icon. 

Here, you’ll see the breakdown of credits consumed by product and infrastructure: 

|<div style="width: 100px">Product</div>|Credit consumers|
|-------|-------------------|
|Cribl Stream|<ul><li>Cribl-managed Workers in Cribl.Cloud</li><li>Hybrid Workers</li><li>Connected Environments GB Received (on-prem Leaders that are passing usage data to Cribl.Cloud)</li></ul>|
|Cribl Edge|Total ingest from Edge Nodes in GB|
|Cribl Search|<ul><li>Search subscriptions (flat-rate pricing)</li><li>Usage-based pricing (CPU x Hours)</li></ul>|
|Cribl Lake|Dataset size volume per GB (on an assumed 30 day data retention schedule)|

### Explore Your Data Usage

To view detailed graphs of data usage by product:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. Select the **Usage** tab.
1. Select a product from the drop-down to view its graphs (including any active [Universal Subscription Connected Environments](cloud-connections)).

![Billing > Usage tab](se-cloud-billing-metrics-4.9.png)
{border="true"}

{{% tabs "stream" "stream" "Stream" "edge" "Edge" "search" "Search" "lake" "Lake"%}}
{{% tab-item "stream" %}}

  The Stream graph shows credits usage for Data in, Data out, and Infrastructure.
  To filter the aggregate display down to
  individual Cribl-managed Groups in Cribl.Cloud, or to all customer-managed (hybrid)
  Groups, use the **Processed Data** drop-down.

{{% /tab-item %}}
{{% tab-item "edge" %}}

   The Edge graph shows Data Ingest and Data Egress.

{{% /tab-item %}}
{{% tab-item "search" %}}

The Search graph shows billed Search Compute in CPU hours and Total Compute Costs.

{{% /tab-item %}}
{{% tab-item "lake" %}}

The Lake graph shows Total Usage and Data Usage.

{{% /tab-item %}}
{{% /tabs %}}

- You can adjust the duration of time for which Cribl processed data over a selectable
  trailing period of 7 days to 1 year.
- The trend line shows daily averages, and you can hover over data points to pop
  out details.
- At the right side of each graph is a total for the selected period.

## Download Invoices

You can download your monthly Cribl.Cloud invoices as CSV or JSON files.
Then, use them in external tools to help with budget planning and analytics or
store them for historical purposes.

>You can only download finalized invoices, not draft ones.
{.box .success}

To download an invoice:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Plan and Invoices**.
1. Under **Monthly Usage History**, hover over **•••** in the **Actions** column of the invoice you want to download.
1. Select **Download CSV** or **Download JSON**.

## Frequently Asked Questions

### How Is Usage Calculated?

For Stream and Edge, your total spend equates to data ingest plus the infrastructure
resources needed for processing your data. We charge on ingress, but you never pay for
egress, so you can send your processed data to as many destinations as you’d like.
For Lake, it equates to the amount of compressed data stored.
And for Search, it equates to the amount of compute resources used to deliver search
results. We charge in compute hours, while most searches take less than a second.

### How Is Cost Calculated?

In Cribl.Cloud, one credit equals one US dollar. Each of our four products 
(Cribl Stream, Cribl Edge, Cribl Lake, Cribl Search) consume credits at a different rate. 
For a detailed explanation, see our [Cribl Pricing Guide](https://cribl.io/pricing/) or
contact Cribl Sales.

Here’s a quick summary of Cribl.Cloud credit consumption:

|<div style="width: 100px">Product</div>|General Calculation|
|-------|-------------------|
|Stream |Ingest pricing (infrastructure pricing based on region): <ul><li>0.32 Credits/GB per Cloud Worker</li><li>0.26 Credits/GB per Hybrid Worker</li></ul>|
|Edge|Ingest pricing: <ul><li>0.21 Credits/GB per Edge Node</li></ul>|
|Search|Compute pricing: <ul><li>1 Credit per CPU*hour</li></ul>|
|Lake|Storage pricing based on 30-day retention: <ul><li>.05 Credits/GB if Cloud Worker managed by Cribl</li><li>.02 Credits/GB if self-managed</li></ul>

### Where Can I View Connected Environment Consumption On My Bill?

Make sure you're logged in to your Cribl.Cloud account.

1. Navigate to **Organization**, then **Billing & Usage**. 
1. Under the **Monthly Usage History** area, expand a finalized invoice, 
   then select **Stream** or **Edge**. 
1. If you have Connected Environments usage, you'll see it listed here as
   **Connected Stream GBs In** or **Connected Edge GBs In**. 