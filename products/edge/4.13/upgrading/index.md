# Upgrading Overview


Learn about the ways you can upgrade Cribl Edge, as well as general and version-specific
upgrade considerations.

---
## Ways to Upgrade Cribl Edge

When you have deployed Cribl Edge on-prem, you choose how to upgrade Cribl Edge
through one of these methods:

- [Upgrade from the UI](upgrading-ui-leader) to upgrade the Leader and set a target version for each Fleet.
- [Upgrade Manually](upgrading-manually) using a command-line interface to upgrade Edge by uncompressing a new version on top of the old one.
- [Upgrade Containers](upgrading-containers) running Cribl Edge using [Docker](deploy-running-docker) or [Kubernetes](deploy-running-kubernetes).

## Upgrading Requirements and Caveats

Here are some considerations to review before you upgrade.

- Upgrading Edge Nodes from the Leader is available on certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).

- Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
  a Cribl-provided test candidate) to a GA version. To get the GA version running,
  you must perform a new install. 

- Review [Edge and Leader Nodes Compatibility](upgrading-troubleshooting#compatible).

- Before upgrading your Leader to v.4.0 and newer, see
  [Persisting Socket Connections](setting-up-leader-and-edge-nodes#socket) to
  prepare the host to keep communications open from Edge Nodes. 

- Cribl Edge 4.1 and newer encrypts TLS certificate private key files when you
  add or modify them. See [instructions](upgrading-troubleshooting#safeguarding) for backing up your keys from
  earlier versions.

### Supported Version Differences Between Leader and Edge Nodes

Cribl strongly recommends keeping Leader Nodes and Edge Nodes on the same version for the best experience and to ensure access to all features.
Upgrading a Leader Node to a newer version may surface configuration options in the UI
that will not be functional until you upgrade the connected Edge Nodes to the same version.

**Keep Reading**

- [Better Practices for Upgrading Cribl Edge Nodes](upgrading-better-practices)
- [Troubleshooting Cribl Edge Upgrades](upgrading-troubleshooting)