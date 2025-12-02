# Scaling Edge Beyond 3k Nodes

Learn how to scale Cribl Edge deployments to support one Leader managing up to 50,000 Nodes. 

---

Our standard [Deployment Planning](deploy-planning) guide is a great place to
start if you intend to deploy 3,000 or fewer Nodes in your Cribl Edge environment.

In version 4.6 and newer, a Leader can support a maximum of 50,000 Edge Nodes. 
To support this higher Node limit, we've made some changes to Cribl Edge.

This guide covers: 

- The changes you can expect to see in the Edge UI.
- Some areas where you might encounter slowness as we continue working on
  supporting more Nodes.
- The specific Leader configuration we’ve tested for best performance.

## What to Expect in the User Interface

We’ve updated the UI to help visually and behaviorally support Fleets that contain more than
3,000 Nodes. This section will highlight the key changes, and alert you to potential issues
you may encounter.

### Searching for Nodes in the Explore Tab

Typing into the [**Node to explore**](explore-edge#explore-tab) field returns
only the first 100 matching Nodes, sorted alphabetically by hostname.

### Previewing Node Mappings

When you’re previewing Node mappings from the **Manage** menu, **Mappings** submenu, the
results are limited to the first 10,000 Mappings.

### Viewing the Status Tab on a Source or Destination

When there are more than 50 Nodes in a Fleet, the **Status** column won't be
visible in the list of Nodes (from a Source or Destination).

To view the status for an individual Node, expand it in the list. You can only
expand one Node at a time. Refresh the list of Nodes and their status with the
**Refresh** button.

## Recommended Leader Configuration

In general, these are the settings you should configure on the Leader in order for
it to be able to manage more than 3,000 Nodes:

- Deploy one CPU for every 3,000 Edge Nodes. To scale to 50,000 Nodes, you might
  need to deploy around 16 CPUs (depending on your specific workload).
- Configure one [Connection Process](deploy-planning#connection) for every 10,000 Edge Nodes.
- While one Edge Fleet _can_ contain all 50,000 Nodes, we've seen slowdown and
  performance issues in testing. Do not push new configurations to more than one
  Fleet at a time.
- Increase memory to 32GB.
- As a starting point, increase the RAM of the Leader instance's physical host
  by 200MB RAM per Edge Fleet. Continue to increase memory per Edge Fleet if
  needed.
- In the current release, each Leader can support a maximum of 50,000 Edge Nodes.
- In **Fleet Settings**, [configure internal metrics](internal-metrics#edge-metrics-controls)
  to **Minimal**. See [Performance Considerations](deploy-planning#performance-considerations)
  for more information.
- There is no change to the default values when pushing config bundles from the Leader.

> [Cribl University](https://cribl.io/university/) offers a course titled [Cribl Edge
> Architecture: Architecture & > Sizing](https://university.cribl.io/#/online-courses/b8a0d47f-544c-49e2-b175-9253da90b2e8)
> that provides additional best practices and guidance. To follow the direct course link, first
> log into your Cribl University account. (To create an account, select the **Sign up** link.
> You’ll need to click through a short **Terms & Conditions** presentation, with chill music,
> before proceeding to courses – but Cribl’s training is always free of charge.) Once logged
> in, check out other useful [Cribl Edge
> courses](https://university.cribl.io/#/catalog/f397da65-f17d-4c43-b147-5fff8b8c5876).
>
{.box .info}

## Upgrading 50,000 Edge Nodes

We recently rolled out a [new upgrading framework](upgrading-ui) in which Edge Nodes asynchronously pull down
upgrade packages from the Leader, rather than the Leader pushing packages to Nodes all at one
time. Nodes communicate to the Leader every 60 seconds via a heartbeat.

Additionally, Nodes request upgrade packages from the Leader in batches of 500. This
allows you to upgrade Nodes without overloading the Leader, even with a large number of Nodes.
In our controlled test environment, we found that upgrading 50,000 Nodes can take anywhere from
25-40 minutes. This is an expected time frame to upgrade that number of Nodes.

## Current Limitations

We’re continuing to address places where we’ve noticed an altered UI experience.
This is what we’ve identified so far:

- **Logs**: The drop-down where you can select an Edge Node will display a maximum of 100 Nodes. 
- **Map view**: To improve performance, the Map view doesn't load all Nodes.
- **Import Edge Data**: The **Import Edge Data** window in Cribl Stream returns a
  maximum of 100 Edge Nodes. 
- **Export list as**: It might take longer than normal to export the list when
  the list contains a large number of Nodes.  
- **Test tab**: The **Test** tab on Destinations displays a maximum of 100 results. 
- **KMS Status report**: In **Fleet Settings**, the **KMS Status report** (under
  **Security**) does not display all Nodes. Instead, search for your desired
  Node.