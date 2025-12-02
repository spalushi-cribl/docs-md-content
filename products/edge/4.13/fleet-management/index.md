# Manage Fleets of Edge Nodes


Get started with managing Fleets of Edge Nodes in a Distributed deployment.

---

In a distributed environment, a single Leader Node can centrally manage Edge Nodes. 
The Leader is responsible for keeping configurations in sync, 
tracking Edge Node status, and monitoring metrics. 

## Concepts

**Single Edge Node** – a single Cribl Edge instance, running as a standalone (not distributed) installation on one server/endpoint.

**Leader Node** – an instance running in **Leader** mode, used to centrally author configurations and monitor Edge Nodes in a distributed deployment.

**Managed Edge Node** – an instance running as a **Managed Edge**, whose configuration is fully managed by a Leader Node. (By default, will poll the Leader for configuration changes every 10 seconds.)

**Fleet** – a collection of Edge Nodes that share the same configuration. You map Nodes to a Fleet using a Mapping Ruleset.

**Subfleet** – a Subfleet groups and manages Edge Nodes that share the same configuration. Each Subfleet inherits configurations from its parent Fleet. Updating and deploying parent-level configurations applies the changes to the Subfleets. The Subfleets then deploy the configuration changes to their Edge Nodes.

**Worker Process** – a Linux process within a Single Instance, or within Edge Nodes, that handles data inputs, processing, and output. The process count for Edge Nodes is constrained to 1.

**Mapping Ruleset** – an ordered list of filters, used to map Nodes into Fleets.

Multiple Fleets are very useful in making your configuration reflect organizational or geographic constraints. E.g., you might have a U.S. Fleet with certain TLS certificates and output settings, versus an APAC Fleet and an EMEA Fleet, each with their own distinct certs and settings.

## Architecture

This is an overview of a distributed Edge deployment's basic components. (The type/s of Nodes within each Fleet will vary depending on your overall architecture:)

![Distributed deployment architecture](edge-fleet-management.c74a757fdf.jpg)
{border="false"}

### Leader Node
  - API Process – Handles all the API interactions.
  - *N* Config Helpers – One process per Fleet. Helps with maintaining configs, previews, etc.

### Edge Node
  - Single Process – Handles everything; collection, processing and communication with the Leader Node.

## Leader Node Requirements

* **OS**: Linux: RedHat, CentOS, Ubuntu, AWS Linux (64bit)
* **System**: +4 physical cores, +8GB RAM, 5GB free disk space
* **Network**: 
  * Heartbeat Port: `4200`: managed Edge nodes communicate with the Leader Node on port `4200` by default.
  * UI/API Port: `9000`: users acccess the Leader on port `9000` by default. 
* **Git**:  `git` must be available on the Leader Node. See details [below](#git).
* **Browser Support**: The five most-recent versions of Chrome, Firefox, Safari, and Microsoft Edge.

> We assume that 1 physical core is equivalent to 2 virtual/hyperthreaded CPUs (vCPUs). All quantities listed above are minimum requirements. We recommend deploying the Leader on stable, highly available infrastructure, because of its role in coordinating all Edge instances.
>
{.box .info}

## Edge Node Requirements

See [Deployment Planning](deploy-planning) for requirements and other details. 

## Installing on Linux

See [Installing on Linux](deploy-linux).

## Installing on Windows 

See [Installing on Windows](deploy-windows).

## Version Control with `git` {#git}

Leader Node requires `git` (version 1.8.3.1 or higher) to be available locally on the host.

**Configuration changes must be committed to git before they're deployed.** 

If you don't have `git` installed, check [here](https://git-scm.com/download/linux) for details on how to get started. 

The Leader node uses `git` to: 
* Manage configuration versions across Fleets. 
* Provide users with an audit trail of all configuration changes.
* Allow users to display diffs between current and previous config versions.