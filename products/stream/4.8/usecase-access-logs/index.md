# Access Logs: Apache, ELB, CDN, S3, Etc.


---

## Recipe for Sampling Access Logs

Access logs are extremely common. They're often emitted by web servers or similar/related technologies (proxies, loadbalancers, etc.), and tend to be highly voluminous. Typical examples include Apache access logs, and CDN logs such as those from [Amazon Cloudfront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html), [Amazon S3 Server Access Logs](https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html), [AWS ELB Access Logs](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html), etc. 

For large installations, bringing everything into an analytics tool is often so cost-prohibitive (storage, resources, license, etc.) that most users don't even bother. However, some of the logs contain relevant information when looked at individually (e.g., errors). The much larger majority contains relevant information when looked at in the aggregate (e.g., successes to determine traffic patterns, etc.). 

It would be great if we could find a middle ground. With the Sampling Function, you can! Specifically, you can:

* Ingest enough sample events from the majority category that your aggregate analysis remains statistically significant.

* Ingest *all* events from the minority categories, and perform troubleshooting and introspection with full-fidelity data.

## Using `status` as the Sampling Condition

Most of the access logs (including the ones mentioned above) have very similar formats. One quick way to sample is to look at the value of the `status` field. `2XX`s indicate success and tend to be, by far, the most common ones – with `200` being the top. **Therefore, `200` is the perfect candidate for sampling**. All other statuses occur much less frequently, indicate conditions that often need to be looked at, and can be brought in with full fidelity. 

## Sample Status 200 at 5:1

1. Add a **Regex Extract** Function that looks at these sourcetypes: `sourcetype=='access_combined' || sourcetype=='aws:s3:accesslogs'`

2. Configure that Function to extract a field called `__status` with this regex:`/HTTP\/\d\.\d"\s(?<__status>\d+)/`

![Defining the Regex Extract Function](access-logs-1.61ce316818.png)
{border="true"}

3. Add a Sampling Function to sample `5:1` when `__status==200`.

4. Save.

![Sampling success reponses](access-logs-2.fdc81559a6.png)
{border="true"}

## Note About Sampling

Each time an event goes through the **Sampling** Function, an index-time `sampled::<rate>` field is added to it. Use this field in your statistical Functions, as necessary.

## Other Sourcetypes

Examples of other sourcetypes that will benefit from sampling, but might need a different `__status` extraction regex:

Sourcetype | Filter Expression
--- | ---
[Amazon Cloudfront Access Logs](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) | `sourcetype=='aws:cloudfront:accesslogs'`
[Amazon ELB Access Logs ](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/access-log-collection.html) | `sourcetype=='aws:elb:accesslogs'`
