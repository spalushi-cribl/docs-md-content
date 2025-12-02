# Connected Environments

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

Cribl.Cloud Government Connected Environments enable you to link your on-prem Stream or Edge deployments to your Cribl.Cloud Government instance. This creates a unified operational and billing experience for your convenience. 

For details, see [Learn About Connected Environments](/stream/cloud-connected-env/).

## Hybrid Integration and Centralized Management

Agencies can connect on-prem Cribl Leaders to the Cribl.Cloud Government environment. This provides centralized, credit-based billing and enables you to use cloud-only services like **Cribl Lake** and **Cribl Search** from a single interface.

These connected environments support a **Universal Subscription**. This feature consolidates all usage and billing data both on-prem and cloud-connected under the Cribl.Cloud Government umbrella, which simplifies cost tracking and auditing.

For details, see [Simplify Billing with Universal Subscription](/stream/about-universal-subscription/).

### Compliance and Data Sovereignty

Connections are established within FedRAMP Moderate-compliant AWS Commercial US-East and US-West environments. All communications between on-prem and cloud Leaders are **encrypted via TLS**.

By default, customer data processed by on-prem Cribl instances is **not sent to Cribl.Cloud**. Only usage and billing metadata are transmitted to support credit tracking, which ensures sensitive agency data remains local unless explicitly configured otherwise.

### Use Cases

Connected environments provide a path to migrate existing on-prem Stream installations to Cribl.Cloud Government. This is also ideal for:

* **Partially air-gapped networks:** Where some parts of the environment are offline.
* **Geographically distributed setups:** To separate data collection and processing.
* **Mixed environments:** Where commercial customers have a subset of their operations tied to government contracts.