# Upgrading Overview


Learn about the ways you can upgrade Cribl Edge, as well as general and version-specific
upgrade considerations.

---
## Ways to Upgrade Cribl Edge

You can upgrade Cribl Edge through these methods:

- [Upgrade from the UI](upgrading-ui) to upgrade the Leader and set a target version for each Fleet.
- [Upgrade Manually](upgrading-manually) using a command-line interface to upgrade Edge by uncompressing a new version on top of the old one.
- [Upgrade Containers](upgrading-containers) running Cribl Edge using [Docker](deploy-running-docker) or [Kubernetes](deploy-running-kubernetes).

## Upgrading Requirements and Caveats

Here are some considerations to review before you upgrade.

- Upgrading Edge Nodes from the Leader requires a Standard or Enterprise [license](licensing).

- Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
  a Cribl-provided test candidate) to a GA version. To get the GA version running,
  you must perform a new install. 

-  Review Edge and Leader Nodes Compatiblity.

- Before upgrading your Leader to v.4.0 and later, see
  [Persisting Socket Connections](setting-up-leader-and-edge-nodes#socket) to
  prepare the host to keep communications open from Edge Nodes. 

- Cribl Edge 4.1 and later encrypts TLS certificate private key files when you
  add or modify them. See [instructions](upgrading-troubleshooting#safeguarding) for backing up your keys from
  earlier versions.


**Keep Reading**

- [Better Practices for Upgrading Cribl Edge Nodes](upgrading-better-practices)
- [Troubleshooting Cribl Edge Upgrades](upgrading-troubleshooting)