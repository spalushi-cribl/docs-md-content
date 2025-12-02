# Cribl Edge 4.5


Welcome to the February release of Cribl Edge! We haven't forgotten your
Valentine's Day gift – here are the shiny, all-new features developed just for
you.

> ## Deprecation Notice: End of Support for CentOS 6 
> **Action Required**
> 
> In 4.4.4, we deprecated support for CentOS 6 for on-prem installations of
> Cribl Stream and Edge. This does not affect Cribl.Cloud. 
> 
> If you are currently running Cribl Edge Nodes on CentOS 6, you must upgrade
> your operating system to at least CentOS 7 or later before upgrading to version
> 4.5. We have removed support in the 4.5 release, and Edge Nodes upgrading to
> version 4.5 will no longer work with CentOS 6.
{.box .danger}


## New Features 

This release provides the following improvements:

### New Way to Upgrade Nodes in Your Fleets in the UI

We're introducing a new way to upgrade Edge Fleets in the UI. This new upgrade
method replaces the **Upgrade** button. To learn how to upgrade Fleets in
versions 4.5.0 and newer, read our [Upgrading
Fleets](upgrading-ui#upgrading-edge-nodes) documentation.

This change improves the efficiency, reliability, and stability of Fleet
upgrades as you add more Nodes and scale your deployment.

#### Selecting a Target Software Version

You'll now manage upgrades in the UI on a per-Fleet basis from **Manage > Fleets** 
(instead of upgrading Fleets from **Edge Settings > Fleet Upgrade**).

![New Fleet upgrade interface](target-software-version.png)
{border="true"}

For each Fleet, you can choose to set the target version for your Fleet and the
Leader will ensure Nodes are upgraded to that version. Or, you can select
**None** (or clear the field) and choose to not upgrade Nodes in the selected
Fleet.

![Target version window](target-version-modal.png)
{scale="50%" border="true"}

#### Notable Upgrading Changes

- On the Manage Fleets page, you can now set Nodes in a Fleet to upgrade
  themselves by selecting a target upgrade version per Fleet.
- **None** is the default target software version for all Fleets.  
- When you select a target upgrade version, the Leader will upgrade all of the
  Nodes in the selected Fleet to match the version you select from the
  drop-down. The upgrade is initiated when the Node connects to the Leader.
- If you don’t select a version or select **None**, the Leader will not upgrade
  any of the Nodes in the Fleet.
- Since Nodes are now automatically upgraded in a Fleet, we’ve removed the
  Upgrade button from the Edge Settings page.
- We’ve deprecated the **Edge Settings > Fleet Upgrade** UI in favor of a new,
  streamlined Fleet upgrade experience.
- We’ve deprecated the **Enable automatic upgrades** toggle functionality in
  Cribl Edge. The toggle still exists, but is not functional.
- If you prefer, you can return to legacy Edge upgrade functionality. 
  See [Enable Edge Legacy Upgrades](upgrading-ui#enable-edge-legacy-upgrades).

### Install Edge on Linux via RPM

You can now install [Cribl Edge via RPM package](deploy-rpm), which benefits
businesses that requires a tightly maintained environment.

### Spool Incoming Event Data to Disk

The [Disk Spool Destination](destinations-disk-spool) can spool the most recent
(configurable) event data to disk from any source and writes it to a predefined
file path location. This consistent path can help you find data when you use
Cribl Search Datasets.

## Other Enhancements

- We’ve reduced the overall memory usage of Edge on Windows by 10-25% by
  enabling pointer-compression in Windows builds for the Cribl package. Now you
  have 10-25% more memory for important facts like Clay Henry, the beer drinking
  goat who was elected mayor of Lajitas, Texas in 1986!

- The [Prometheus Edge Scraper Source](sources-edge-prometheus) captures more
  event metadata for your Kubernetes Pods and nodes:
    - `__kube_node` when Discovery Type is set to **Kubernetes Node**.
    - `__kube_node` and `__kube_pod` when Discovery Type is set to **Kubernetes Pods**.

## Shared Features

These new features are shared between Stream and Edge:

### OTLP Metrics Function 

The [OTLP Metrics Function](otlp-metrics-function) transforms any
metrics events into the OTLP (OpenTelemetry Protocol) format. This Function can be used
in order to send metric events (events that contain the internal field
(`__criblMetrics`) to any OpenTelemetry-capable destinations.

### Email Notifications Target

Edge now supports an [Email notification target type](notifications#email-notifications).
If you’re a Cribl Edge admin, email notifications make
it easy to receive alerts about any operational issues that require your attention, such as a
particular Source or Destination condition or a pending license expiration.

## Corrections

This release includes the following fixes:

### Sources

- The [Splunk HEC Source](sources-splunk-hec) now allows you to disable individual auth tokens
  via a toggle in the UI. CRIBL-21894

- The [SNMP Trap Source](sources-snmp-traps) now handles SNMP traps containing protocol data units (PDUs) whose
  length is greater than 127. This fixes a bug that interpreted those PDUs incorrectly and
  caused the Source to drop affected traps. CRIBL-22111

- The AppScope package is no longer included by default in a deployment. Instead, it is
  downloaded on the first usage of the AppScope Source. CRIBL-21554

### Destinations

- The Kafka Destination can now send data encoded as OpenTelemetry (OTel) Protocol Buffers (protobufs).

- Edge and Stream have improved the ability to handle retries with common Destinations in the form of Retry-After headers. These are now available for:
  - All [HTTP Destinations](destinations-backpressure-triggers#http-based-destinations)
  - [Azure Data Explorer Destination](destinations-azure-data-explorer)
  - [Google Chronicle Destination](destinations-google_chronicle)
  - [Elasticsearch Destination](destinations-elastic)
  - [Notifications targets](notifications-targets) that use HTTP (PagerDuty, Slack, and Webhook)

  Respecting Retry-After headers means improved reliability when sending to Destinations by respecting rate limits and not overwhelming down-stream systems. CRIBL-21892
	
- Filesystem-based Destinations running on Windows now write file paths with forward slashes,
  fixing a bug that caused certain partition expressions to produce paths containing
  backslashes. CRIBL-20601.

### Notifications

- Edge Notifications now let you limit notifications to the onset and resolution of a
  triggering condition. CRIBL-21217

- Notification targets have an improved user interface and offer JSON editing, so you can
  configure and clone existing targets. CRIBL-21856, CRIBL-21855, CRIBL-20061

### Functions

- Using C.Lookup in a Code Function no longer breaks logging in the Node process. CRIBL-22043

- If a user does not have the proper permissions while editing or creating the Masking
  Function, the modal no longer shows the output section at the bottom to prevent confusion.
  CRIBL-21658

### Security Fixes

- In new customer-managed deployments of Cribl Edge, the Leader now defaults to using an auth
  token that is randomly generated during installation. Cribl administrators will no longer be
  required to manually generate the Leader's auth token when enabling distributed mode. This
  does not affect existing installations, nor is it relevant for Cribl.Cloud, where auth tokens
  are already randomly generated. See [Securing Auth Token](securing-auth-token/). CRIBL-12596 

### Other Functional Fixes

- Managed Edge Nodes have an improved upgrade process that gracefully prevents Leader Nodes from trying to upgrade Edge Nodes that were deployed via RPM images, or that are running in containers (such as containerd, Docker, or Kubernetes). Edge Nodes deployed via RPM images must be upgraded using RPM. Edge Nodes running in containers must be upgraded using an approach that is appropriate for the type of container (for example, you can use a Helm chart to upgrade edge Nodes running in Kubernetes), without involving the Leader. CRIBL-18999

- The Routes UI is more responsive when searching a large number of Routes. CRIBL-20988
