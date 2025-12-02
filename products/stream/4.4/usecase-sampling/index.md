# Sampling


---

## Sampling at Ingest-Time 

Let's say that you wanted to analyze and troubleshoot with **highly verbose/voluminous** data – for example, CDN logs, ELB Access Logs, or VPC Flows – but you were concerned about storage requirements and search performance. With Sampling, you can bring in enough samples that your analysis remains statistically significant, and also do all the necessary troubleshooting. 

See the example below, or see more details in [Access Logs](usecase-access-logs) and [Firewall Logs](usecase-firewall-logs).

## Sampling Example

Let's use the out-of-the-box **Sampling** function to sample all events from `sourcetype=='access_combined'` where `status` is `'200'`. We'll sample these at 5:1 (and all other events at 1:1). This should lower the volume of all verbose successes (`200`s), but still bring in **all** potentially erroneous events (`400`s, `500`s, etc.) that can be used for troubleshooting.

* First, make sure you have a Route and Pipeline configured to match desired events.

* Next, let's add a **Regex Extract** Function to extract the status field from `_raw`, and let's call the resulting field `__status`. Remember, fields that start with `__` are special fields in Cribl Stream, and can be used anywhere in a Pipeline. 

![Extracting the `__status` field](sampling-1.e2616ab517.png)
{border="true"}

Next, let's add a **Sampling** function, and scope it to all events where `sourcetype=='access_combined'`. Let's apply a filter condition of `__status == 200`, and a Sample Rate of `5`.

![Sampling success responses](sampling-2.8f30b5824a.png)
{border="true"}

To confirm that sampling works, compare the event count of all `200`s before and after.

> Each time an event goes through the **Sampling** function, an index-time `sampled::<rate>` field is added to it. You can use this field in your statistical functions, as necessary.
>
{.box .info}
