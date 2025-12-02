# Map Workers to Worker Groups


Mapping Rulesets define how Worker Nodes are distributed among Worker Groups.
A Ruleset consists of a list of rules that evaluate Filter expressions on the information that Worker Nodes send to the Leader Node.

> Only one Mapping Ruleset can be active at any one time.
{.box .warning}

The ruleset behavior is similar to [Routes](routes), where the order matters, and the **Filter** section supports full JavaScript expressions. The ruleset matching strategy is first-match, and one Worker Node can belong to only one Worker Group.

> Configuring Mapping Rulesets is possible only when you have started Cribl Stream in [Leader mode](setting-up-leader-and-worker-nodes#config-leader).
{.box .info}

If you define no Mapping Rulesets of your own, the built-in `default` Mapping Ruleset with a `default` rule matching all (`true`)
assigns all Worker Nodes to the `default` Worker Group, unless the Worker Node itself [specifies differently](#worker-mapping-priority).

## Worker Mapping Priority

Worker Nodes can be assigned to Worker Groups in two ways: via Mapping Rulesets,
and by including the group in the Worker's payload.
When both are defined, the assignment follows this priority:

1. Group matched by a filter in a Mapping Ruleset.
2. Group defined in the Worker Node.
3. The `default` group.

## Create a Mapping Ruleset

To create a Mapping Ruleset:

1. In the sidebar, select **Mappings**.
1. Select **Add Ruleset**
1. Provide a unique **ID** for the new Ruleset and confirm with **Save**.
1. Select the new Ruleset in the table to enter a rule editing screen.

> You can't create custom Mapping Rulesets for Cribl Stream Workers in Cribl.Cloud.
{.box .info}

### Add Mapping Rules

Each Mapping Ruleset can have one or more rules.

1. Select **Add Rule** to start adding rules.
   While you build and refine rules, the Preview in the right pane will show which currently reporting and tracked Worker Nodes map to which Worker Groups.
1. For each rule, formulate a **Filter** expression that maps Worker Nodes.
1. In **Group**, identify the Worker Group to which the Worker Node should be assigned.
1. **Save**, and add more rules, if needed.

### Activate Mapping Ruleset

A ruleset must be activated before it can be used by the Leader Node.

To activate it, go to **Mappings** and select **Activate** on the required ruleset.

Activating a ruleset deactivates any other existing rulesets.

## Mapping Rule Example

This example assumes that you want to define a rule for all hosts that satisfy the following set of conditions:

- IP address starts with `10.10.42` AND Worker Node has more than 6 CPUs, OR:
- `CRIBL_HOME` environment variable contains `w0`.

The rule should assign the Worker Node to `Group420`.

| Field         | Value                                                                           |
| ------------- | ------------------------------------------------------------------------------- |
| **Rule Name** | `myFirstRule`                                                                   |
| **Filter**    | `(conn_ip.startsWith('10.10.42.') && cpus > 6) \|\| env.CRIBL_HOME.match('w0')` |
| **Group**     | `Group420`                                                                      |
