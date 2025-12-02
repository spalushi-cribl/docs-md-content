# Design Fleet Hierarchy


Design your Fleets into a practical hierarchy.

---

Cribl Edge enables you to group Edge Nodes into logical Fleets and Subfleets.
A well-designed Fleet hierarchy is extremely important for the health of the Leader Node
and lets you take advantage of configuration inheritance.

## Fleet Design Better Practices

Always design your Fleet hierarchy before deploying your Edge Nodes. 
For details, see [Designing Your Fleet Hierarchy](#hierarchy).

### Consider Leader Health 

To ensure the health of your Leader Node, follow these guidelines:

- Consider the number of Edge Nodes restarting at once, 
  since they will all be pulling their configurations from the Leader at the same time.

- For deployments with over 100,000 Edge Nodes, 
  change the default [metrics controls](internal-metrics#edge-metrics-controls) to `minimal`.

### Deploy Nodes to Specific Fleets

Use the bootstrap script in the [Add/Update Edge Node](edge-nodes-add-update) 
modal to deploy Edge Nodes to a specific Fleet.
Depending on your Fleet design, Cribl recommends adding tags to
the bootstrap script to uniquely identify the Fleet. 
You can use these tags for automatic mappings.

### Use `default_fleet` as Fallback for Stray Nodes

Do not map Edge Nodes to `default_fleet`. It should serve as fallback for mapping errors. 

Create mappings for all the Fleets so that Edge Nodes are automatically placed in the Fleet when they bootstrap and connect to the Leader.
This way, if any rogue Edge Node gets deployed, it will automatically show up in `default_fleet`.
You can then monitor `default_fleet` to catch those Nodes and reassign them.

If you disable all the Sources in the `default_fleet`, there will be no impact on data consumption. 

> This guidance doesn't apply to the [free license](licensing#free-license), 
> where the only option is `default_fleet`.
{.box .info}

## Design Your Fleet Hierarchy {#hierarchy}

If your organization is deploying a significant number of Edge Nodes,
it’s essential to thoroughly plan and design your Fleet architecture.
If you are moving from other agent-based systems, avoid exactly replicating the previous structure,
as Cribl Edge operates differently.

The first question you should ask is, do you need Subfleets?
In some cases, a single-level, flat design is sufficient.

In more complex environments, layering the configurations can help you to manage a large number of Edge Nodes
that share common yet differentiated configurations.
For example, as an administrator, you might need multiple layers of configurations
to manage the following: `Org` > `Department` > `OS`.
In this case, the Edge Nodes at the `OS` level can inherit and share configurations from the `Department` level.
You can update a parent-level configuration (Fleet) and have it applied to all Subfleets and their respective Edge Nodes,
reducing the time and effort needed to manage them.

Maintaining a large number of Fleets can introduce operational complexities and increase the overhead of managing your Cribl Edge deployment.
Before creating a large number of Fleets, explore whether your requirements can be effectively addressed
by leveraging Cribl Stream as a central Destination
and using Pipelines within a more streamlined Fleet structure to manage routing and processing logic.

### Fleet Hierarchy Options

There are several ways you can build the hierarchy:

- Geographical: If your organization collects different data categories in different regions, consider this option.
- Data center: This is a good option if you maintain data centers in different regions. 
- Server function (web server, database, and so on): Another good way to organize your Fleets can be based on server functions.
- OS-based: If you are running an environment with both Linux and Windows machines, consider separating them into different Fleets per OS. 

### Fleet Limitations and Recommendations

When designing your Fleet hierarchy, consider the following recommendations:

- **Recommended Depth:** To promote manageability and performance, limit your Fleet hierarchy to no more than **two levels** of Subfleets below the top-level Fleet.

- **Cloud Environment Restriction:** Cribl.Cloud environments have a current limitation of **50 Fleets** per deployment. Carefully plan and optimize your Fleet design to stay within the limit.

If you are working in a large organization and find these limitations insufficient,
**engage with your Cribl account team and Cribl design partners**
to develop an optimized Fleet design that meets your specific needs.

## Fleet Naming {#name}

Consider naming your Fleets and Subfleets in a way that helps you recognize the configurations at a glance. 

For example, you can use the top-level Fleet name as part of your lower-level Fleets. Naming your Fleet with a logical `<parent-fleet>-<subfleet1>-<subfleet2>` will easily provide you with context whether you are viewing at the Fleet, Subfleet, or Edge Node level. It can also help identify Edge Nodes that are out of place or mapped incorrectly. 

Here's an example of naming a Fleet with two levels of Subfleets:

- BZT
    - BZT-NewYork
        - BZT-NY-Linux
        - BZT-NY-Windows
    - BZT-LA
        - BZT-LA-K8s
        - and so on

Other guidelines to keep in mind when naming your Fleets:

- Fleet names can't have spaces in them; use a dash or underscore instead.
- You can't reuse a Fleet name.
- Once you create a Fleet, you can't edit the name.
- You can't delete a Fleet if there are Subfleets under it.

## Plan Fleet Inheritance {#inheritance} 

An efficient Fleet hierarchy design lets you take full advantage of Fleet inheritance.
Each Subfleet will inherit the configurations of every parent Subfleet before it.
To learn more about how inheritance works, see [Fleet Inheritance](fleet-inheritance).

Here are some guidelines to keep in mind, as you think about layering your configurations. 

### Top-Level Fleets (Grandparents)

Assign the top-level Fleets for company-wide configurations.
Use this level to:

- Apply certificates and other company-wide settings.
- Avoid OS-specific or site-specific configurations.

### Level 1 Subfleets (Parents)

Per-site configurations. Use this level to:

- Configure site-specific Sources/Destinations.
- Specify any site-specific configurations, like common Destinations.

### Level 2 Subfleets (Children)

Configurations used by groups of similar systems. Use this level to:

- Apply OS-specific configurations at this level. For example, an “All Linux” and “All Windows” for a given site.
- Group common OS-specific metrics and common log files into OS-specific Fleets. 

### Level 3 Subfleets (Grandchildren)

While Cribl typically doesn't recommend creating more than two levels of Subfleets,
if your specific complex organization requires them,
use the third level to apply function-specific Subfleets to add specific metrics, logs, and/or Windows Event sources.
For example, you can group by:

- Apache HTTPD
- Database
- SMTP servers
- Active Directory
- IIS
- Exchange 
