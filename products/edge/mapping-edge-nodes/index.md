# Mapping Edge Nodes to Fleets


Learn about Fleet Mappings and how to set up a Mapping Ruleset.

---

Fleet Mappings enable you to assign Edge Nodes to Fleets by
applying specific rules defined in Mapping Rulesets. You can map Edge Nodes into
Fleets based on geographical location, environment type, operating system,
host, and more. This makes centralized configuration management easier for groups
of Edge Nodes, and makes it easy for you to add and remove Edge Nodes from Fleets
as your deployment scales.

## Access Fleet Mappings

Select **Mappings** in the sidebar to display status and controls for the active Mapping Ruleset. 

![Mappings status/controls](ed-mappings-tab-4.13.png)
{border="true"}

>This page displays a maximum of 10,000 results.
{.box .success}

## Mapping Rulesets

A Distributed deployment of Cribl Edge starts off with a predefined `default` Mapping Ruleset.

Within a Ruleset, a list of rules evaluate Filter expressions
on the information that Edge Nodes send to the Leader.

The order of rules in a Ruleset matters.
The **Filter** section supports full JS expressions.
The Ruleset matching strategy is first-match, and one Edge Node can belong to only one Fleet.

![Managing Ruleset page](ed-default-mapping-rule-4101.png)
{border="true"}

> Only one Mapping Ruleset can be active at any one time, although a ruleset can contain multiple rules.
> The system should contain at least one defined and present Fleet.
{.box .info}

## Create a Mapping Ruleset

To create a Mapping Ruleset:

1. In the sidebar, select **Mappings**. 
1. Select **Add Ruleset**. 
1. Give the **New Ruleset** a unique **ID** and select **Save**. 
1. Select **Configure** and start adding rules with **Add Rule**. 

As you build and define rules, the Preview pane will display
how Edge Nodes will map to Fleets with your defined rules.

You must activate a Ruleset before the Leader can use it.
To activate it, go to **Mappings** and select **Activate** on the required Ruleset.
You can also **Configure** or **Delete** a ruleset,
or **Clone** a Ruleset if you'd like to work on it offline, test different filters, and so on. 

Although not required, you can configure Edge Nodes to send a preferred Fleet
name with their payload (found in payload's `cribl.group` key).
See [Mapping Order of Priority](#priority) for how this ranks in mapping priority. 

## Mapping Rule Examples

See some common Mapping Rule examples.

### Map Edge Nodes Based on OS

Fleets are not aware of the operating systems their Edge Nodes run on,
so they can't take into account limitations such as platform-specific Sources and Destinations.

When you manage Edge Nodes on different systems, Cribl recommends creating OS-specific Fleets.

To map Edge Nodes to Fleets based on their OS, use the `platform` filter.

| OS      | Filter                  |
| ------- | ----------------------- |
| Linux   | `platform === "linux"`  |
| Windows | `platform === "win32"`  |
| macOS   | `platform === "darwin"` |

### Map Edge Nodes Connected to an Outpost

To map Edge Nodes connected to a specific Outpost to a specific Fleet,
filter on the `outpost.guid` or `outpost.host` fields, for example:

**Filter**: `outpost.host === '5ab6c676be6a'`.

### Map Edge Nodes Based on Tags

You can use tags assigned to Edge Nodes to map specific Nodes to a Fleet.

To map Edge Nodes that have a specific tag to a Fleet, filter on the `cribl.tags` field,
for example:

**Filter**: `cribl.tags.includes("WinLaptop")`

### Complex Edge Node Mappings

The mapping filter supports JS expressions and lets you string together more complex filters.

For example, let's assume that you want to define a rule for all hosts that satisfy this set of conditions: 

- IP address starts with `10.10.42`, AND Edge Node has more than 6 CPUs, OR:
- `CRIBL_HOME` environment variable contains `DMZ`.

**Filter**: `(conn_ip.startsWith('10.10.42.') && cpus > 6) || env.CRIBL_HOME.match('DMZ')`.

## Mapping Order of Priority {#priority}

Priority for mapping to a Fleet is as follows: Mapping Rules > Fleet
(`cribl.group`) sent by Edge Node > `default_fleet` Fleet.

  * If a Filter matches, use that Fleet.
  * Else, if an Edge Node has a Fleet defined, use that.
  * Else, map to the `default_fleet` Fleet.

## Additional Metadata

Edge Nodes can pass additional metadata to the Leader when Edge is
connected to an AWS EC2 instance or running in a container.

Additionally, Cribl Edge Nodes will surface the following metadata from your AWS
instance when the data is available:

- Security Groups
- Identity, including Account ID and Instance ID
- Roles
- Tags
- Public IPv4
- Hostname



### Find Edge Nodes Running in a Container

Edge Nodes running in a container report additional metadata about the container
to the Leader. This helps you quickly identify which of your Edge Nodes are
running in a container from the Leader UI and map them to the appropriate
Fleets.

When Cribl Edge is running in a container, you will see the `hostOs` property in
events collected by the [System State Source](sources-system-state). This object
will contain properties about the container when they're available, such as: 
- `arch`
- `cpu_count`
- `cpu_speed_mhz`
- `cpu_type`
- `interfaces`
- `memory`
- `platform`
- `release`
- `hostname`
- `timezone`

From the **Mappings** menu, 
you will also see the `hostOs` property in the **Edge Node info** panel. 
This object will include the `id`, `addresses`, and `version` of the container when available.

> `interfaces` is a set of key-value pairs that contain an array of CIDR values.
{.box .info}

### Collect AWS EC2 Instance Tag Metadata

If Cribl Edge is connected to an AWS EC2 instance, you will see metadata for AWS
instance tags in the **Mappings** tab. You can use tags to map EC2 Nodes to
the correct Fleet. 

> For Cribl Edge to obtain tags, you **must [allow access to tags in the EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#allow-access-to-tags-in-IMDS)**.
{.box .warning}
