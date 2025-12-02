# Upgrading Edge Nodes via UI


Learn how to upgrade Edge Nodes via the UI. 

---

## Upgrading Edge Nodes {#target}

You can upgrade the Nodes in a Fleet from the **Manage > Fleets**
page. Upgrade options are on a per-Fleet basis:

- Choose **None** to never upgrade any of the Nodes in the Fleet.
- Select a target software version to upgrade all of the eligible Nodes in the
  Fleet to the selected version.

To upgrade the Nodes in a selected Fleet to a target version:

1. Go to **Manage > Fleets**.
2. In the **Target Software Version** column, click the current version (or **None**, if applicable).

![New Fleet upgrade interface](target-software-version.png)

3. Click the **Upgrade target version** drop-down and select a version.

![Target version window](target-version-modal.png)
{scale="50%"}

The next time the Node in the selected Fleet connects to the Leader, the Node
will request the selected target version, and the Leader will upgrade the Node
to that version. 

Nodes in the selected Fleet will not upgrade to any future
version until you select or change the target software version in the drop-down
(even if you upgrade the Leader).

If you set the target software version to **None**, the Nodes in the selected
Fleet will never upgrade. This lets you keep Edge Nodes at a specific version,
even if you upgrade the Leader.


> The **Add/Update Edge Node** command uses the **Target Software Version** for your Fleet to improve update accuracy. This applies to all supported environments: Docker, Kubernetes, Linux, and Windows. For details, see [Edge Node Updates Using Fleet Version Settings](managing-edge-nodes#fleets-target).
{.box .info}

### Specific Considerations for UI-Based Upgrades {#ui-upgrades}

To choose the appropriate **Target Software Version** option consider the following scenarios. 

- **None**: The Nodes in the Fleet will never be upgraded automatically. This option is suitable for highly critical Edge Nodes or Nodes managed by other departments that require strict control over upgrades.

- **Match Current Leader version**: The Nodes will be upgraded to match the software version of the currently running Leader Node. Once a Fleet is upgraded, it remains pinned to that specific version until:

  - The Leader itself is upgraded to a newer version.
  - An administrator manually intervenes and adjusts the target version for the fleet to the new leader version.
  
This option is useful for Fleets where you want to synchronize the Edge Nodes with the Leader's version to ensure compatibility and leverage the latest features.

### Subfleets

Subfleets do not inherit the **Target software version** from their parent
Fleet, but you can set the target software version for each Subfleet using the
same steps you used in the parent Fleet.