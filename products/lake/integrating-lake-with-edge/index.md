# Integrating Cribl Lake with Cribl Edge


Archive your Cribl Edge data to Cribl Lake, via Cribl Stream.

---

To send data from Cribl Edge to Cribl Lake, you need to route it through Cribl Stream, using the Cribl HTTP or Cribl TCP Destination/Source pair. Either option prevents double billing when you ingest the data from Edge to Stream.

The main steps are:

1. [Send data](#send-data-from-cribl-edge) from Cribl Edge to a Stream Worker Group.
1. [Receive data](#receive-data-in-cribl-stream) in Cribl Stream.
1. [Send data](#send-data-to-cribl-lake) from Cribl Stream to Cribl Lake.

> To learn more about communicating between Cribl Edge and Cribl Stream,
> see the [Cribl Edge to Cribl Stream](/edge/usecase-edge-stream/) guide.
> 
> The [Edge Getting Started tutorial](/edge/getting-started-tutorial/)
> presents a similar use case with detailed steps for configuring the Cribl HTTP Sources and Destination
> to seamlessly send data between Edge and Stream.
{.box .success}

## Send Data from Cribl Edge

1. In Cribl Edge, create a new [Cribl HTTP](/edge/destinations-cribl-http/) Destination.
(You could also use [Cribl TCP](/edge/destinations-cribl-tcp/), but this example will use the former.)
1. Next, use QuickConnect to pass your Edge data to this Destination.

## Receive Data in Cribl Stream

1. In Cribl Stream, create a new [Cribl HTTP](/stream/sources-cribl-http/) Source.
1. Make sure that the **Address** and **Port** fields in the Source's configuration correspond to what you configured in Cribl Edge.

## Send Data to Cribl Lake

1. Once data is flowing from Cribl Edge to Cribl Stream, create a [Cribl Lake Destination](destinations-cribl-lake) in the same Worker Group.
1. Select the Lake Dataset you want to use as the target for the Destination.
1. Finally, connect the Cribl HTTP Source with the new Cribl Lake Destination via QuickConnect.

## Verify Data Flow

To make sure that data is flowing correctly into Cribl Lake, you can run a test
[search](/search/search-cribl-lake/) over the selected Lake Dataset.