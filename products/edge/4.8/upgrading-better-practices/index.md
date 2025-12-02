# Better Practices for Upgrading Cribl Edge


Streamlining the Upgrade Process

---

While Cribl Edge is designed to provide an easy way to upgrade the version of Cribl Edge on your nodes, we recognize that everyone’s environment and requirements are different. Here are some recommendations for planning upgrades in your environment either through our Fleet-level upgrades, or when using other deployment tools.

## Plan Your Upgrade{#analyze}

A successful Cribl Edge upgrade requires careful planning. Consider these steps to ensure a smooth transition.

### For your Environment
- Identify your current Cribl Edge version. Determine your Cribl Edge version by running the command `./cribl version` in your terminal or by checking the upper right corner of the Cribl Edge UI landing page.
- Evaluate changes or updates in your OS that could interfere with the upgrade, such as any software that might block connections. Ensure the Cribl process is allowlisted and permitted to execute without limitations to facilitate a smooth installation.
- Coordinate with your Security/IT team. Before deploying Cribl Edge on Nodes, consult with your security or IT department. They can advise on any necessary approvals or specific deployment guidelines you need to follow.

### For your Deployment

- Review [Leader and Edge Nodes Compatibility](upgrading-troubleshooting#compatible).
- Review the [Release Notes](release-notes) for the version you want to upgrade to and any previous versions you might have skipped.
- Check the [Known Issues](known-issues) page for anything might impact your environment, including compatibility issues with your OS, third-party applications, or configurations. Additionally, assess potential issues with [Sources](sources#monitoring-source-status), Destinations, and other configurations.
- If you initially installed Cribl Edge using containers, refer to the [Upgrading Containers](upgrading-containers) documentation for details.

## Decide on an Upgrade Strategy {#strategy}

For organizations using Leader-initiated updates, Fleets offer a valuable tool for managing upgrades. By categorizing Edge Nodes into different Fleets based on upgrade schedules or risk tolerance, you can control the upgrade process with greater precision. If you are using other upgrade management tools, using Fleets this way can still be beneficial, but their impact might be less pronounced.

Consider organized your Fleets into: 

- **Mission-critical Edge Nodes**: To prioritize software stability and minimize downtime for these critical Edge Nodes, organize them into Fleets that will follow a longer upgrade cycle.

- **Non-critical Edge Nodes**:  Organize non-critical Edge Nodes into Fleets that will have a more frequent upgrade cycle (every two to three months, for example), to balance feature adoption and system stability.

- **Early-Adopter Edge Nodes**: To evaluate new releases, consider creating dedicated test Fleets comprised of Edge Nodes suitable for testing. These Fleets can be upgraded frequently.

Configure Fleets to match your desired upgrade approach. See [Fleet Configuration](upgrading-ui-nodes#ui-upgrades) for details.

## Establish an Appropriate Window For Upgrades and Maintenance {#maintenance-windows}

Consider acceptable downtime and maintenance windows:
- Edge Node upgrades will require a restart of the Edge process. Consider organizing Fleets in a way that accounts for rolling upgrades and avoid potentially concurrent downtime.
- Note that Edge Node upgrades occur in batches of 500, so large Fleets may require longer upgrade times.
- Coordinate with Edge Node owners to schedule upgrades that minimize service interruptions, taking into account time zone variations.
  
## Roll Out Fleet-Level Upgrades for Large Deployments {#large-deployments}
Fleet-level upgrades involve updating multiple Edge Nodes simultaneously. This approach is ideal for larger deployments where managing individual upgrades would be impractical.

### Steps for Fleet-Level Upgrades

- **Staged rollout**: Start your Cribl Edge upgrade in a designated test Fleet or SubFleet (as a pilot group) to minimize risk and identify issues before impacting production. Or consider separating your dev/testing environment from your production. 
- **Asynchronous upgrades**: Use the upgrading framework to allow Nodes to request upgrade packages from the Leader in batches, reducing load on the Leader.
  - Nodes communicate to the Leader via heartbeat every 60 seconds.
  - Upgrade packages are requested in batches of 500.

- **Error handling**: Look for informative error messages in logs such as `msiexec`, the Windows Event Log, and `cribl.log`. Log files are located in the `$CRIBL_HOME/log/` directory:
  - Server/API (Leader logs): `C:\ProgramData\Cribl\log\cribl.log`: Server/API (Leader)
  - Edge Node logs: `C:\ProgramData\Cribl\log\worker\0\cribl.log`

## Monitor the Upgrade Status {#status}

For details, see [Fleet Upgrade Information](upgrading-ui-status).

## Specific Considerations {#consider}

In addition, review the considerations for each of the applicable scenarios below:

- [Specific considerations for UI-based upgrades](upgrading-ui-nodes#ui-upgrades)
- [Specific considerations for upgrading containers](upgrading-containers#container-upgrades)
- [Specific considerations for Edge Nodes deployed via RPM Packages](upgrading-manually-linux-rpm)
- [Specific considerations for upgrading Windows Environments](upgrading-windows#windows-upgrades)

**Keep Reading**

- [Upgrading via UI](upgrading-ui)
- [Upgrading Manually](upgrading-manually)
- [Upgrading Containers](upgrading-containers)
- [Troubleshooting Cribl Edge Upgrades](upgrading-troubleshooting)