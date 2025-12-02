# Splunk Cloud Platform and BYOL Integrations



The [Splunk HTTP Event Collector](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector) (HEC) is the preferred method for integrating with the Splunk Cloud Platform. It’s easy to set up, offers superior compression, and efficiently load balances data across multiple indexers in a distributed Splunk environment. While Splunk-to-Splunk (S2S) can be used for specific scenarios, such as legacy integrations or granular data distribution, HEC generally provides a more straightforward and efficient integration process.

Cribl Stream provides multiple integrations for sending data to the Splunk Cloud Platform. The following table outlines the supported Cribl Stream Destinations and Splunk protocols for different Splunk Cloud Platform deployment scenarios:

| Cribl Stream Destination | Splunk Protocol | Splunk Deployment |
|---|---|---|
| [Splunk HEC Destination](destinations-splunk-hec) | Splunk HEC | - Distributed Splunk Cloud Platform <br />- Bring Your Own License (BYOL) deployment (either in a non-Splunk cloud or on-prem) |
| [Splunk Load Balanced Destination](destinations-splunk-lb) | S2S | - Distributed Splunk Cloud Platform <br>- BYOL deployment |
| [Splunk Single Instance Destination](destinations-splunk) | S2S | - Single-instance Splunk Cloud Platform (trial or smaller deployments) |

> For BYOL deployments, leverage the `.pem` and `outputs.conf` files already in use on your Splunk Universal Forwarders to maintain consistency and simplify the security setup. The Splunk [documentation](https://docs.splunk.com/Documentation/Splunk/latest/Security/ConfigureSplunkforwardingtousesignedcertificates) has detailed instructions on securing your Splunk indexers to ensure the overall security of your deployment.
  {.box .success}
