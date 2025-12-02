# Working with Data


Cribl Edge is a tool to collect and process observability data
and deliver them to Cribl Stream or any supported Destination.

Cribl Edge also offers data processing and routing capabilities
that let you transform, enrich, reduce, and redact received data
before sending it to one or more target Destinations.

[Routes](routes) let you control the flow of events by filtering and directing them to selected Pipelines.

[Pipelines](pipelines) are where the data transformation happens.
Each Pipeline, a series of [Functions](functions), can modify the incoming events in a highly configurable way.

Cribl Edge offers tools to help you test and verify your data flow.
Use [Datagens](datagens) to generate sample data,
and check [Data Preview](data-preview) to see and validate the transformation you are performing.

[Packs](packs) offer portable sets of pre-packaged data routing and transformation tools
can include ready-made Routes, Pipelines, Function, and others.
