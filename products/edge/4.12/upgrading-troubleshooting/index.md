# Troubleshooting Edge Upgrades

In case of upgrading emergency, break glass.

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

### Upgrading to 4.2.x {#upgrade-42}

Leaders on 4.2.x are compatible with Edge Nodes on 4.1.2, 4.1.3, and
later. Due to a security update, Edge Nodes running on 4.0.4 cannot receive
configurations from 4.2.x Leaders.

### Upgrading to 3.5.4 or Later {#later-versions}

- If you’re upgrading to 3.5.4 or later, all Edge Nodes will need to be on the
  same version as the Leader. Leaders running 3.5.4 and newer test whether
  Edge Nodes are running a compatible version before deploying configs that could
  break Nodes' data flow. The Leader will prompt you to upgrade these Nodes as
  needed.

- Version 3.5.4 is also a compatibility breakpoint for the Cribl HTTP
  [Source](sources-cribl-http) and [Destination](destinations-cribl-http), and
  for the Cribl TCP [Source](sources-cribl-tcp) and
  [Destination](destinations-cribl-tcp). When running on Cribl Edge 3.5.4 and
  later, these two Sources can send data only to Workers running 3.5.4 and
  later, and these two Destinations can receive data only from Nodes running
  3.5.4 and newer. When running on Cribl Edge 3.5.3 and older, these four
  integrations can similarly interoperate only with Nodes running 3.5.3 and
  older.

## Safeguarding Unencrypted Private Keys for Rollback {#safeguarding}

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

