# Event Processing

The Cribl processing engine is built around the concepts of Pipelines and Functions. A Pipeline defines the strategic path data takes from input to output, while Functions are the operations, like enrichment and transformation, applied to data along that path. 

Understanding their architectural interplay is what allows you to control infrastructure costs, prevent performance bottlenecks, and build a data plane that is scalable, resilient, and maintainable.

## Event Model and Processing Order

Understanding how Cribl Stream and Cribl Edge structures and processes a single event is fundamental to all design decisions. The internal logic is fixed, ensuring that data handling is predictable and repeatable.

**Event Model**: At a high level, all data that enters Cribl Stream and Cribl Edge is treated as a JSON object. This standardized structure is what enables Functions to manipulate the data. For a detailed breakdown of the event schema, see the [Event Model](stream/event-model) documentation.

**Event Processing Order**: Events move through a fixed Pipeline from Source to Destination. While event order is preserved within a single connection on a single Worker, it is not guaranteed globally across multiple Workers. For a complete description of the processing sequence, refer to the [Event Processing Order](stream/event-processing-order) documentation.

An essential architectural principle is to keep routing logic fast. Avoid running expensive operations like complex regex in your Route filters, as these expressions are evaluated for every event. To optimize performance:

* Order your Routes to place the most CPU-intensive filters (like complex regex) as far down the routing list as possible. This ensures they run against the smallest possible subset of data, after cheaper, broader filters have already processed most events.

* Use boolean logic to short-circuit expensive operations. Always place simple, high-selectivity expressions first in your filter. For example, in the filter `index=='myIndex' && /myField/.test(_raw)`, the expensive regex is only evaluated if the simple index check passes, saving significant CPU.

This processing model is universal and consistent across both on-prem and Cribl.Cloud deployments.

## Memory and Performance Optimization

Performance is directly tied to how efficiently the system uses memory, CPU, and input/output.

* **Event Size**: The memory required to handle an event is directly proportional to its size. Minimize event size by dropping unneeded or high-cardinality fields as early as possible.

* **Function Efficiency**: Functions vary in their resource cost. A critical architectural principle is to order Functions within a Pipeline from **least to most expensive**. Always filter and discard unwanted data *before* performing complex transformations. For advanced optimization, you can cache computed fields in a variable early in the Pipeline to be reused by later Functions.

* **Regex Best Practices**: Inefficient regular expressions are a common cause of high CPU usage. Always anchor patterns (`^`, `$`) where possible, avoid catastrophic backtracking, and prefer simple string operations (for example, `startsWith()` or `includes()`) over regex for non-complex matching.

* **Queuing and Batching**: Use persistent queues (PQ) on disk to absorb backpressure from downstream destinations and prevent data loss. Judiciously batching data at the Destination also improves throughput and reduces the load on downstream systems.

The impact and control over these optimizations depend on your deployment model:
* **On-prem / Self-hosted**: You have full control over the infrastructure. This includes tuning system settings and provisioning appropriately sized storage for persistent queues.
* **Cribl.Cloud**: The underlying infrastructure, scaling, and restarts are managed for you. The focus is entirely on application-layer efficiency (Pipelines, Functions, regex) to control performance.
* **Hybrid (Stream & Edge)**: A common architectural pattern is to push heavy, complex transformations to the Stream Worker Group while keeping Edge Node configurations as lean as possible to minimize resource consumption on the endpoint.

## Pipeline Routing Strategies

Routing is the core logic that directs events to the correct processing Pipeline and Destination. A well-designed routing strategy is critical for an efficient, resilient, and maintainable system.

