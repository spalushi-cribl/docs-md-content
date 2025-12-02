# Upgrade Cribl Edge


Learn about the ways you can upgrade Cribl Edge, as well as general and version-specific
upgrade considerations.

---

When upgrading a Cribl Edge deployment, the general order of steps is:

1. [Upgrade the Leader Node](upgrade-leader).
1. [Upgrade Outpost Nodes](manage-outpost#upgrade), if present.
1. [Upgrade Edge Nodes](upgrade-nodes).
1. Commit and deploy the changes on the Leader.

> If your deployment uses a [second Leader](deploy-add-second-leader),
> refer to [Upgrading with a Second Leader](upgrade-leader#second-leader).
{.box .info}

## Prepare for an Upgrade {#prepare}

Before upgrading the Leader, plan your whole upgrade strategy,
in particular whether you want to upgrade all your Edge Nodes at the same time.
Edge Nodes will not upgrade automatically when you upgrade the Leader.
In particular:

- Revise [Better Practices](upgrading-better-practices) for things to consider before starting the upgrade.
- Ensure you have configured [Backup and Rollback](upgrading-ui-rollbacks).
- If you are upgrading older Cribl Edge versions, pay attention to the special cases described in [Troubleshooting Edge Upgrades](upgrading-troubleshooting#older).

### Supported Version Differences Between Leader and Edge Nodes

Cribl strongly recommends keeping Leader Nodes and Edge Nodes on the same version for the best experience and to ensure access to all features.
Upgrading a Leader Node to a newer version may surface configuration options in the UI
that will not be functional until you upgrade the connected Edge Nodes to the same version.

### Upgrade from Non-GA Versions

Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
a Cribl-provided test candidate) to a GA version. To get the GA version running,
you must perform a new install. 
