# Use Cribl to Replay Data from Low-Cost Storage


This high-level guide explores how to use Cribl Stream, Cribl Lake, and Cribl Search to access and analyze data stored in cost-effective locations for investigation, reporting, and auditing.

First, choose your approach:

* **Cribl Stream replay:** Ideal for pre-processed JSON data and advanced manipulation during re-ingestion.
* **Cribl Search:** Suitable for querying and analyzing data directly in low-cost storage without re-ingesting.
* **Cribl Lake with Cribl Stream replay (accelerated):** Combine Cribl Lake's optimized data storage with Cribl Stream's replay capabilities for faster access and manipulation.
  
**Cribl Stream replay:**

* **Choose data format:** Choose JSON (recommended) for its wide compatibility and pre-parsed structure.
* **Use the Replay feature:** Use Collectors (S3, Filesystem) to fetch specific data from storage on-demand.
    * Filter by file path attributes for optimized efficiency.
    * Collectors reintroduce data for processing within Cribl Stream's Pipeline.
* **Route replayed data:** Define routing rules based on data content or metadata to send data to various destinations (SIEM, databases, analytics platforms) for analysis or reporting.

Learn about the basics of [Cribl Stream replay](/stream/usecase-replay-s3/), and then get hands-on practice by trying out the [Full-Fidelity Replay](https://sandbox.cribl.io/course/replay-raw) Sandbox.

**Cribl Search:**

* **Query data:**  With Cribl Search, you can directly query and analyze data stored in cost-effective solutions like Amazon S3 and Cribl Lake (including pre-made Datasets). This eliminates the need for data movement and allows for efficient exploration of historical data organized within Cribl Lake.
* **Investigate and report:** Use search functions and visualizations within Cribl Search to perform investigations and generate reports without re-ingesting data.

For details, see [Searching Cribl Lake](/lake/search-cribl-lake/). Or get hands-on practice with the [Cribl Search](https://sandbox.cribl.io/course/overview-search) Sandbox. 

**Cribl Lake with Cribl Stream replay (accelerated)**

* **Store data in Cribl Lake:** Use Cribl Lake's optimized storage format for faster data retrieval.
* **Use Cribl Stream replay:** Integrate Cribl Stream for advanced data manipulation and routing capabilities during replay.

For details, see [Cribl Lake](/lake/) and [Datasets](/lake/datasets/). For hands-on practice, check out the [Cribl Lake](https://sandbox.cribl.io/course/overview-lake) Sandbox.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}