# Ingest-Time Fields


---

## Adding Fields to Data in Motion

To add new fields to any event, we use the out-of-the-box **Eval** Function. We can either apply a Filter to select the events, or we can use the default `true` Filter expression to apply the Function to all incoming events. 

## Adding Fields Example

Let's see how we add `dc::nyc-42` to all events with `sourcetype=='access_combined'`:

* First make sure you have a Route and Pipeline configured to match desired events.

* Next, let's add an **Eval** function to it with `sourcetype=='access_combined'` as Filter.

* Next, let's click on **Add Field**, add our `dc` field, and click **Save**.

![Adding the `dc` field](ingest-time-2.c4b9199b7a.png)
{border="true"}

To confirm, verify that this search returns results: `sourcetype="access_combined" dc::nyc-42 ` 

* You can add more conditions to the filter, if you'd like. For example, to limit the field to only events from hosts that start with `web-01`, we can change the filter input to `sourcetype=='access_combined' && hosts.startsWith('web-01')`.

This is a **very** powerful method to change incoming events in real time. In addition to providing the right context at the right time, users can further benefit substantially by using `tstats` for **faster** analytics.


## Removing Fields

You can remove fields by listing and/or wildcarding field names. Let's see how we can remove all fields that start with `date_`.:

* First, make sure you have a Route and Pipeline configured to match desired events.

* Next, let's add an **Eval** function to it (as above).

* Next, in **Remove Fields**, add `date_*` and hit Save.

![Goodbye `date_` field](ingest-time-4.52e36f6a28.png)
{border="true"}

To confirm, verify that this search: `sourcetype="access_combined" date_minute=* ` will soon stop returning results. Enjoy a more efficient Splunk!