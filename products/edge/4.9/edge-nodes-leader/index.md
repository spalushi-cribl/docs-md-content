# How Edge Nodes and Leader Nodes Work Together


Learn how Edge Nodes and Leader Nodes work together

---

The Leader Node has two primary roles:  

1. Serves as a central location for Edge Nodes' operational metrics. The Leader
   ships with a monitoring console that has a number of dashboards, covering
   almost every operational aspect of the deployment.
 
1. Serves as a central location for authoring, validating, deploying, and
   synchronizing configurations across Fleets. 

![Leader Node/Edge Nodes relationship](edge-master-node-with-work-groups.c850974436.jpg)
{border="true"}

## Network Port Requirements (Defaults)

- UI access to Leader Node: TCP 9000.
- Edge Node to Leader Node: TCP 4200. Used for Heartbeat/Metrics/Leader requests/notifications to clients (such as live captures, teleporting, status updates, and config bundle notifications).
- Edge Node to Leader Node: HTTPS 4200. Used for config bundle downloads.
- Edge Node bootstrapping/update: TCP 443.

## Leader/Edge Node Communication

Edge Nodes will send a heartbeat to the Leader every 60 seconds.
This heartbeat includes information about the Nodes and a set of current
system metrics. The heartbeat payload includes facts – such as hostname, IP
address, GUID, tags, environment variables, and current software/configuration
version – that the Leader tracks with the connection. 

When an Edge Node checks in with the Leader:
 
- It sends a heartbeat to Leader with "facts" about itself.
- The Leader uses Edge Node's facts and Mapping Rules to map it to a Fleet.
- The Edge Node pulls its Fleet's updated configuration bundle, if necessary.

Edge Nodes that don’t maintain their connection to the Leader will eventually disappear from the Edge Nodes page.

> The Leader is unaware of Edge Nodes' platforms (such as Linux or Windows) within a Fleet. So the ConfigHelper omits platform-specific limitations. Therefore, when you manage Edge Nodes on heterogeneous platforms, create a Windows-specific Fleet and mapping. See [Fleets Hierarchy and Design](fleets-design). 
>
{.box .warning}

### Leader Disconnection and Telemetry

Cribl Edge is built to keep your data flowing smoothly, even if it temporarily loses connection with the Leader Node. This means you won't experience disruptions during network hiccups or if the Leader is briefly unavailable.
Cribl Edge periodically sends telemetry to verify its license with Cribl's servers. This is different from its connection to the Leader. If the telemetry fails for an excessive time, processing might be interrupted.

> This telemetry is disabled for Cribl.Cloud-managed Edge Nodes.
>
{.box .info}

Cribl Edge Nodes that are disconnected from the Leader will resume operation after an Edge restart, using their last settings and license. In most cases, an Edge Node gets its license from the Leader, so if it restarts without a connection, it will use a temporary free license and continue its check-ins. Just keep in mind that while the Edge Node is disconnected, you won't see real-time updates in the main monitoring dashboard, and it won't receive any new configuration changes.