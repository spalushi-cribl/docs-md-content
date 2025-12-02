# Upgrading Overview


Learn about the ways you can upgrade Cribl Edge, as well as general and version-specific
upgrade considerations.

---
## Ways to Upgrade Cribl Edge

You can upgrade Cribl Edge in two ways:

- [Upgrade from the UI](upgrading-ui).
- [Upgrading Manually](upgrading-manually) allows you to upgrade by
  uncompressing a new version of Edge on top of the old one. 

## General Upgrade Considerations

Here are some considerations to review before you upgrade.

- Upgrading Edge Nodes from the Leader requires a Standard or Enterprise [license](licensing).

- Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
  a Cribl-provided test candidate) to a GA version. To get the GA version running,
  you must perform a new install. 

- Before upgrading your Leader to v.4.0 and later, see
  [Persisting Socket Connections](setting-up-leader-and-edge-nodes#socket) to
  prepare the host to keep communications open from Edge Nodes. 

- Cribl Edge 4.1 and later encrypts TLS certificate private key files when you
  add or modify them. See instructions just below for backing up your keys from
  earlier versions.

## Leader and Edge Nodes Compatibility {#compatible}

Leaders on v.4.2.x are compatible with Edge Nodes on v.4.1.2, v.4.1.3, and
later. Due to a security update, Edge Nodes running on v.4.0.4 cannot receive
configurations from v.4.2.x Leaders.

### Upgrading to v.3.5.4

If you’re planning to upgrade to v.3.5.4, note that any Edge Nodes must be at
the same version as the Leader for v.3.5.4. Leaders running v.3.5.4 and later
should test whether Edge Nodes are running a compatible version before deploying
configs that could break Edge Nodes' data flow. The Leader will prompt you to
upgrade these Nodes as needed.

To prevent Edge Nodes from failing on incompatible configurations, we introduced
three new behaviors announced in Cribl Edge 3.5.4 release notes. These will make data
throughput more resilient, but might enforce more-frequent (automated or
explicit) upgrades to Nodes' Edge versions:

- The Edge Leader will block configuration deployments to Edge Nodes running an
  earlier Edge version that's incompatible with the new configs. The Leader will
  prompt you to upgrade these Nodes to a compatible version.
- If Edge Nodes detect a Source or Destination type for which they have no
  supporting configuration, they will ignore (skip initializing) that
  integration, rather than failing.
- For unsupported Destination types, Nodes will create a temporary placeholder
  Destination, but will exert Blocking backpressure until the configuration is
  updated. You will see a warning of the form: `Skipping configuring
  Destination...due to unrecognized type`.

## Safeguarding Unencrypted Private Keys for Rollback {#safeguarding}

Cribl Edge 4.1 and later encrypt TLS certificate private key files when you add
or modify them. 

> Before upgrading from a pre-4.1 version, make a backup copy of all
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
private key files. If you need to roll back to a pre-4.1 version, see [Restoring Unencrypted Private Keys](securing-and-monitoring#restoring).

**Keep Reading**

- [Upgrading via UI](upgrading-ui)
- [Upgrading Manually](upgrading-manually)
- [Troubleshooting Cribl Edge Upgrades](upgrading-troubleshooting)