# Connect Nodes to Leader Through Cribl Outpost


Simplify Node-to-Leader communication with Cribl Outpost.

---



> ##### Preview Feature {#preview}
>
> This Preview feature is still being developed. We do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance, and related documentation could be incomplete.
>
> Please continue to submit feedback through normal Cribl [support channels](/support/),
> but assistance might be limited while the feature remains in Preview.
> 
{.box .info}

Cribl Outpost simplifies management of deployments 
by acting as a relay between individual Edge Nodes or Stream Workers and the Leader Node.

Once you establish an Outpost and configure it to accept traffic from Edge Nodes or Stream Workers,
control plane communication between those Nodes and the Leader will pass through the Outpost.

Cribl Outpost is ideal in situations where you can't establish direct communication
between Edge Nodes or Workers and the Leader, for example due to restrictions in opening up firewall ports.

Using an Outpost limits the need to set up third-party proxies
and simplifies maintenance by reducing the complexity of firewall setups.

> Cribl Outpost requires an Enterprise plan or Enterprise license. For details, see [Pricing](https://cribl.io/pricing/).
> 
> If you connect an Outpost to a Leader that does not have a proper plan or license,
> the Outpost node will close the listener for communication incoming from Edge Nodes or Workers,
> but will be shown as healthy on the Leader.
{.box .info}

## How Outpost Works

An Outpost is a special type of Node that rests between Edge Nodes or Stream Workers and the Leader.

![Overview of how Outpost connects Leader with Edge Nodes and Stream Workers](outpost-diagram-4.15.png)
{border="false" scale="90%"}

For an individual Edge Node or Worker, an Outpost is indistinguishable from the Leader:
Nodes communicate with the Outpost the same way they would with a Leader,
using the same configuration (for example, using the URL defined in the `CRIBL_DIST_LEADER_URL` environment variable or in `instance.yml`).

Instead of connecting directly to multiple Nodes (possibly thousands),
the Leader maintains those connections with one Outpost only.
This means you only need to configure communication lines such as firewall exceptions once per Outpost.
