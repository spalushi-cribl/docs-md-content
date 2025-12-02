# Lookups as Filters for Masks


You can make your data architecture more maintainable by using [Lookups](lookups-library) to route and transform events within Cribl Stream. This use case demonstrates an unusual solution, but one that served one Cribl customer's particular goals (which might overlap with yours):

- Ingest many – hundreds of – different `sourcetype`/`index` field combinations. 
- Send all this data through a common Pipeline.
- Stack four [Mask Functions](mask-function) in the Pipeline.
- Evaluate and process each `sourcetype`/`index` field combination **only** within its applicable Mask Functions – either two or three Masks per combination.

> This last restriction reduces latency, by preventing Mask Functions from evaluating non-applicable events, simply to ignore them.
> 
> Just to reiterate, this use case outlined here responded to this customer's requirements – one Pipeline combining multiple Mask Functions, for many `sourcetype`/`index` combinations. More typically, you'd use multiple Pipelines to process different `sourcetype`/`index` combinations.
>
{.box .warning}

To enable this approach, the example below centralizes masking logic for multiple conditions in a Lookup table and corresponding [Lookup Functions](lookup-function). The Lookup's output filters events to the applicable Mask Functions. Specifically, we'll show how to instruct Cribl Stream to:

- Check for a particular `index`/`sourcetype` combination in each event, and
- Based on that combination, determine which Masks to apply to that event.

## Design the Lookup

To use a lookup as a filter, you'd start by creating a comma-separated lookup table in this format, and [adding it to Cribl Stream](lookups-library):

``` {title="index_tracker.csv"}
index,sourcetype,masks
apache_common, sourcetypec, ssn|credit_card|auth_token
syslog,sourcetypeb,ssn|auth_token
weblog,sourcetypea,auth_token|bearer_token
```

Below the header, each row specifies an index, a sourcetype, and (in the third column) a pipe-delimited list of applicable masks.

To make this example work, the table must have only **one** row for each index/sourcetype combination. (This unusual restriction is particular to this scenario.) So, as you build out the lookup table, you cannot add new masks for **existing** index/sourcetype combinations by appending new rows. Instead, you must modify the third column of the existing rows.

## Configure the Pipeline

Create a Cribl Stream Pipeline with a Lookup Function configured like this, pointing to your lookup table:

![Lookup Function's configuration](lookup-function-config.98c537606a.png)
{border="true"}

This Function keys against both the `index` and `sourcetype` fields. When it finds a matching combination, it adds a new key-value pair to your event for future filtering.

The key of that key-value pair (namely, `__masks`) starts with a double underscore, to make it a Cribl Stream internal field. This convention ensures that the key-value pair will **not** get passed along to the Destination. 

However, you might prefer to export the key-value pair. For example, you might want a Splunk Destination to index the list of masks applied to a given event, alongside that event. (This approach applies to many forensic use cases.) If so, remove the double underscore from the above Function's **Lookup Field Name in Event** value, and from the subsequent Filter expressions for each Mask Function.

Each Mask Function has a JavasScript Filter that breaks the pipe-delimited string into an array, and determines whether the tag for that type of mask (e.g., `bearer_auth`) is in the `__masks` key-value pair. If so, it applies the mask processing. If not, the event moves on to the Pipeline's next Mask Function. 

Here are the four Mask Functions below the Lookup Function:

![Mask Functions](masks-filtered.f476980dcc.png)
{border="true"}

In this particular example, the pipe-delimited mask tags in the lookup table's third column match the Mask Functions' names, as well as matching their **Filter** conditions. This is just for simplicity – the Functions could have any names, as long as the **Filter** expressions match the tags.
