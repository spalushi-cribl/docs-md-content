# Upgrading Edge on Windows via the UI


Upgrade Cribl Edge Nodes on Windows via the UI.

---

There are two ways to upgrade Edge Nodes installed on Windows, depending
on the version of Edge you're running:

| Cribl Edge Version | Upgrade Method |
|--------------------|----------------|
| Version 4.3.1 and newer | Upgrade Windows Nodes from the Leader [via the UI](upgrading-ui-nodes), or by re-running a Windows installer from the filesystem (manually upgrading). |
| Version 4.3.0 and older | Re-run a Windows installer from the filesystem. Upgrades, backups, and rollbacks, are not supported in the UI for Cribl Edge versions 4.3.0 and older. For details, see [Manually Upgrading Windows Edge Nodes](upgrading-windows-manually) |

## Upgrading Windows Edge Nodes via the UI

Upgrade Windows Edge Nodes that are running version 4.3.1 and newer
[via the UI](upgrading-ui-nodes).

### Specific Upgrade Considerations for Windows Environments {#windows-upgrades}

- **Endpoint Security Interference**: If you are having trouble installing or upgrading Cribl Edge, check for antivirus or other endpoint security tools that may be blocking the cribl process (like Windows Defender). See workaround: Common Challenges on the Edge | Cribl Docs.

- **ThreatLocker might block Microsoft Edge installation on Windows 11**: This causes silent failures. Contact your ThreatLocker admin to check for SYSTEM account blocks. They can create exceptions for Edge installations.

For details, see [Debugging Cribl Edge on Windows](edge-common-challenges#debugging-cribl-edge-on-windows).