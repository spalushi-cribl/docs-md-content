# Fleets Hierarchy and Design


Cribl Edge enables you to flexibly group Edge Nodes into logical Fleets and Subfleets, organized as you like. However, Fleet design is extremely important for the health of the Leader. In this guide, we address "better practices" for designing your Fleets and Subfleets.

## Leader Health 

Below are some guidelines for designing your Fleets to optimize your Leader's health:

- Consider the number of Edge Nodes restarting at once, 
  since they will all be pulling their configurations from the Leader at the same time.

- For deployments with a larger number of Edge nodes, 
  change the default [metrics controls](internal-metrics#edge-metrics-controls) to `minimal`.

## Fleet Design Better Practices for Enterprises 

Consider the following "better practices" for designing Fleets for enterprises:

- Do not use `default_fleet`. 
  The `default_fleet` is the fallback for mapping errors. 
  If you purchased an [Enterprise License](licensing#enterprise-license), 
  disable all the Sources in the `default_fleet` to prevent accidental collections. 
  This guideline doesn't apply to the [free license](licensing#free-license), 
  where the only option is `default_fleet`.

- Design your Fleet hierarchy before deploying your Edge Nodes. 
  For details, see [Designing a Fleet Hierarchy](#hierarchy) below.

- Use the bootstrap script in the [Add/Update Edge Node](managing-edge-nodes#bootstrap) 
  modal to deploy Edge Nodes to a specific Fleet.
  Depending on your Fleet design, we recommend adding tags to
  the bootstrap script to uniquely identify the Fleet. 
  You can use these tags for automatic mappings.

- Create mappings for all the Fleets so that Edge Nodes are automatically placed in the Fleet when they bootstrap and connect to the Leader. This way, if any rogue Edge Node gets deployed, it will automatically show up in `default_fleet`. If you disable all the Sources in the `default_fleet`, there will be no impact on data consumption. 

- Monitor your `default_fleet` for any unintentional/rogue Edge Nodes. 

## Designing Your Fleet Hierarchy {#hierarchy}

The first question you should ask is, do you need Subfleets? In some cases, a single-level (flat) design is sufficient.

In more complex environments, layering the configurations can help you to manage a large number of Edge Nodes that share common yet differentiated configurations. For example, as an administrator, you might need multiple layers of configurations to manage the following: `Org` > `Department` > `OS` > `Application`. In this case, the Edge Nodes at the `Application` level can inherit and share configurations from the `Org` level. You can update a parent-level configuration (Fleet) and have it applied to all Subfleets and their respective Edge Nodes, reducing the time and effort needed to manage them.

There are several ways you can build the hierarchy:

- Geographical: If your organization collects different data categories in different regions, consider this option.
- Data center: This is a good option if you maintain data centers in different regions. 
- Server function (web server, database, etc.): Another good way to organize your Fleets can be based on server functions.
- OS-based: If you are running an environment with both Linux and Windows machines, consider separating them into different Fleets per OS. 
  
To learn more about Fleets, see [Fleets](fleets) and check out [Configuring a Fleet](fleets#configuring) for configuration details. 

### Fleet Hierarchy Considerations

Some points to consider when designing your hierarchy: 

* **Recommended Depth:** To promote manageability and performance, limit your Fleet hierarchy to no more than **two levels** of Subfleets below the top-level Fleet. Examples provided in this document will primarily illustrate two levels.

* **Cloud Environment Restriction:** The Cribl.Cloud environments have a current limitation of **50 Fleets** per deployment. Carefully plan and optimze your Fleet design to stay within the limit.

* **Considerations for Large Organizations:** The general guidance provided in this document may readily apply to many smaller organizations. However, larger organizations with complex environments and a large number of Edge Nodes may find that a two-level hierarchy is insufficient. In such cases, it is strongly recommended to **engage with your Cribl account team and Cribl design partners** to develop an optimized Fleet design that meets your specific needs.

* **Strategic Deployment:** For organizations deploying a significant number of Edge Nodes, a thorough **planning and design phase** for your Fleet architecture is essential. Avoid exactly replicating the structure used in previous agent-based systems, as Cribl Edge operates differently.

* **Complexity and Overhead:** Maintaining a large number of Fleets can introduce operational complexities and increase the overhead of managing your Cribl Edge deployment. Before creating a large number of Fleets, explore whether your requirements can be effectively addressed by leveraging Cribl Stream as a central Destination and using **Pipelines** within a more streamlined Fleet structure to manage routing and processing logic.


## Naming your Fleets {#name}

What's in a name, you ask? Well, a whole lot when it comes to Fleets. Consider naming your Fleets and Subfleets in a way that helps you recognize the configurations at a glance. 

For example, you can use the top-level Fleet name as part of your lower-level Fleets. Naming your Fleet with a logical `<parent-fleet>-<subfleet1>-<subfleet2>` will easily provide you with context whether you are viewing at the Fleet, Subfleet, or Edge Node level. It can also help identify Edge Nodes that are out of place or mapped incorrectly. 

Here's an example of naming a four-level Fleet in an intuitive way: 
- BZT
    - BZT-NewYork
        - BZT-NY-Linux
            - BZT-NY-Linux-Apache
            - BZT-NY-Linux-Gizmotrax
        - BZT-NY-Windows
            - BZT-NY-Win-IIS
    - BZT-LA
        - BZT-LA-K8s
        - etc.

Other guidelines to keep in mind when naming your Fleets:

- Fleet names can't have spaces in them; use a dash or underscore instead.
- You can't reuse a Fleet name.
- Once you create a Fleet, you can't edit the name.
- You can't delete a Fleet if there are Subfleets under it.

## Inheritance Considerations {#inheritance} 

As suggested above in [Designing Your Fleet Hierarchy](#hierarchy), plan and design the Fleets' inheritance in advance. Here are some guidelines to keep in mind, as you think about layering your configurations. 

### Top-level Fleets (Grandparents)

Assign the top-level Fleets for company-wide configurations. Consider: 
- Applying certificates and other company-wide settings at this level.
- Avoid OS-specific or site-specific configurations at this level.

### Subfleet 1 (Parents)

Per-site configurations. Consider:
- Configure site-specific Sources/Destinations at this level.
- Specify any site-specific configurations, like common Destinations.

### Subfleet 2 (Children)

Configurations used by groups of similar systems. Consider:

- Apply OS-specific configurations at this level. For example, an “All Linux” and “All Windows” for a given site.
- Group common OS-specific metrics and common log files into OS-specific Fleets. 

### Subfleet 3 (Grandchildren)

Apply function-specific Subfleets to add specific metrics, logs, and/or Windows Event sources. For example, you can group by:

- Apache HTTPD
- Database
- SMTP servers
- Active Directory
- IIS
- Exchange 

### Modify or Remove Inherited Subfleet Settings

If you make a change to an inherited config (such as Sources/Destinations) in a Subfleet, the Subfleet will no longer get updates made to the parent Fleet for that inherited config. Other configs are still inherited from that parent. 

However, if you **remove** an inherited config entry from the Subfleet (for example, remove a Source definition),
the Subfleet will inherit it again when you next commit the configuration of parent Fleet.

When you make changes to a parent Fleet configuration, use the **Commit and Deploy with Subfleets** option. This way, you are always keeping the inherited configurations in sync.
