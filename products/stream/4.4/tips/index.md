# Tips and Tricks


In designing, supporting, and troubleshooting,  complex Cribl Stream deployments and workflows, we've found the following general principles helpful. 

> Every use case is different. Don't hesitate to contact Cribl Support for help with solving your specific needs.
>
{.box .success}

## Architecture and Deployment

#### Separate Data by Worker Groups, Routes, or Both? {#separate}

Separate your data volume by Worker Groups, or by Routes, or both. Common business reasons for sorting by Groups include data isolation by business group, ensuring data privacy, and achieving cost savings through geo-isolation. Another consideration is: Which type of separation will be the easiest to understand and manage as your organization grows? 

#### Connecting Cribl Stream Groups/Instances with Same Leader: Cribl HTTP or Cribl TCP {#common-pair}

Where Workers share the same Leader, the obvious choices for sending data between those Workers are the Cribl HTTP [Source](sources-cribl-http) and [Destination](destinations-cribl-http) pair, or the Cribl TCP [Source](sources-cribl-tcp) and [Destination](destinations-cribl-tcp) pair. Either option prevents double-counting of this internal data flow against your on-prem license or Cribl.Cloud plan.

#### Connecting Stream Groups/Instances with Different Leaders: TCP JSON {#disparate-pair}

When sending between Cribl Stream Worker Groups and/or instances connected to **different** Leaders, use the TCP JSON [Source](sources-tcp-json) and [Destination](destinations-tcp-json) pair. While Cribl Stream supports multiple Sources/Destinations for sending and receiving, TCP JSON is ideal because it supports TLS, has solid compression, and is overall well-formatted to maintain data's structure between sender and receiver.

## Input Side: Event Breakers and Timestamping

Validate timestamping and event breaking before turning on a Source.

If a Source supports Event Breakers (e.g., AWS Sources), it is much more efficient to perform JSON unroll in a Breaker, versus in a Pipeline Function. 

## Routes

#### Avoid Route Creep

Design data paths to move through as few Routes as possible. This usually means we want to reduce volume of events as early as possible. If most of the events being processed are sent to a Destination by the first Route, this will spare all the other Routes and Pipelines wasted processing cycles and testing against whether filter criteria are met.

#### Prevent Function Creep

As a mirror principle, clone events as late as possible in the Routes. This will minimize the number of Functions acting on data, to the extent possible.

#### Leave No Data Behind

Create a catchall Route at the end of the list to explicitly route events that fail to match any Route filters.

## Pipelines and Functions Logic

#### Don't Overload Pipelines

Do not use the same Pipeline for both pre-processing and post-processing. This makes isolation and troubleshooting extremely difficult.

#### Extract or Parse by Desired Yield

If you need to extract one or just a few fields, use the [Regex Extract](regex-extract-function) Function. If you need to extract all or most of an event's fields, use the [Parser](parser-function) Function.

#### Use Function Groups for Legibility

Function groups might be helpful in organizing your Pipeline. These groups are abstractions, purely for visual context, and do not affect the movement of data through Functions. Data will move down the listed Functions, ignoring any grouping assignments. 

#### Use Comments to Preserve Legibility

Comment, comment, comment. There is a lot of contextual information which might become lost over time as users continue to advance and add Routes and Pipelines to Cribl Stream. A good principle is to keep the design decisions as simple and easy to understand as possible, and to document the assumptions around each Route and Pipeline in comments as clearly as possible. 

#### Create Expressions Around Fields with Non-Alphanumeric Characters

If there are fields with non-alphanumeric characters – e.g., `@timestamp` or `user‑agent` or `kubernetes.namespace_name` – you can access them using `__e['<field-name-here>']`. (Note the single quotes.) For more details, see MDN's [Property Accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors) documentation. In any other place where such fields are referenced – e.g., in an [Eval Function](eval-function)'s Field names – you should use a single-quoted literal, of the form: `'<field-name-here>'`.

####  Specify Fields' Precedence Order in Expressions  {#precedence}

In any Source that supports adding **Fields (Metadata)**, your **Value** expression can specify that fields in events should override these fields' values. E.g., the following expression's L‑>R/OR logic specifies that if an inbound event includes an `index` field, use that field's value; otherwise, fall back to the `myIndex` constant defined here: `` `${__e['index'] || 'myIndex'}` ``.

#### Break Up `_raw`

Consider avoiding the use of `_raw` as a temporary location for data. Instead, split out explicitly separate fields/variables.

#### Optimize PQ Using File Size

Consider using a smaller maximum file size in [Persistent Queues](persistent-queues) settings, for better buffering.

## Output Side

Although this might be obvious: Ship metrics out to a dedicated alerting/metrics engine (ELK, Grafana, Splunk, etc.)

## Troubleshooting

Troubleshoot streams processing systems from right to left. Start at the Destination, and check for block status from the Destination back to the Source.

Don't run health checks on data ports too frequently, as this can lead to false-positives errors.
