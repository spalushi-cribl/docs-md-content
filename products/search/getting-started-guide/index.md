# Quick Setup


Create a Cribl.Cloud Organization and run your first searches.

---

## Sign Up for Cribl.Cloud

Your first step is to create your Cribl.Cloud Organization.

1. Start at:
   [https://cribl.cloud/signup/](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink).
2. Select the **New User? Free signup** option, and register with your work email address.
3. Use the verification code from Cribl's email to confirm your registration.
4. On the **Create Organization** page, optionally enter an **Organization Name** (a friendly alias for the randomly
   generated ID that Cribl will assign to your Organization).
5. Select an AWS Region to host your Cribl.Cloud Leader. Cribl currently supports the following Regions:

   - `US West (Oregon)`
   - `US East (Virginia)`
   - `US East (Ohio)`
   - `Europe (Frankfurt)`
   - `Europe (London)`
   - `Europe (Zurich)`
   - `Asia Pacific (Sydney)`
   - `Asia Pacific (Singapore)`
   - `Canada (Central)`

   > The Organization region defines where the Leader Node resides.
   >
   > Once you select a region when registering your Organization, you cannot change it.
   > You can, however, create [Cribl-managed Stream Worker Groups](/stream/cloud-workers) in different AWS and Azure regions.
   >
   > In Cribl Search, the Organization region is the physical location where searches are executed.
   > In [Cribl Lake](/lake/), the region defines the location where data is stored.
   {.box .info}

6. Select **Create Organization**.

7. On the resulting page labeled **We're spinning up your Organization...**, complete the quick survey to let us know how you plan to use Cribl products. 
   > The goals that you select here, and the Sources and Destinations that you choose on the next page, will display personalized guidance as soon as you open your Organization. 

8. Select **Complete** to access your new Organization.

9. Select the **Search** tile's **Explore** button to open Cribl Search.
    
   ![Cribl.Cloud landing page](search-cloud-landing.png)
   {scale="100%"}
10. The first time you sign in, you'll see a tile with a goat who can walk you through setting up Cribl Search. If you skip the walkthrough now, you can start it at any later time by opening the Support/Copilot drawer's **Get Help** tab, where you'll find the goat.

![Walkthrough](search-walkthrough-in-menu.png)
{scale="60%"}

## Searching

In this guide, you'll use a built-in sample Dataset named `cribl_search_sample`.

1. From the Cribl Search sidebar, select **Data** to open the default **Datasets** tab. Find the `cribl_search_sample` Dataset within the **Datasets** table.
1. Select the **Search** button on the right side of the `cribl_search_sample` row.
   
      ![Datasets](search-built-in-dataset.png)
   {scale="100%"}
1. **Search Home** opens and runs a basic search on the Dataset.

    

   ![Limit Dataset results](search-basic-dataset-search.png)
   {scale="100%"}

   We've referenced the Dataset with the field `dataset` and its `ID`, in this case, `cribl_search_sample`. The pipe `|`
   character indicates a new process for the query to run. Here, it's added a [`limit`](limit) operator so the search
   only returns up to 1,000 events from the Dataset.

1. Next, let's run an aggregating search – [counting](count) the number of events per `source` (a
   [built-in field](build-a-search#built-in) that contains the name of the data object that included the event).

   ```kusto {runSearch=true}
   dataset="cribl_search_sample"
   | limit 1000
   | summarize count() by source
   ```

    

   ![Count by source](search-aggregate-dataset-search.png)
   {scale="100%"}
   Aggregate results are displayed on the **Chart** tab. Cribl Search comes with rich
   [charting options](charting) out-of-the-box, allowing you to adjust charts as needed.

## Next Steps

Now that you have a Cribl.Cloud Organization and have seen a couple of basic searches, see the following topics:

- [Cribl.Cloud](cloud) for details on management.
- [Cribl Search Tour](search-overview) for an overview of the Cribl Search UI.
- How to [build a search](build-a-search) for details on query writing.
- [Common search examples](common-examples) with common use case examples.

These next topics walk you through searching your own data:

- [Connect to Cribl Edge](set-up-edge) and [Searching Cribl Edge](search-edge) – assumes that you have [Cribl Edge](/edge/) Nodes running.

- [Connect to Amazon S3](set-up-s3) and [Searching Amazon S3](search-s3) – configuring and using an Amazon S3 Dataset Provider and Dataset. (This is a specific example of configuring Cribl Search to query other object stores.)

