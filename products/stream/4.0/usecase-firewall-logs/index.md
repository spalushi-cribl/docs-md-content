# Firewall Logs: VPC Flow Logs, Cisco ASA, Etc.


---

## Recipe for Sampling Firewall Logs

Firewall logs are another source of important operational (and security) data. Typical examples include [Amazon VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html), [Cisco ASA Logs](https://www.cisco.com/c/en/us/td/docs/security/asa/syslog/b_syslog.html), and other technologies such as Juniper, Checkpoint, pfSense, etc.

As with [Access Logs](usecase-access-logs), bringing in **everything** for operational analysis might be cost-prohibitive. But sampling with Cribl Stream can help you:

* Ingest enough sample events from the majority category that your aggregate analysis remains statistically significant. E.g., sample all `ACCEPT`s at `5:1`.

* Ingest **all** events from the minority categories, and perform troubleshooting and introspection with full-fidelity data. E.g., bring in all `REJECT`s.

## Sampling VPC Flow Logs

AWS' [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) feature enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow Log data can be published to Amazon CloudWatch Logs and Amazon S3. 

Typical VPC Flow Logs look like this: 

``` {title="Flow Log Records for Accepted and Rejected Traffic"}
2 123456789010 eni-abc123de 172.31.16.139 172.31.16.21 20641 22 6 20 4249 1418530010 1418530070 ACCEPT OK
2 123456789010 eni-abc123de 172.31.9.69 172.31.9.12 49761 3389 6 20 4249 1418530010 1418530070 REJECT OK
```

Let's use a **very simple** Filter condition and only look for `ACCEPT` events:

1. Add a **Regex Extract** Function that looks at: `sourcetype=='aws:cloudwatchlogs:vpcflow'`

2. Configure that Function to extract a field called `__action` with this regex:`/(?<__action>ACCEPT)/`

![Extracting the `__action` field](firewall-logs-1.57dfb1d5bc.png)

3. Add a **Sampling** Function to sample `5:1` when `__action=="ACCEPT"`.
4. Save.

![Sampling `ACCEPT` events](firewall-logs-2.5bb598e736.png)

## Note About Sampling

Each time an event goes through the Sampling Function, an index-time field is added to it: `sampled: <rate>`. It's advisable that you use that in your statistical functions, as necessary.

## Other Sourcetypes

Other sourcetypes that will benefit from sampling, but might need a different `__action` extraction regex:

Sourcetype | Filter Expression
--- | ---
[Cisco ASA Logs](https://www.cisco.com/c/en/us/td/docs/security/asa/syslog/b_syslog.html) | `sourcetype=='cisco:asa'`
Related sourcetypes to consider sampling: | `sourcetype=='cisco:fwsm'`<br/>`sourcetype=='cisco:pix'`
