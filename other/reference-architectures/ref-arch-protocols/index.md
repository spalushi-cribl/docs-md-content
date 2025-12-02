# Interoperation Protocols

This section details the protocols Cribl products and planes use to communicate with each other. All protocols described in this section can be secured using TLS.

To send data between Worker Nodes and/or Edge Nodes without duplicate charges, use either the HTTP or TCP protocol. For details, see [Data Transfer](ref-arch-products#data-transfer).

Note that each of these protocols is designed to be used as a paired Source and Destination. For example, you might send data from a [Cribl HTTP Destination](/edge/destinations-cribl-http) in  Cribl Edge to a [Cribl HTTP Source](/stream/sources-cribl-http) in Cribl Stream. A similar pairing would be a [Cribl TCP Destination](/edge/destinations-cribl-tcp) to a [Cribl TCP Source](/stream/sources-cribl-tcp). And to send data between Worker Groups, Cribl Stream exposes its own version of the [Cribl HTTP Destination](/stream/destinations-cribl-http) and [Cribl TCP Destination](/stream/destinations-cribl-tcp), which have exactly the same configuration options as their Edge counterparts.

## Cribl HTTP {#cribl-http}

For most use cases, Cribl HTTP is the preferable protocol, offering a more performant solution that's easier to set up and use. Its core advantage is that it inherently allows for a more efficient and even distribution of events across Worker Processes.

It is ideal for scenarios involving Network Load Balancers (NLBs) and proxies, as it's designed to work seamlessly with them. You can place a load balancer in front of multiple Workers, to ensure even more consistent distribution of events across Worker Processes. By default, in Cribl.Cloud-managed data planes, all HTTP data is distributed using an NLB.

## Cribl TCP {#cribl-tcp}

Cribl TCP is built to send to multithreaded receivers. Its current limitation is vulnerability to "pinning", where a single connection can get stuck on one Worker Process, leading to uneven workload. 

This pinning issue is mitigated by enabling the built-in load balancing feature on the TCP Source side, which automatically distributes incoming data across all available Worker Processes on that Node. Like HTTP, Cribl TCP also supports the use of external load balancers for distribution across Workers.


Setting up Cribl TCP is more involved than Cribl HTTP setup, and the difficulty increases as the network topology becomes more complex. Cribl Edge-to-Stream forwarding is one example of such a complex scenario.

## Leader Communication with Groups and Fleets {#leader-protocol}

The communication path between the Stream/Edge data plane and the control plane is secured on port 4200 using TCP. The Workers and Edge Nodes must be able to communicate/connect to the Leader on this port, using this protocol.