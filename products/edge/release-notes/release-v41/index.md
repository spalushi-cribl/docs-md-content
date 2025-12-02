# Cribl Edge 4.1


## New Features 

This release provides the following improvements:

- Expanded container support: Cribl Edge now supports `containerd` runtime in the Processes and Containers UI. 
- A new Journal Files Source collects data from systemd's centralized logging, the `journald` service.
- You can now use a single installer for silent installs and UI installs on Windows. Other improvements to the installer include storing installation files in a different location than modified data, which reduces errors. You also have options to persist configuration and install Cribl Edge as a service.
- Health dashboards for Sources, Destinations, Routes and Pipelines are now available at a Fleet/SubFleet level. These metrics are configurable at a Flee/Subfleet level. Earlier, these metrics were available on the Edge Node when you teleport into it. See [Performance Considerations](deploy-planning#performance-considerations) for configuration guidance. 
- A new Kubernetes Events Source emits cluster-level events. These events are generated automatically in response to state changes or errors with nodes, pods, or containers.
- The Cribl Edge System State Source has several new collectors including Network Interfaces, Disks & Filesystems, DNS Resolvers, Local Users & Groups, and Metadata.
- Add/Update Edge Node UI now supports Docker, Kubernetes, and Windows.  
- You can now deploy to Fleets and Subfleets with one click. Be aware of the [performance issues](deploy-planning#performance-considerations) that could occur when deploying too many Edge Nodes at the same time. 
- Cribl TCP/HTTP Destinations now exclude `__metadata` and `__kube_*` properties by default, to reduce the amount of traffic sent upstream. 

## Breaking Changes

> **Leader upgrades to v.4.1.x require Edge Nodes to also upgrade to v.4.1.x, for feature compatibility.** If you haven't enabled automatic upgrades of customer-managed (on-prem or hybrid) Edge Nodes, be sure to upgrade all Edge Nodes before deploying changes from a v.4.1.x Leader. Deploys to Edge Nodes on versions older than 4.1 will silently hang.
> 
> **TLS certificate private keys within Cribl configuration files are now encrypted.** Before you upgrade to v.4.1.x and add new private keys, back up your existing unencrypted keys. Otherwise, you will be unable to roll back to older versions, because the new keys are not backward-compatible. (CRIBL-13535)
> 
> **Source-side Persistent Queues now default to** `Always on` **mode.** If you have code that automates creating Cribl Sources, while assuming the previous default `Smart` mode, you'll need to update your code to preserve the existing behavior. This change does **not** affect existing Sources' configurations. (CRIBL-15565)
>
{.box .warning}

## Corrections 

This release includes the following fixes:



- [CRIBL-15891] The Event Breaker that the Windows Event Log Source was using has been removed from **Knowledge** > **Event Breakers**. The Event Breaker was not supposed to be configurable and has been removed to prevent errors. 
- [CRIBL-11497] and [CRIBL-11585] Diag files and contents in Windows are named correctly now. 


