# Route Data to Multiple Destinations Using Cribl Stream


Cribl Stream makes it easy to route data to multiple downstream analytics and storage services. A particularly useful scenario is to send full-fidelity data to inexpensive object storage (like Cribl Lake) for retention, while filtering and optimizing valuable data for downstream analysis. Or, you might want to split the same incoming events, metrics, or traces into separate processing streams for different teams, applying transformations specific to each team's needs and downstream services. To route data to the optimal destinations, with appropriate transformations, follow this roadmap.

## Identify What Needs to Go Where, in What Form
If you need to retain full-fidelity data for compliance or future threat-hunting, you'll want to [route](/stream/routes/) all incoming events to commodity storage like Cribl Lake. To send only "interesting" complete events to downstream security analytics, a second Route's [Pipeline](/stream/pipelines/#pipelines) could apply both filtering and a sampling algorithm. For telemetry monitoring, a third Pipeline might aggregate, redact, or otherwise transform the whole data stream to optimize it for downstream analytics.

## Determine How to Route Your Data
If independent, parallel streams of data will meet your needs, the [QuickConnect](/stream/quickconnect/) visual UI is the simplest way to draw connections between upstream and downstream services. To flexibly route data according to a cascading set of conditional filters, use [Data Routes](/stream/routes/).

## Set Up Sources and Destinations
Each Cribl Stream [Source](/stream/sources/) and [Destination](/stream/destinations/) is a configuration model of (respectively) an upstream or downstream service in your infrastructure. Some of these integrations are designed to connect a specific service. Others – like the HTTP Source, or the TCP JSON Destination – are generic integrations that you can customize to handle services that don't have a native integration.

## Build Processing Pipelines
As you connect Sources and Destinations through [Pipelines](/stream/pipelines/#pipelines), you can plug in and configure an extensive set of built-in [Functions](/stream/functions/) to parse, filter, reduce, or otherwise transform each stream's data to meet your needs. Enrich your data with additional context from external sources by applying built-in or custom lookup tables, database connections, and other [Knowledge Libraries](/stream/knowledge-library/). Redact or mask sensitive field values to comply with security policies and best practices. Remove fields that your downstream analytics don't need, and freely add fields to tag transformed events.

## Test, Monitor, Go Live
Cribl Stream provides built-in [data generators](/stream/datagens/), data capture to sample files, and [Data Preview](/stream/data-preview/) options to help you iterate on, and validate, your routing and transformations before you incur any ingress or egress costs with live data.

To test data flow beyond static samples, and to then enable it in production, you'll need to provision Workers in the Worker Group you're configuring. For details, see [Managing Cribl.Cloud Worker Groups](/stream/cloud-workers/).

## That's It!
Cribl Stream enables you to precisely route and optimize data in motion, ensuring that the right data is delivered to the right places in the right format. For a step-by-step example, see our [Bifurcating Observability Data to Multiple Destinations](https://cribl.io/blog/bifurcating-observability-data-to-multiple-destinations/) blog post. You can find an updated version on our [Cribl Amazing Data Stories](https://cribl.io/amazing) site, by downloading *Your Guide to the Data Engine of Tomorrow*.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}