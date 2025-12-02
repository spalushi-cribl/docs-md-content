# Deployment Types


Deployment guide to get you started with self-hosted Cribl Stream

---

There are at least **two** key factors that will determine the type of Cribl Stream deployment in your environment: 

* Amount of Incoming Data: This is defined as the amount of data planned to be ingested per unit of time. E.g. How many MB/s or GB/day? 

* Amount of Data Processing: This is defined as the amount of processing that will happen on incoming data. E.g., is most data passing through and just being routed? Or are there a lot of transformations, regex extractions, field encryptions? Is there a need for heavy re-serialization? 

## [Single-Instance Deployment](deploy-single-instance) 

When volume is low and/or amount of processing is light, you can get started with a single instance deployment. 

## [Distributed Deployment](deploy-distributed)

To accommodate increased load, we recommend [scaling up and perhaps out](scaling) with multiple instances.

## [Splunk App Deployment](deploy-splunkapp) 

If you have an existing Splunk [Heavy Forwarder](https://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Deployaheavyforwarder) infrastructure that you want to use, you can deploy [Cribl App for Splunk](deploy-splunkapp). See the note below before you plan.

> ##### Cribl App for Splunk Deprecation Notice
>
> Please see details [here](deploy-splunkapp).
>
{.box .warning}

## [Kubernetes/Helm Deployment](deploy-kubernetes) 

You can deploy Cribl Stream Leader Nodes (or single instances) and Worker Nodes via Cribl's Helm charts.

## [Docker Deployment](deploy-docker) 

You can deploy Cribl Stream instances using images from Cribl's public Docker Hub.
