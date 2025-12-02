# Working with Data


Cribl Stream is a tool to collect, process, and route data.
After receiving events from a Source you can work with data
to transform, enrich, reduce, and redact it, before sending it to one or more target Destinations.

[Routes](routes) let you control the flow of events by filtering and directing them to selected Pipelines.

[Pipelines](pipelines) are where the data transformation happens.
Each Pipeline, a series of [Functions](functions), can modify the incoming events in a highly configurable way.

Cribl Stream offers tools to help you test and verify your data flow.
Use [Datagens](datagens) to generate sample data,
and check [Data Preview](data-preview) to see and validate the transformation you are performing.

[Packs](packs) offer portable sets of pre-packaged data routing and transformation tools
can include ready-made Routes, Pipelines, Function, and others.

> For a series of guides and better practices in enriching, sampling, and transforming data, see:
> - [Using Lookups](using-lookups)
> - [Data Sampling](data-sampling)
> - [Transforming Data](transforming-data)
{.box .success}
