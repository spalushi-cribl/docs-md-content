# Product Suite Interoperation

Cribl products are designed to operate independently, but they have compounding value when working together. This section describes the most common interoperability scenarios. The shared management and control planes provide you with the facilities to easily coordinate integration and security across all products.

## Data Transfer {#data-transfer}

Cribl products are designed to send data between different data planes without incurring duplicate charges.

### Transferring Data in Motion {#transfer-stream-edge}

Cribl Edge and Cribl Stream pricing is based on the data they collect or
receive. A common data transfer pattern is to collect data at the endpoint with
Cribl Edge, then distribute that data to Cribl Stream for additional processing
and last-mile distribution. 

This architecture allows you to aggregate data centrally from distributed
endpoints. A key advantage is that it provides a single distribution point,
making it more secure and easier for receivers to secure data reception. This
architecture also provides last-mile queuing for resilience in the case of
outages in your downstream receiver. 

When different Cribl products are connected to the same control plane (called a
Leader), they use one of two specialized protocols to send data to each other:
Cribl HTTP or Cribl TCP.

These Cribl-specific protocols also enable data
transfer between Workers that are not connected to the same Leader. This is
common in hybrid cloud deployments or when transferring data between different
Cribl.Cloud Workspaces or on-prem environments. Cribl only counts data towards
your usage on the Worker where it's initially received or collected.

Using these Cribl-specific protocols, you can send data on to these recipients
at no additional cost:

- One or more additional Edge Nodes.

- One or more additional Worker Groups.

- Another Cribl.Cloud Workspace on a billing plan or Organization with a paid
  on-prem license. This includes scenarios where the sending and receiving
  environments do not share a Leader, but have the same on-prem license key
  installed or are within the same Cribl.Cloud Organization. 
  See [Transfer Data Between Workspaces or Environments](/stream/usecase-transfer-data) for more
  details.

>For details about these protocols' implementation, see [Interoperation Protocols](ref-arch-protocols).
>
{.box .success}

### Transferring Data to/from Cribl Lake {#transfer-lake}

Cribl Stream has built-in Destinations and Sources for Cribl Lake, which can respectively send data to, and collect data from, specified Cribl Lake Datasets. Configuring these Destinations and Sources requires the Editor or higher Permission at the Cribl Stream [product level](/stream/permissions#product), or the Editor or Higher Permission on an individual [Worker Group](/stream/permissions#group). Once these integrations are configured, transferring data to and from Cribl Lake requires no Permission settings on Cribl Lake.

### Transferring Data to/from Cribl Search {#transfer-search}

Cribl Search results can be either routed to Cribl Stream for additional processing, or stored directly in Cribl Lake, using Cribl Search commands for each product. Routing to Cribl Stream will send the data (at no additional cost) through the Cribl Stream processing, routing, and distribution workflow, for subsequent routing to other tools like a SIEM or IT Ops analytics tool.

Of course, the primary (and more interesting) aspect of Cribl Search is [directly querying and analyzing data](/search/search-your-data/).

## Querying Data in Cribl Search {#data-query}

Cribl Search is designed to work out-of-the-box with other parts of the Cribl Product Suite. 

Cribl Search can query data on endpoints [using Cribl Edge](/search/set-up-edge). Data can be buffered at the edge using source-based buffers or a dedicated buffer destination. Cribl Search can dispatch operators to Edge Nodes managed by a shared Leader. 

Cribl Search also has built-in Dataset Providers for Cribl Stream. These providers access Cribl Stream internal operational logs, which you can use to facilitate troubleshooting and analysis of Cribl Stream internal operations. 

>By design, Cribl Search does not search data that Cribl Stream is actively processing, because doing so would impose a performance penalty on the streaming data, leading to higher latency and slower processing times.
>
{.box .info}

Cribl Search has tight integration with Cribl Lake, which is a [default Dataset Provider](/search/search-cribl-lake). Also, any new Dataset created in Cribl Lake can be queried immediately by teams who have been granted access to that Dataset. No additional configuration is required, and all products with a shared control plane also have a unified RBAC model to ensure secure access by users authorized on each of the different products.
