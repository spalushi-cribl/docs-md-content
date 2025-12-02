# Basic Concepts


Notable features and concepts to get a fundamental understanding of Cribl Edge

---

As we describe features and concepts, it helps to have a mental model of Cribl Edge as a system that 
collects and process observability data – logs, metrics, application data, etc. – in near real time, from your Linux and Windows machines, apps, microservices etc., and delivers them to Cribl Stream or any supported destination.

![Edge processing, management, and receivers](edge-fleet-management.c74a757fdf.jpg)

Let's zoom in on the center of the above diagram, into an Edge Node, to take a closer look at the processing and transformation options. The basic interface concepts to work with are [Sources](#sources), which collect data; and [Routes](#routes), which manage data flowing through [Pipelines](#pipelines), which consist of [Functions](#functions).

![Routes, Pipelines, Functions](edge-diagram-complex-new.7145e71e7c.jpg)

## Sources 


[Sources](sources) are configurations that enable Edge to collect data from the local machine (logs, metrics, etc.) or to receive data from remote senders (TCP, Syslog etc. ).

## QuickConnect

QuickConnect is a graphical interface for setting up data flow through your Edge deployment. You can quickly drag and drop connections between Sources and [Destinations](destinations), optionally including – or excluding – [Pipelines](pipelines) or [Packs](packs).

The only major constraint is that QuickConnect completely bypasses [Routes](routes). So QuickConnect configurations have no Routing table, and no conditional cloning, cascading, or routing of data – every QuickConnect connection is parallel and independent.

You can access the **QuickConnect** interface from the top nav's **Collect** screen.

## Routes


Routes evaluate incoming events against **filter** expressions to find the appropriate Pipeline to send them to. Routes are **evaluated in order**. Each Route can be associated **with only one** Pipeline and one output (configured as a Edge **Destination**). 

By default, each Route is created with its `Final` flag set to `Yes`. With this setting, a Route-Pipeline-Destination set will consume events that match its filter, and that's that.

However, if you disable the Route's `Final` flag, one or more event **clones** will be sent down the associated Pipeline, while the original event continues down Edge's Routing table to be evaluated against  other configured Routes. This is very useful in cases where the same set of events needs to be processed in multiple ways, and delivered to different destinations. For more details, see [Routes](routes).

## Pipelines


A series of Functions is called a Pipeline, and the order in which you specify the Functions determines their execution order. Routes deliver Events to the beginning of a Pipeline, and as they're processed by a Function, the events are passed to the next Function down the line. 

![](pipeline.1da03169c5.png)
{scale="40%"}

Pipelines attached to Routes are called **processing Pipelines**. You can optionally attach **pre-processing Pipelines** to individual Edge Sources, and attach **post-processing Pipelines** to Edge Destinations. All Pipelines are configured through the same UI – these three designations are determined only by a Pipeline's placement in Edge's data flow.

![Pipelines categorized by position](routes-isometricFlow.8bcf8a3c08.png)
{scale="80%"}

Events only move forward – toward the end of a Pipeline, and eventually out of the system. For more details, see [Pipelines](pipelines).

## Functions


At its core, a **Function** is a piece of code that executes on an event, and that encapsulates the smallest amount of processing that can happen to that event. For instance, a very simple Function can be one that replaces the term `foo` with `bar` on each event. Another one can hash or encrypt `bar`. Yet another function can add a field – say, `dc=jfk-42` – to any event with `source=*us-nyc-application.log`. 

![Functions stacked in a Pipeline](functions-example.e41506e6b8.png)

Functions process each event that passes through them. To help improve performance, Functions can optionally be configured with [filters](introduction-reference#filters), to limit their processing scope to matching events only. For more details, see [Functions](functions).
