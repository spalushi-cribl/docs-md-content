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

## Access Your Billing Information

The **Billing & Usage** page is a single source of truth for 
your Cribl.Cloud Organization billing and usage data. 

1. Log in to Cribl.Cloud.
1. In the sidebar, select **Organization**, then **Billing & Usage**.

    ![Organization sidebar menu](org-billing-usage.png)
    {border="true"}

On the resulting page, you can:

- Understand your billing data at-a-glance, including total credits consumption
  and a breakdown of monthly usage.
- Access, download, and export your invoices in CSV and JSON format.   
  
>If you don't see **Organization** in the sidebar, select the Organization icon ![](icon-organization.png)
>from the top navigation bar and choose your Organization.
{.box .success}

## Understand Your Monthly Invoice

Your monthly invoice offers details about the usage and costs you’ve incurred in
the current and previous months for your products and [Connected Environments](cloud-connections),
along with a cost item breakdown for more granularity.

![Monthly invoice information](cloud-billing-invoices.png)
{border="true" scale="75%"}

To view your monthly Cribl.Cloud invoice, make sure you’re logged in to your
Cribl.Cloud account and you’ve selected your Organization. 

1. From the sidebar, select **Billing & Usage**.
1. The **Invoices** tab is where you’ll find your monthly invoices. 

    - Each invoice is ordered by month, most recent to oldest.
    - The invoice status will be **Draft** for the current month until your
      billing period ends, as defined in your contract.
    - You can download **Final** invoices as JSON or CSV using the ![](actions-dropdown.light.svg) menu.

1. To view the details of each monthly invoice, select the plus [+] icon. 

Here, you’ll see the breakdown of credits consumed by product and infrastructure: 

|<div style="width: 100px">Product</div>|Cost item|
|-------|-------------------|
|Cribl Stream|<ul><li>Cloud Worker Group Net Data Ingest</li><li>Hybrid Workers GBs Received</li><li>Connected Stream GB In (on-prem Leaders that are passing usage data to Cribl.Cloud)</li></ul>|
|Cribl Edge|<ul><li>Edge Node GBs Ingress</li><li>Connected Edge GBs In</li></ul>|
|Cribl Search|<ul><li>Search Subscription (flat-rate pricing)</li><li>Search Total Compute (CPU x Hours)</li></ul>|
|Cribl Lake|Cribl Lake Total Storage (dataset size volume per GB on an assumed 30 day data retention schedule)|

### Download Invoices

You can download your monthly Cribl.Cloud invoices as CSV or JSON files.
Then, use them in external tools to help with budget planning and analytics or
store them for historical purposes.

>You can only download invoices with a **Final** status. **Draft** invoices can't
>be downloaded.
{.box .success}

To download an invoice:

1. In the sidebar, select **Organization**, then **Billing & Usage**.
1. On the **Billing & Usage** submenu, select **Invoices**.
1. Hover over Actions ![](actions-dropdown.light.svg) for the invoice you want to download.
1. Select **Download CSV** or **Download JSON**.

### View Credits: Consumed, Available, and Average Monthly Consumption

On the **Usage Overview** tab, you can see an overview of your total credit consumption,
including credits and usage from your active Cribl.Cloud plan. 

![Main landing page for Cribl.Cloud Billing & Usage](cloud-billing-usage.png)
{border="true" scale="75%"}

In the **Cumulative Credit Consumed** area, you can hover over a bar to view the
total amount of credits consumed for a given month.

In the **Monthly Usage** area, you'll find a breakdown of credit consumption by
product and infrastructure. 

Finally, in the **Per Product Consumption** area, you can expand a product to
view its monthly usage, along with the top credit consumers for that product
(for example, Worker Groups in Cribl Stream).

![Cribl.Cloud Billing interface showing the per-product consumption breakdown](cloud-billing-per-product.png)
{border="true" scale="75%"}



## Frequently Asked Questions

### How Is Usage Calculated?

For Stream and Edge, your total spend equates to data ingest plus the infrastructure
resources needed for processing your data. We charge on ingress, but you never pay for
egress, so you can send your processed data to as many receivers as you’d like.
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
|Search|Compute pricing: <ul><li>1 Credit per CPU hour</li></ul>If a Search is entirely satisfied by one or more Lakehouses, you are not billed Search CPU hours.|
|Lake|Storage pricing based on 30-day retention: <ul><li>.05 Credits/GB if Cloud Worker managed by Cribl</li><li>.02 Credits/GB if self-managed</li><li>Lakehouse is billed on how long the Lakehouse has been running. The price per month depends on the Lakehouse capacity you purchased.</li></ul>|

### Where Can I View Connected Environment Consumption On My Bill?

Make sure you're logged in to your Cribl.Cloud account.

1. Navigate to **Organization**, then **Billing & Usage**. 
1. Under the **Monthly Usage History** area, expand a finalized invoice, 
   then select **Stream** or **Edge**. 
1. If you have Connected Environments usage, you'll see it listed here as
   **Connected Stream GBs In** or **Connected Edge GBs In**. 

### How Often Is Credit Data Refreshed in Cribl.Cloud?

Cribl.Cloud refreshes credit data every five minutes to provide you with the
most accurate information about your usage and cost.