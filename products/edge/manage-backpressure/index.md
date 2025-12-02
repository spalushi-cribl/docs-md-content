# Manage Backpressure


Backpressure refers to mechanisms that manage the flow of data when production outpaces consumption. Sometimes an upstream Source can cause a backpressure event when Cribl Edge receives a sudden influx of data that it can't immediately process. And sometimes a downstream Destination can cause a backpressure event when the receiver doesn't have the capacity to receive incoming data from Cribl Edge. Backpressure events carry the risk of data loss, system crashes, or other undesirable effects.

Fortunately, Cribl Edge has built-in methods for responding to backpressure events. It has feedback mechanisms and triggers for detecting backpressure events. Cribl Edge also includes various settings for responding to backpressure events when they occur, such as enabling persistent queueing to store data during backpressure events.

Read the pages in this section to learn more about how Cribl Edge handles backpressure and backpressure configuration options:

- [About Persistent Queues](/persistent-queues)
- [Optimize Source Persistent Queues](/persistent-queues-sources)
- [Optimize Destination Persistent Queues](/persistent-queues-destinations)
- [About Destination Backpressure Triggers](/destinations-backpressure-triggers)
