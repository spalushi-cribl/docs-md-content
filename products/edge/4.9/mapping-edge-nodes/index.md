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

Click the **Mappings** tab (**Manage > Mappings**) to display status and
controls for the active Mapping Ruleset. 

![Mappings status/controls](ed-mappings-tab.2705357ae0.png)
{border="true"}

>This page displays a maximum of 10,000 results.
{.box .success}

## Mapping Rulesets

Within a Ruleset, a list of rules evaluate Filter expressions on the information
that Edge Nodes send to the Leader.

To manage and preview the Rules in a Ruleset, click into it. 

In a Ruleset, the order of Rules matters. The **Filter** section supports full JS
expressions. The Ruleset matching strategy is first-match, and one Edge Node can
belong to only one Fleet.

![Managing Ruleset page](ed-default-mapping-rule.c3f80c45b5.png)
{border="true"}

>Only one Mapping Ruleset can be active at any one time, although a ruleset can
>contain multiple rules. The system should contain at least one defined and present Fleet.
>system.
{.box .info}

## Creating a Mapping Ruleset

To create a Mapping Ruleset:

1. Select **Manage** on the top nav > **Mappings**. 
1. Select **Add Ruleset**. 
1. Give the **New Ruleset** a unique **ID** and click **Save**. 
1. Select **Configure** and start adding rules with **Add Rule**. 

As you build and define rules, the Preview pane will display all the Edge Nodes
that will map to Fleets matching your defined rules.

You must activate a ruleset before the Leader can use it. To activate it,
go to **Mappings** and click **Activate** on the required ruleset. The
**Activate** button will then change to an **Active** toggle. Using the adjacent
buttons, you can also **Configure** or **Delete** a ruleset, or **Clone** a
ruleset if you'd like to work on it offline, test different filters, etc. 

Although not required, you can configure Edge Nodes to send a preferred Fleet
name with their payload (found in payload's `cribl.group` key)
See [Mapping Order of Priority](#priority) for how this ranks in mapping priority. 

> For platform-specific mapping, set the `platform` property to `win32`
> for Windows, or to `linux` for Linux.  
>
{.box .info}

## Example Mapping Rule

Within a Mapping Ruleset, click **Add Rule** to define a new rule. Assume that
you want to define a rule for all hosts that satisfy this set of conditions: 

* IP address starts with `10.10.42`, AND:
* More than 6 CPUs OR `CRIBL_HOME` environment variable contains `DMZ`, AND:
* Belongs to `Fleet420`.

### Rule Configuration

* **Rule Name**: `myFirstRule`
* **Filter**: `(conn_ip.startsWith('10.10.42.') && cpus > 6) || env.CRIBL_HOME.match('DMZ')`
* **Fleet**: `Fleet420`

## Default Fleet and Mapping

A Cribl Edge instance will create the following automatically when it runs as Leader:

- A `default_fleet` Fleet.
- A `default` Mapping Ruleset,

## Mapping Order of Priority {#priority}

Priority for mapping to a Fleet is as follows: Mapping Rules > Fleet
(`cribl.group`) sent by Edge Node > `default_fleet` Fleet.

  * If a Filter matches, use that Fleet.
  * Else, if an Edge Node has a Fleet defined, use that.
  * Else, map to the `default_fleet` Fleet.

## Managing Edge Nodes on Multiple Platforms {#platforms}

The Leader is unaware of Edge Nodes' platforms, such as Linux or Windows, within a
Fleet, which means the ConfigHelper omits platform-specific limitations.
Therefore, when you manage Edge Nodes on heterogeneous platforms, create a
Windows-specific Fleet and mapping.

For what's supported on Windows, see [Cribl Edge on Windows](explore-edge-win).

## Collecting AWS EC2 Tag Metadata

If Edge is connected to an AWS EC2 instance, the **Mappings** tab can gather
metadata for AWS instance tags. You can use tags to map EC2 nodes to the correct
Fleet. 

**You must [allow access to tags in the EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#allow-access-to-tags-in-IMDS)
for Cribl Edge to obtain them.**