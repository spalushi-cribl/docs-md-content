# Configure Cribl Guard


>Cribl Guard is available to Cribl Stream users with the Editor Permission (or `stream_editor` Role) or higher, on an Enterprise plan.
{.box .success}

When applied as a Function in a Pipeline, Cribl Guard
automatically scans your data streams for sensitive information, then applies
mitigation methods to that data. To learn more about Cribl Guard and understand
the use cases, go to [Learn About Cribl Guard](/guard/about).

## Overview of Cribl Guard Workflow

There are two distinct setup workflows to configure Cribl Guard. You can
choose between the two, or use a combination of the approaches for different
Destinations.

- Enable Cribl Guard protection on individual Destinations and mitigate sensitive data
  based on the Default ruleset, or:
- Add the Cribl Guard Function to new or existing Pipelines with the ruleset(s)
  you select.

### Enable Cribl Guard on Destinations

When you enable Cribl Guard on your Destinations, Cribl creates a new
Post-Processing Pipeline for that Destination containing the Cribl Guard
Function and configured with the **Default** ruleset.

1. Log in to your Workspace, then select Cribl Stream.
1. From the sidebar navigation, select **Guard.**
   - Cribl Guard is disabled on Destinations by default.
   - The Destinations in this list are per Worker Group.
1. Toggle **Protect.**
1. You'll see a Post-Processing Pipeline appear in the **Route/Pipeline** column
   and the toggle will display blue.
   >Cribl Guard will begin drawing down credits as soon as data flows through
   >this Pipeline, even if no sensitive data is found. See [How You're Billed](#how-youre-billed-for-cribl-guard)
   >for more information.
   {.box .info}
1. Select **Commit & Deploy** to activate the Pipeline changes on your Destination.

To learn more about the rules contained in the Default ruleset, go to the
[Default section of the ruleset docs](guard-rulesets#default-ruleset).

### Add the Cribl Guard Function to Pipelines

If you have different or more complex use cases than the Default ruleset covers,
you can use the Guard Function in your Pipelines. With the Guard Function, you
can customize the rulesets that Cribl uses to identify and mitigate sensitive
data.

1. Open a Worker Group.
1. Select **Processing > Pipelines > [Select a Pipeline] > Add Function > Standard > Cribl Guard.**
1. Configure the [Cribl Guard Function](guard-function) to fit your needs.
1. Add one or more Scanning Rulesets to match incoming sensitive data.
   You can use the out-of-the-box Cribl Rulesets or create custom rules from the
   [Knowledge Library](guard-rules-library).
1. Select **Commit & Deploy**.

## Explore the Cribl Guard Homepage

The **Guard** homepage is your place to view and protect Destinations with a few
easy steps. All the Destinations configured in this Workspace will appear on the
**Guard** page.

![The Cribl Guard homepage with callouts for description](guard-explore.png)
{border="true" scale="100%"}

| Callout |Area| Description |
| :----   |:---| :---------- |
|**A**|Worker Group Selection| Choose the Worker Group you want to view Destinations for. You can view Worker Groups one at a time.|
|**B**|Metrics Overview| Card overview for Cribl Guard. This area is a way to view at-a-glance metrics for data scanned and modified, as well as Destinations protected.|
|**C**|Protect Toggle| Protect your Destinations with this toggle. This creates a Post-Processing Pipeline for the Destination and uses the [Default Cribl Guard ruleset](guard-rulesets#default).|
|**D**|Pipelines| The Pipeline associated with the Destination, along with the type of Pipeline it is (Pre-Processing, Processing, Post-Processing). Select the Pipeline name to open it. Visit [Types of Pipelines](/stream/pipelines#types-of-pipelines) for more details.|
|**E**|Scanning Rulesets| The number of Guard rulesets configured in the Pipeline.|
|**F**|Events Metrics| At-a-glance metrics for the number of scanned and modified events and bytes in this Pipeline.|
|**G**|Actions| Select the pencil button to jump into the Pipeline for quick editing.|

## How You Are Billed for Cribl Guard

Cribl Guard billing works by charging for bytes of data scanned. Cribl Guard
only begins scanning data and therefore accruing costs when you either:

1. Toggle **Protect** on a Destination, or
1. Add the Cribl Guard Function to a Pipeline and start processing data through
   that Pipeline.
1. The [Guard homepage](#explore-the-cribl-guard-homepage)
   shows you an overview of the events and bytes scanned and modified per
   Destination and overall.

To view your Cribl Guard spend, navigate to the [FinOps Center](/stream/cloud-billing)
in your Organization.