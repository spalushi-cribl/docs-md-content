# Cribl Edge 4.0


## New Features 

### Cribl Search

It's fall (in love) season and we've dropped our long-awaited, much-anticipated, brand-new search product. [Cribl Search](/search/) will take you to the **edge**, where you can search data that hasn't been centralized yet. Cribl Search will roll with you to search data in motion, as it **streams** through your Observability Pipeline. And **rest** assured, Cribl Search will take you to your lake to search your stored data. 

The search tool you've all been waiting for is now available on Cribl.Cloud, ready to perform federated "search-in-place" queries on any data, in any format, at any location – at the edge, in flight, in an observability lake, or even within existing systems. You'll vastly increase your scope of analysis without the cost or complexity of first shipping, ingesting, and storing the data.

### Leader Scalability 

You asked and we obliged: You can now deploy more Edge Nodes to bring in more data while spending less time managing it. We increased Leader scalability to support up to 15,000 Edge Nodes. Check out our current requirements for [Leader Scalability](deploy-planning#scaling).

### Fleet Management

We wouldn't increase your Edge Nodes' capacity without making it easier for you to manage them. We've redesigned our UI to give you a top-of-the-line user experience, including granting you the power to create layered configurations and efficient groupings to manage a large number of Edge Nodes. You can now create Subfleets that inherit configurations from their parent Fleets, and you can create Subfleet children five levels deep.

We redesigned with one goal in mind: significantly reduce the time to value. Some of our redesign highlights include: 

- Enhanced Fleet information with a top-level metrics graph in the dashboard.

- Hierarchical layout for inheritance to make it easier to view the Fleets, nested Subfleets, and their children Fleets. 

- Monitoring view at the per-Fleet level. 

- Enhanced Teleport experience with a new Health/Monitoring view and metrics. 

For details, check out our [Explore Cribl Edge](explore-edge) and [Fleets](fleets) docs. 

### Cribl Edge and AppScope Tie the Knot 
 
If you thought we couldn't top the unveiling of Cribl Search, how about this? Announcing the Cribl Edge + AppScope union that allows you to collect high-fidelity application data you didn't even know existed. For all of you advanced users, this integration enhances observability data (that Cribl Edge collects from the container/host) with full application visibility, including rate, error, and duration of HTTP requests, filesystem accesses, network flows, and performance metrics – from any Linux executable.

This integration has two parts: 
- Our robust **Knowledge** library now includes [AppScope Configs](appscope-configs), enabling you to customize runtime configurations for AppScope. We even threw in a default sample configuration – out of the box – to give you a head start. 
- The [AppScope Source](sources-appscope) now includes an **AppScope Filter Settings** tab, where you can easily configure, manage, and deploy AppScope directly from the edge. 

### Kubernetes on the Edge 

To all of you K8s heads, this is for you. We've launched mad support for Cribl Edge on Kubernetes, and from deployment docs to brand new Sources, we've got you covered. Here are the features we've been cooking up:
- On the deployment front, we created a Helm chart to remove guesswork on how to deploy Cribl Edge as DaemonSet in Kubernetes. For context, check out our doc [Deploying via Kubernetes](deploy-running-kubernetes).
- On the Sources front, we've got not one, but two brand-new Sources to deliver metrics and logs right to your doorstep: [Kubernetes Metrics Source](sources-kubernetes-metrics) and [Kubernetes Logs Source](sources-kubernetes-logs). Take the observability of your Kubernetes environments to the next level, via our  `kube` metadata UI option and `__metadata.kube` property.

### System State Source 

As we continue to extend capabilities on the edge, we're introducing the brand-new [System State Source](sources-system-state), which collects snapshots of the host system's current state – including contents of the hosts file and routes – and sends them as events to downstream destinations. You can also view the data collected on the **Explore** > **System Data** tab.

### Cribl Edge on Windows

We've got all of you Windows admins covered, too. Our focus on this release has been Edge Windows-Linux parity, so we now offer: 

- A [Home](explore-edge-win#home) tab, where you can explore the metrics and log data that the Edge Node has auto-discovered, and manually discover and explore other data of interest.

- A brand new Source, [Windows System Metrics](sources-windows-metrics/), to collect Windows metrics. 

- TLS support options on the [Windows installer](deploy-windows).

- The [System State Source](sources-system-state), which works on Windows. 

### Other Enhancements 

We're continually enhancing our Sources, Destinations, and Functions to keep delivering the features you've been asking for. Here's a quick rundown: 

-  In the [File Monitor Source](sources-file-monitor), you can now filter by by file age, file modification time, and events newly arrived since Edge's last startup.

- [Windows Event Forwarder Source](sources-wef) now supports OCSP (Online Certificate Status Protocol) checks on client certificates.

- Splunk Single Instance and Splunk Load Balanced Destinations now support S2S v4.

- The Google Chronicle Destination now supports custom Log types.

- Redis now offers ++Cluster support.

- The new Pipeline Profiler instruments and optimizes individual Functions' execution times.

- Idle-session expiration now has a more generous 60-minute default.

- To generate smaller diagnostic bundles via the on-prem UI, you have a new option to disable **Include metrics**.

### Consolidated Top Navigation

Here's the one you've been waiting for: Cribl Edge has a whole new look. We've tossed the left nav, and we've consolidated all primary navigation into text links running along the top of your browser. 

No more struggling to remember what each of the left nav's little alien symbols stood for. Actually, no more left nav to hover over, expand, and mask your work. "Some of our options have changed," but the underlying pages are mostly the same, and we hope you'll find them easier to get to.

![Cribl Edge 4.0 navigation](nav-4.0-edge-annotated.23ff4f61a6.png)
{border="true"}

> The product selector at upper left appears only in distributed deployments, and it displays a **Cribl Search** option only on Cribl.Cloud.
> 
> In distributed deployments, most **Limits** settings – including **Metrics** and **Jobs** – have moved from **Global Settings** to **Fleet Settings**. (In single-instance deployments, they remain under **Global Settings**.)
> 
> The few remaining alien symbols at the top right are required by the Area 51 technology that Cribl software incorporates.
>
{.box .info}

## Renamed Sources and Integrations

CrowdStrike and Humio integrations are now renamed "CrowdStrike Falcon" in the UI and documentation, because there's a new sheriff in town. Access endpoints are unchanged.
- The CrowdStrike Source is now labeled [CrowdStrike FDR](/stream/sources-crowdstrike).
- The Humio HEC Destination is now labeled [CrowdStrike Falcon LogScale](destinations-humio-hec).

![](artist-formerly-known-as.f0123b9ae1.png)
{scale="7%" border="true"}


## Cross-Compatibility for This Release

Cribl Stream 4.0, Cribl Edge 4.0, Cribl Search 1.0, and AppScope 1.2 are mutually compatible. If you integrate any of these products with earlier versions of peer products, some or all features will be unavailable.

## Deprecation Notices

> The following resources, previously badged Deprecated, are no longer supported as of this release. Please transition your configurations accordingly:
>
{.box .warning}
- Cribl Stream Source: Please use the [Cribl HTTP](sources-cribl-http) or [Cribl TCP](sources-cribl-tcp) Source instead.
- Cribl Stream Destination: Please use the [Cribl HTTP](destinations-cribl-http) or [Cribl TCP](destinations-cribl-tcp) Destination instead.
- Prometheus Publisher Function: Please use the [Prometheus Destination](destinations-prometheus) instead.
- Reverse DNS Function: Please use the [DNS Lookup Function](dns-lookup-function) instead.

Also as of this release, Cribl has removed the Intercom (support) chat widget from the lower-right corner of all on-prem and Cribl.Cloud deployments.  For other ways to contact Support, please see [Get Product Help](/support/support-contact/).

## Corrections 

This release includes the following fixes:

- [CRIBL-13187] The Fleets page no longer reloads every time a Group is committed or deployed.

- [CRIBL‑12762] and [CRIBL-12892] File Monitor Source no longer fails with high-volume logs and file rotation.

- [CRIBL-11972] Using `CRIBL_GIT_* environment` variables no longer crash the Leader on startup.

- [CRIBL-11413] Edge MSI installer now supports TLS options.

- [CRIBL-13077] Edge Fleet dropdown is no longer blocked by the icon below it in QuickConnect. 
