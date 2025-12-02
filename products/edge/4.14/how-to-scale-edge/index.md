# Planning Large-Scale Deployments of Cribl Edge


Learn how to scale Cribl Edge deployments to support one Leader managing up to 250,000 Nodes. 

---

Our standard [Deployment Planning](deploy-planning) guide is a great place to
start if you intend to deploy 10,000 or fewer Nodes in your Cribl Edge environment.

For larger deployments, a Leader can support a maximum of 250,000 Edge Nodes
when metrics are set to **Minimal** mode and 100,000 Edge Nodes with metrics set
to **Basic**.

This guide covers: 

- The specific Leader configuration we recommend for best performance.
- Overview of upgrading a large Cribl Edge deployment.
- Limitations to the UI resulting from a large number of Edge Nodes.

## Recommended Leader Configuration

The base configuration detailed on [Cribl Stream Sizing and Scaling](/stream/scaling#vms)
is a good starting point.

From there, you can configure these settings on the Leader so it can manage up
to 250,000 Edge Nodes:

- Add 2GB of RAM for every 10,000 Edge Nodes.
- Deploy one vCPU core for every 10,000 Edge Nodes.
- For Cribl Edge to support up to 100,000 Nodes, you can [configure internal metrics](internal-metrics#edge-metrics-controls) to **Basic** in **Fleet Settings**.
- For 250,000 Nodes, you must configure internal metrics to **Minimal** in **Fleet Settings**.
    
  >You should configure these metrics for every Fleet if your Edge Node count 
  >is more than 10,000. See [Performance Considerations](deploy-planning#performance-considerations)
  >for more information.
  {.box .info}
  
- To spin up the right number of connection processes, add the number of Edge
  Nodes you will deploy into the [**Edge Nodes Count**
  field](deploy-planning#configure-edge-node-count).
  This field crunches numbers in the background and automatically spins up the
  correct number of Edge connection processes for you.
- There is no change to the default values when pushing config bundles from the Leader.

## Recommended Fleet Configuration

There are additional settings you'll need to configure for each Fleet you
intend to create:

- Add 1 vCPU per Fleet.
- Add 2GB of RAM per Fleet.

## Upgrading a Large Number of Edge Nodes

The [upgrading framework](upgrading) allows Edge Nodes to pull down upgrade
packages from the Leader asynchronously, rather than the Leader pushing packages
to Nodes all at one time. Nodes communicate to the Leader every 60 seconds via a
heartbeat.

Additionally, Nodes request upgrade packages from the Leader in batches of 500.
This allows you to upgrade Nodes without overloading the Leader, even with a
large number of Nodes. 

>In our controlled test environment, we found that
>upgrading can take 25-40 minutes. This is an expected time frame
>to upgrade that number of Nodes.
{.box .info}

## User Interface Limitations

For Fleets with a large number of Edge Nodes, the UI is limited in certain places
where a complete list of Nodes can't be displayed.

### Searching for Nodes in the Explore Tab

The [**Node**](explore-nodes) field in the **Explore** tab returns only the first
100 matching Nodes. When you start typing, the results match the search query
and are sorted alphabetically by hostname.

### Previewing Node Mappings

Preview Node mappings by selecting **Mappings** in the Leader Node's sidebar. (Only the first 10,000 mappings are displayed.)

### Other Limitations

- **Logs**: The drop-down where you can select an Edge Node will display a
  maximum of 100 Nodes.
- **Import Edge Data**: The **Import Edge Data** window in Cribl Stream returns a
  maximum of 100 Edge Nodes. 
- **Test tab**: The **Test** tab on Destinations displays a maximum of 100 results. 
- **KMS Status report**: In **Fleet Settings**, the **KMS Status report** (under
  **Security**) does not display all Nodes. Instead, search for your desired
  Node.