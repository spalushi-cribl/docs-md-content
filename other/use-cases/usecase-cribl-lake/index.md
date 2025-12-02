# Quickly implement Cribl Lake as Your Out-of-the-Box Data Lake Solution


Cribl Lake offers a data lake solution that is easy to set up, configure, and manage. Cribl Lake provides the following capabilities:

* **Data storage:** Cribl Lake organizes your data into Datasets and stores them efficiently in gzip-compressed JSON or in Parquet format. 
* **Simplified management:** Cribl Lake integrates with Cribl.Cloud to manage underlying infrastructure. This relieves you from setting up and maintaining physical servers or cloud resources for storage.
* **Simplified access control:** Cribl Lake works with the straightforward Cribl.Cloud RBAC model. This streamlines configuring roles and policies.
* **Flexible retention periods:** Cribl Lake enables you to define a separate retention period for each Dataset, ranging from 1 day to 10 years.
* **Flexible data input:** Cribl Stream provides a built-in Cribl Lake Destination, routing data to Cribl Lake from any of the many senders that Cribl Stream supports. Supported senders include Cribl Edge (see [Integrating Cribl Lake with Cribl Edge](/lake/integrating-lake-with-edge/)).
* **Simple data replay:** Cribl Stream provides a built-in Cribl Lake Collector Source that simplifies ingesting or replaying data stored in the lake. From the Collector, you can route data to any of the destinations that Cribl Stream supports.
* **Integrated search:** Cribl Search can directly query the Datasets stored in Cribl Lake. This tight integration means no additional setup to search your stored data.
* **Usage and cost visibility:** Integration with Cribl.Cloud enables you to monitor your storage usage and costs. Flexibly allocate your Cribl.Cloud credits to Cribl Lake storage, with no additional purchase required.

To implement Cribl Lake as a data lake solution:

Cribl Lake is already provisioned for you in your Cribl.Cloud Organization. Select the **Cribl Lake** tile on the home page to configure your data lake.
1. On the Lake Home page, examine the tiles for the built-in Datasets. These are provided to cover the most basic use cases, and have fixed retention periods.
1. Select **Manage** whenever you want to add your own custom Datasets. You can adjust the Datasets retention periods to match your data governance policies. You can also define [accelerated fields](/lake/datasets/#accelerated-fields) that support faster queries. You can further modify your custom Datasets at any later time.
1. On your Cribl.Cloud Organization **Members** tab, authorize other users on Cribl Lake by granting them either the **Read Only** or **Editor** Permission. (As an Organization Owner, you can modify access rights at any time.)
Set up a Cribl Stream Destination and routing to send production and internal health data to Cribl Lake. For details, see [Cribl Lake Destination](/lake/destinations-cribl-lake/). 
1. Optionally, set up replay to Cribl Stream. Replay enables you to access Cribl Lake data for re-analysis, testing, or forwarding to downstream services. For details, see [Cribl Lake Collector](/lake/collectors-cribl-lake/).
1. Optionally, begin querying your lake data in Cribl Search. Cribl Lake Datasets are instantaneously available as Cribl Search Datasets. You can perform ad-hoc and scheduled searches using standard Cribl Search [tools and syntax](/search/your-first-query/).
1. Monitor costs and storage utilization in your Cribl.Cloud Organization **FinOps Center**. See (respectively) the **Invoices** tab and the **Usage Overview** tab.


By following these steps, you can quickly enable a fully functioning data lake with Cribl Lake, while transparently monitoring usage and costs. For further details, see our full [Cribl Lake documentation](/lake/) and our [Charting New Waters with Cribl Lake](https://cribl.io/blog/introducing-cribl-lake/) introductory blog post.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}