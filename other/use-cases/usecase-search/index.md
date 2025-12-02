# Search and Analyze Data Directly at Its Source Using Cribl Search


Cribl Search helps you search, explore, and analyze machine data – such as logs, instrumentation data, application data, and metrics – in place without first moving it to specialized storage. This includes data on Cribl Edge or in a data lake such as Cribl Lake or Amazon S3. This data can reside anywhere, such as in the public or private cloud or on premises.

 Here’s how you can use Cribl Search to interact with your data in place:

**1. Define and manage Datasets:**
* A Dataset is essentially a queryable snapshot of your data. You can create Datasets representing specific sets of data you're interested in. Datasets point to the data's location, whether on Cribl Edge devices or within a data lake.
* To add a new Dataset, select **Data** in the Search navigation pane, select the **Datasets** tab, and then click **Add Dataset**. 
  
[Learn more about Datasets](/search/basic-concepts/#datasets). 

**2. Use Dataset Providers:**
* A Dataset Provider is a definable source of data for one or more Datasets. It enables Cribl Search to connect to your various data sources and tells it where to send the query and interact with the data. 
* To add a new Dataset Provider, select **Data** in the Search navigation pane, select the **Dataset Providers** tab, and then click **Add Provider**. 

[Learn more about Dataset Providers](/search/basic-concepts/#dataset-providers). 

**3: Conduct searches:**
* Searches can range from simple keyword searches to more complex queries using  search functions and operators that fine-tune your results and return only the most relevant data. 
* You can also use natural language to write search queries with the aid of the [Cribl Copilot KQL assistant](/search/copilot-kql). 
* Enter your search queries directly into the box at the top of the Search Home page. To get started, try this search using a built-in [sample Dataset](/search/getting-started-guide/#searching). 

[Try these common search examples](/search/common-examples/). 

**4: Analyze results:**
* Your search results appear in a tabular or raw text format, depending on the context of the search.  
* Use the **Events**, **Fields**, and **Charts** tabs to delve deeper into your data.

[Learn more about search results](/search/search-overview/#search-results).

**5. Visualize search data:**
* Use Cribl Search charting capabilities to visualize your search results, making it easier to understand trends, patterns, and anomalies within your data.
* Choose from various chart types and customize your charts to best display the aggregate search results for more insightful data analysis.

Learn more about [charting](/search/charting/) and [chart types](/search/chart-area/). 

**6. Leverage historical and real-time data:**
* Execute searches on both historical and real-time data as it flows into the system, giving you an up-to-the-minute view of your live data environment.

These steps are your high-level guide to effectively searching and analyzing your data without the need to move it into a centralized system. This direct interaction can lead to faster insights, reduced data transfer and storage costs, and more flexible data management. Read the [Cribl Search documentation](/search/) for in-depth instruction and resources, or try the [Cribl Search Sandbox](https://sandbox.cribl.io/course/overview-search) for hands-on learning. 

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}