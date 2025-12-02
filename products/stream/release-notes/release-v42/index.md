# Cribl Stream 4.2


> ##### Breaking Change
>
> A critical regression caused Worker Groups to become unresponsive, preventing access to Sources and Destinations. This issue was caused by incorrectly encrypted credentials. Users should install or upgrade to Cribl Stream v.4.2.1 when it becomes available. 
>
{.box .warning}

Cribl Stream 4.2 turns up the dial on data magic. This release summons new features, enhancements, and fixes that will leave you spellbound. Get ready to embark on a journey of data sorcery as we unveil the secrets of the latest release.

## New Features 

This release provides the following improvements:

### Members and Permissions

Welcome to the latest evolution in Cribl's role-based access control, [Members and Permissions](members). This feature adds finer-grained access control and authorization, allowing you to assign access and capabilities to users independently at the Organization, Product, Group/Fleet, and resource levels. 

### Stream Projects

We're proud to announce that [Stream Projects](projects) is officially released! Now out of beta, Projects opens up Streams to new teams, users, and use cases by providing isolated workspaces (Projects) where users can build pipelines safely, securely, and collaboratively.

Cribl admins can now introduce Projects to new parts of their organization, scoping and containing user access on a per-Project basis. This gives users a streamlined workspace where they can access specific data and Destinations and manage data flows without affecting others in the organization. 

### New and Enhanced Sources, Collectors, and Destinations

We've added a new Microsoft [Azure Sentinel](destinations-sentinel) Destination that makes onboarding your security data into Azure Sentinel a one-step process. The new Destination supports all custom tables and four Azure Sentinel native tables.

[Database Collectors](collectors-database) now have rising column support, providing state tracking across collection jobs run against databases. This ensures that database collection resumes at the most recent record in a database table, only collecting new data from the targeted tables between runs.

The Office 365 Message Trace Source now has an [OAuth Certificate authentication option](sources-office365-msg-trace#oauth-cert). 

The [System State Source](sources-system-state) adds more collectors, including Listening Ports, Logged-In Users, and Services.

You can now write summaries and histograms to a Prometheus Destination and receive summaries and histograms from a Prometheus Source.

The [Amazon CloudWatch Logs Destination](destinations-cloudwatch-logs#how-the-amazon-cloudwatch-logs-destination-batches-events) now specifies that a batch of log events in a single request cannot span more than 24 hours.

Some Sources and Collectors have a new option to reject unauthorized certificates that cannot be verified against a valid Certificate Authority.

Disabled Sources no longer generate Cribl internal metrics, which reduces clutter on the Monitoring page and improves Leader scalability. 

### Function Enhancements

A new field has been added to the [Serialize Function](serialize-function) that allows you to specify the delimiter between key=value pairs.

The [Code Function](code-function) now has an optional **Activity log sample rate** setting that you can use to reduce the volume of log entries. You might do this, for example, when an unexpected mismatch between incoming data and the Code Function produces a flood of errors. 

### New Export Option

You can now export the list of Worker Nodes connected to the Leader on the **List View** tab.

### New Worker Config Status Button

We've added a new **Workers** button that displays at-a-glance Worker health status, visible from anywhere on the **Manage** tab. Clicking this button takes you directly to the **Workers** tab, with the list filtered to show just the Group you're working with. 

### Default Compression on HTTP-based Destinations

To minimize egress charges, all HTTP-based Destinations now enable compression by default. 

## Breaking Changes

> * In v.4.2 and newer, Stream Projects/Subscriptions rely entirely on the granular [Members and Permissions](members) access control first introduced in Stream 4.2. The v.4.1.x `project_user` Role and `ProjectSourceSubscribe` Policy, which provided access to all Projects, are now retired.
> 
>     If you are upgrading with Projects configured in v.4.1.x, users of those Projects will be visible in Stream as Members, and you'll need to assign them new Permissions. For details, see [Adding Users to Projects](projects#access).
> 
> * Any Projects created in versions older than v.4.2 will need to be re-created. You should also delete and re-create any Subscriptions that carry over during the upgrade.
>
{.box .warning}

## Corrections

This release includes the following fixes:

### Sources, Collectors, and Destinations Fixes

CRIBL-18473 Linux System Metrics Source now correctly emits "Per interface" metrics in "All" mode.

CRIBL-18900 Fixed an issue that was causing `unsupported op-code` errors for customers sending to Cribl Stream using the S2S V4 Protocol after upgrading their Splunk forwarders to 9.1.x. 

CRIBL-14948 Notifications are no longer sent for inactive Sources and Destinations. 

CRIBL-17568 Corrected issue with filesystem-based destinations (including S3, Azure Blob Storage, and Filesystem) in which the file partitioning string was occasionally truncated when events were sent if the `Remove empty staging dirs` option was toggled off, resulting in events being stored on incorrect paths.

CRIBL-17685 When Cribl Stream loses its connection to a downstream indexer, diverting events to a different configured indexer, Splunk Load Balanced Destinations will now send these events in correct form. Prior to this fix, Stream sometimes omitted internal fields that Splunk requires (`_linebreaker` and `_done`), resulting in Splunk merging content from multiple events together upon arrival.

### Access and Functional Fixes

CRIBL-18400 **Manual** mode now honors the **Max depth** setting in the File Monitor Source and on the **Explore** > **Files** tab.

CRIBL-15448 The `collector_all` role now allows you to run and configure existing collectors on all Worker Groups/Fleets (in addition to creating new collectors). 

CRIBL-17939 Users with the `GroupEdit` policy can now commit changes.

CRIBL-17792 For Cribl Stream Leaders that have the `URL base path` setting configured, Worker Nodes now update with the latest config version from the Leader.

CRIBL-17245 Results will no longer be incomplete when modifying a Lookup file via **Table** mode.

CRIBL-17231 A new, configurable **Assume Role** > **Duration (seconds)** setting corrects the problem of failing sessions caused by insufficient session duration. Affected Amazon Sources and Destinations include Kinesis, SQS, and S3, as well as SNS and CloudWatch Destinations. 
