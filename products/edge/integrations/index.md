# Integrations


Integrations enable seamless data flow between various systems. They encompass both Sources and Destinations, allowing you to get data from multiple origins and deliver processed data to various endpoints. Integrations are the entry and exit points in the [event processing order](event-processing-order).

* [Sources](sources) are configurations that enable Cribl to receive data from various origins. They serve as the entry points in the event processing order, enabling the collection of raw data for subsequent processing.

* [Destinations](destinations) are configurations that define where Cribl sends processed data. They act as the exit points in the event processing order, sending enriched and transformed events to the appropriate systems for storage, analysis, or further processing.

## Key Concepts for Optimal Data Flow

* [Destination Backpressure Triggers](destinations-backpressure-triggers): Understanding the conditions that can cause backpressure in Destinations is crucial for preventing data loss and ensuring smooth data flow, especially during peak load periods.

* [Persistent Queues](persistent-queues): To safeguard against data loss when integrations are slow or unavailable (backpressure events), persistent queues provide a temporary storage mechanism on disk.

* [Load Balancing](load-balancing): Distributing data evenly across multiple Sources and Destinations is essential for maximizing performance and reliability.
