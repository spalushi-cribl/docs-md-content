# Troubleshooting Edge Upgrades


Find solutions to common issues you can encounter when upgrading Cribl Edge.

---

While we expect you'll have an easy time upgrading Cribl Edge, here is the
troubleshooting information we've curated in case you do experience a hiccup:

## Direct Upgrades

Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
a Cribl-provided test candidate) to a GA version. To get the GA version running,
you must perform a new install. 

## Leader and Edge Nodes Compatibility {#compatible}

Upgrading a Leader Node to a newer version may surface configuration options in the UI
that will not be functional until you upgrade the connected Edge Nodes to the same version.

Do not upgrade Edge Nodes to a newer version than the Leader Node.

## Upgrade to Older Versions {#older}

Take a look at the following information if your Edge Nodes are running older software versions.

### Upgrade to 4.2.x {#upgrade-42}

Leaders on 4.2.x are compatible with Edge Nodes on 4.1.2, 4.1.3, and
later. Due to a security update, Edge Nodes running on 4.0.4 cannot receive
configurations from 4.2.x Leaders.

### Upgrade to 4.1.x or Newer

Cribl Edge 4.1 and newer encrypts TLS certificate private key files when you
add or modify them. See [instructions](safeguarding) for backing up your keys from
earlier versions.

### Upgrade to 4.0.x or Newer

Before upgrading your Leader to 4.0 and newer, see
[Persisting Socket Connections](setting-up-leader-and-edge-nodes#socket) to
prepare the host to keep communications open from Edge Nodes. 

## Safeguard Unencrypted Private Keys for Rollback {#safeguarding}

Cribl Edge 4.1 and newer encrypt TLS certificate private key files when you add
or modify them. 

> Before upgrading from a version older than 4.1, make a backup copy of all
> unencrypted TLS certificate private key files. Having access to the unencrypted
> files is essential if you later find that you need to roll back to your previous
> version. 
>
{.box .warning}

To safeguard your unencrypted private keys, make a full backup of all Cribl
config files. Files in the `auth/certs` directory are particularly important,
such as those in:

- `groups/default/local/cribl/auth/certs/`
- `groups/<groupname>/local/cribl/auth/certs/`
- `cribl/local/cribl/auth/certs/`
-  Etc.

Take appropriate precautions to prevent unauthorized access to these unencrypted
private key files. If you need to roll back to a version older than 4.1, see [Restoring Unencrypted Private Keys](securing-encryption-keys#restoring).