* **Routes**: The primary mechanism for controlling data flow is Routes. They are evaluated sequentially from top to bottom. The first Route whose filter expression matches an event will capture it and send it down its configured Pipeline. Because processing stops at the first matching **Final** Route, it's crucial to order them correctly to prevent subsequent rules from becoming unreachable.
* **Performance**: Because Routes are evaluated sequentially, filters at the top of the routing table are processed against the largest number of events. An event stops being evaluated by subsequent Routes once it matches a Final route. Therefore, all route filters must be highly efficient, but this is especially critical for Routes near the top of the list. Avoid complex logic, string operations on large fields, and especially any function that introduces latency (like a lookup) within a filter expression.
* **Advanced Routing & Resilience**: For complex architectures, certain internal components are useful:
    * **Output Router Destination**: This allows a single Pipeline to perform conditional routing to multiple Destinations. Useful for multi-destination fan-out, dynamic failover, and load balancing between downstream systems.
    * **Cribl Internal Source/Destination**: These pass data between different Cribl products, such as from a Cribl Edge Fleet to a Stream Worker Group. This is the primary mechanism for tiered processing in a hybrid deployment.
    * **DevNull Destination**: An internal Destination that discards events. It is useful for testing the performance and filter logic of Routes and Pipelines without sending data downstream.
* **Architectural Patterns & Maintainability**: As the number of Sources and Destinations grows, managing Routes is a key architectural requirement.
    * **Source Pre-processing and Common Pipeline Processing**: Use Pre-processing Pipelines (on the Source) for input-specific logic, like parsing a unique header. For global logic, use an initial, non-final Route that sends all data to a shared Pipeline for common tasks before final routing. This pattern promotes reusability.
    * **Catch-All Route**: Actively manage the default catch-all Route that Cribl provides. This prevents accidental data loss if the default is changed and ensures unhandled data is sent to a specific location for review or is explicitly dropped.
    * **Cloning vs. Referencing**: When multiple Routes need similar logic, you decide whether to clone the Pipeline or have all Routes reference the same one. Referencing simplifies updates, but cloning provides isolation if one Pipeline needs to diverge later.

For a deeper dive into Pipeline design, see our [Best Practices for Pipelines](stream/pipelines#best-practices-for-pipelines). To help build and debug the filter expressions used in [Routes](stream/routes), you can leverage our [AI-powered Copilot](stream/copilot-editor).

## Data Enrichment

Enrichment is the process of adding context to your data so it's more valuable for analytics and security use cases. An enrichment strategy involves choosing the right tool for the job.

### Enrichment Features

* **Lookup Function**: This remains the primary mechanism for most enrichment tasks. It uses a field from an event as a key to retrieve corresponding values from a data source (like a CSV file) and appends them to the event.
* **GeoIP Function**: A specialized and optimized function for a common use case: correlating an IP address with geographical data, including country, city, and coordinates.
* **Eval Function**: A versatile function used to create or modify fields based on arbitrary expressions. It's useful for performing calculations, manipulating strings, and reformatting data as part of an enrichment workflow.
* **Code Function**: For complex or custom enrichment logic that cannot be handled by other functions, the Code Function allows you to execute custom JavaScript. This provides maximum flexibility but should be used sensibly to ensure performance and maintainability.

### Architectural Considerations

Effective enrichment requires careful design to ensure performance, reliability, and scalability.

* **Choosing the Right Data Source**: The source of your enrichment data is a design choice. You can use static file-based lookups (like CSVs), connect to external SQL/NoSQL databases, or make real-time calls to external APIs. For large or frequently changing datasets (millions of rows), the recommended architecture is to host the lookup in a dedicated, high-performance service like Redis.
* **Enrichment Placement**: Best practice is to enrich after data has been filtered and reduced. This avoids wasting resources on enriching events that will be dropped. In a hybrid architecture, consider what context is available where: enrich with host-level metadata at the Edge, and with global, centralized data (like threat intelligence) in the Stream Worker Group.
* **Resilience and Error Handling**: Account for enrichment failures. What happens if a lookup returns no match, an external API times out, or a database is unreachable? You decide whether to drop the event, pass it through without enrichment, or add a default value. Implement logic to handle these failure modes to ensure data flow is not interrupted.

For technical details on how lookups work, see [About Lookups](stream/lookups-about). For practical examples and inspiration, explore the [Data Enrichment Use Case](use-cases/usecase-enrich).