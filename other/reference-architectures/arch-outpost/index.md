# Cribl Outpost Consideration

> #### Preview Feature
> This Preview feature is still being developed. While Cribl considers it to be
> generally stable, we do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance,
> and documentation for the feature might be incomplete.
>
> While you should continue to submit feedback and bug reports through normal
> Cribl support channels, support might be limited while the feature remains in
> Preview.
{.box .info}

Cribl Outpost (a Preview feature) is a new solution for managing deployments in restricted environments with multiple data centers or complicated networking setups.

Once you establish an Outpost Node and configure it to accept traffic from Edge Nodes, control plane communication between those Nodes and the Leader will pass through the Outpost. This design simplifies network maintenance: Instead of configuring firewall exceptions for potentially thousands of individual Edge Nodes, you only need to establish the communication lines to and from the Outpost Node(s).


| Benefit| Description |
| :--- | :--- |
| **Network simplification** | Outpost Nodes act as a proxy, shifting the network connection point from the Leader into your local environment. Edge Nodes connect only to the Outpost, drastically reducing the number of firewall rules and exceptions you need to manage (from thousands to just one set per Outpost). |
| **Resilience & HA** | Outpost Nodes are stateless. A single Outpost loss does not stop data flow but prevents changes (e.g., configuration deployments or upgrades) for connected Edge Nodes. For high availability, you can deploy multiple Outpost Nodes behind a load balancer or use DNS round-robin. You can track the status of disconnected Outposts and their connected Nodes from the Leader. |
| **Control vs. data plane** |The Outpost only handles control-plane traffic (configuration, orchestration). If your network has tight security restrictions and you are sending data to another location, you may need to enable additional firewall rules and ports for the data plane (flow of collected events) and/or leverage other third-party proxies. |

For details, see [Connect Edge Nodes to Leader Through Cribl Outpost](/edge/outpost/).