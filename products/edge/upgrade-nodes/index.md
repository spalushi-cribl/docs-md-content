# Edge Node Upgrades


Upgrade Edge Nodes automatically or manually.

---

There are two ways to upgrade Edge Nodes: in bulk together with the whole Fleet,
and manually, per Edge Node.

> Edge Nodes in containers undergo a different upgrade process.
> Refer to [Upgrading Containers](upgrading-containers) for more information.
{.box .info}

## Fleet Upgrades

Upgrading Edge Nodes in bulk with their Fleet is the more straightforward of the two upgrade options,
and manageable completely from the Leader Node UI.
With this method, you set the desired target version for all Edge Nodes in a given Fleet.
All Edge Nodes will then upgrade automatically to the target version.

See [Upgrade Edge Nodes in a Fleet](upgrade-nodes-fleet) for more details.


> Upgrading Edge Nodes from the Leader is available on certain plan/license tiers.
> For details, see [Pricing](https://cribl.io/pricing/).
{.box .info}

Automatic Edge Node upgrades have the following limitations:

- You can't automatically upgrade Edge Nodes installed via RPM. Refer to [Upgrade RPM Installations](upgrade-nodes-manually#rpm) for the manual upgrade process for those Nodes.
- You can't automatically upgrade Edge Nodes on Windows running version 4.3.0 or older. Use [manual upgrade](upgrade-nodes-manually) instead.
- Upgrading a Single-instance deployment requires a manual upgrade.

## Manual Upgrades

Upgrading Edge Nodes manually requires running the upgrade process on the CLI for each Node.

Manual upgrades let you pause and resume upgrades as needed, which gives you more granular control over the process.
You can also upgrade individual Nodes separately.
This is useful when you're testing upgrades on a subset of Nodes before rolling out to the entire deployment.

Consider upgrading via CLI when your deployment is airgapped for security or compliance reasons
and can't download new packaged from the internet.

See [Upgrade Edge Nodes Manually](upgrade-nodes-manually) for more details.

> When upgrading Edge Nodes on Windows, consult [Debugging Cribl Edge on Windows](edge-common-challenges#debugging-cribl-edge-on-windows).
{.box .info}
