# Upgrading Manually Overview


Learn how to upgrade Edge Nodes manually using a command line interface (CLI). 

---

In this guide, we'll cover benefits to upgrading manually and how to use the
command line to upgrade your Edge Nodes. 

> **A Word About Words**
> 
> We will use "upgrading manually" and "on the command line / CLI" interchangeably in this article.
>
{.box .info}

## Why Upgrade Cribl Edge Manually?

With Edge, you have the option to upgrade manually or [via the UI](upgrading-ui-leader). Here are some
use cases and benefits to using the command line to upgrade:

### Flexibility and Control

Manually upgrading means you can pause and resume upgrades as needed,
which gives you more granular control over the process.

You can also upgrade individual Nodes separately. This is useful when you're
testing upgrades on a subset of Nodes before rolling out to the entire
deployment.

### Air-Gapped Environments and Custom Deployments

Consider upgrading via CLI when your organization can't connect to the internet
for security or compliance reasons. You don't need to be online to upgrade on
the command line.

When Cribl Edge is deployed in non-standard ways, such as via RPM package,
within containers, or using a container orchestrator like Kubernetes, manual
upgrades might be necessary.

### Specific Upgrade Paths

When you upgrade Edge Nodes on a CLI, you don't have to worry about a version
upgrade path - there are no restrictions on the versions you can upgrade to or
from.

**Keep Reading**

- [Upgrading Cribl Edge on Linux (CLI)](upgrading-manually-linux)
- [Upgrading via RPM](upgrading-manually-linux-rpm)
- [Manually Upgrading Cribl Edge on Windows](upgrading-windows)
- [Troubleshooting Cribl Edge Upgrades](upgrading-troubleshooting)