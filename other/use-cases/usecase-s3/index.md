# Use Cribl Search to Explore Data in an Amazon S3 Bucket


You can easily connect Cribl Search to AWS and start querying your Amazon S3 buckets.

## Connect to AWS

First, you need to grant Cribl Search access to your AWS data and decide where and what you plan to search. You’ll probably need to configure this only once:

1. Add an Amazon S3 Dataset Provider. This is how you tell Cribl Search where to look for data, and provide it with access credentials.<br>Read more: [Add an Amazon S3 Dataset Provider](/search/set-up-s3/#provider)
1. Add an Amazon S3 Dataset. This is how you point Cribl Search at the specific data you want to query within your S3 bucket.<br>Read more: [Add an Amazon S3 Dataset](/search/set-up-s3/#dataset)

Now you’re ready to start searching!

## Search an S3 Bucket

1. Get familiar with the main Search page.<br>Read more: [Search Page Overview](/search/search-overview/)
1. Write your first query, and play around with it. For example:

    ```
    dataset="amazon_s3_dataset_id" source=*vpc*
     | limit 100
    ```
    Read more: [Write Your First Query](/search/your-first-query/) and [Build a Search](/search/build-a-search/)
1. Leverage the Cribl Search query language to construct more complex searches matching your needs. See [Searching Amazon S3](/search/search-s3/) for inspiration, and consult the [Language Reference](/search/operators/) for details on specific query language elements.

Now you’re ready to [save](/search/search-overview/#saved) and [schedule](/search/scheduled-searches/) your searches, set up [alerts](/search/notifications/), [visualize](/search/charting/) the results, and much more.

Remember to observe and plan for any [data transfer charges](/search/set-up-s3/#charges) associated with AWS S3, especially if your S3 bucket and Cribl.Cloud Workspace are in different Regions.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}