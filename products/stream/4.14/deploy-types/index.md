# On-Prem Deployment


Deployment guide to get you started with self-hosted Cribl Stream

---

In an on-prem (in other words, self-hosted) deployment, Cribl Stream runs on your own infrastructure.

There are at least **two** key factors that will determine the type of on-prem Cribl Stream deployment in your environment: 

* Amount of Incoming Data: This is defined as the amount of data planned to be ingested per unit of time. E.g. How many MB/s or GB/day? 

* Amount of Data Processing: This is defined as the amount of processing that will happen on incoming data. E.g., is most data passing through and just being routed? Or are there a lot of transformations, regex extractions, field encryptions? Is there a need for heavy re-serialization? 

> An alternative to self-hosting Cribl Stream is using [Cribl.Cloud](deploy-cloud).
{.box .success}

The available on-prem deployment types are:

- [Single-instance deployment](deploy-single-instance) - When volume is low and/or amount of processing is light, you can get started with a single instance deployment. 

- [Distributed deployment](deploy-distributed) - To accommodate increased load, we recommend [scaling up and perhaps out](scaling) with multiple instances.

- [Splunk App deployment](deploy-splunkapp) - If you have an existing Splunk [Heavy Forwarder](https://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Deployaheavyforwarder) infrastructure that you want to use, you can deploy [Cribl App for Splunk](deploy-splunkapp).

- [Orchestrated deployment](orchestrated-deployment) - You can deploy Cribl Stream Leader Nodes (or single instances) and Worker Nodes via Cribl's Helm charts.

- [Docker deployment](deploy-docker) - You can deploy Cribl Stream instances using images from Cribl's public Docker Hub.
