# Choosing an Architecture

Choosing the best Cribl deployment model means methodically evaluating your organization's needs, technical limitations, and business goals. This guide offers a framework to help you make that decision.

## Deployment Model Comparison

The following table provides a high-level overview of the key differences between the available Cribl deployment models.

| Criterion | Cribl.Cloud | Customer-managed | Hybrid |
| :---- | :---- | :---- | :---- |
| **Setup Time** | Minutes to hours | Weeks to months | Days to weeks |
| **Operational Overhead** | Minimal | High | Medium |
| **Data Sovereignty** | Limited control | Full control | Configurable |
| **Scalability** | Provision extra Worker Nodes via the UI  | Manual scaling includes adding new infrastructure | Mixed approach |
| **Infrastructure Investment** | Operating expense model | Capital expenditure \+ Operating expenses | Mixed model |
| **Compliance Flexibility** | Provider-dependent (AWS/Azure) | Full customization | Selective control |
| **Multi-Cloud Support** | Native AWS/Azure supported regions | Any environment | Best of both |
| **Update Management** | Automatic | Manual coordination | Mixed approach |

## Deployment Model Decision Criteria

This table details the key criteria for selecting a deployment model, outlining which option is best for specific scenarios.

| <div style="width: 100px">Criteria</div>| Cribl.Cloud| Customer-Managed|Hybrid |
| :---- | :---- | :---- | :---- |
| **Compliance Requirements** | **Choose when:** <ul><li> Standard compliance frameworks (e.g., SOC 2, ISO 27001\) are sufficient.</li><li> Data residency is permitted within approved cloud regions. </li><li> The priority is to reduce compliance management overhead. | **Choose when:** <ul><li> Data sovereignty laws mandate local processing. </li><li> Regulatory frameworks prohibit cloud data processing. </li><li> Air-gapped environments are required, or custom compliance controls are necessary. | **Choose when:** <ul><li>  There are mixed compliance requirements across data types or business units.</li><li> A gradual migration to cloud compliance is being pursued. </li><li> Selective data sovereignty requirements exist. |
| **Performance & Operational Needs** | **Choose when:** <ul><li>  Rapid deployment and time-to-value are critical.</li><li> Operational resources for infrastructure management are limited. </li><li> Ease of scaling and updates are preferred. </li><li> Network latency to cloud regions is acceptable. | **Choose when:** <ul><li> Ultra-low latency processing is required. </li><li> Existing infrastructure investments need to be maximized. </li><li> Full operational control and custom hardware acceleration are necessary. | **Choose when:** <ul><li> Performance requirements vary across different use cases.</li><li> Local data processing with additional cloud monitoring and control. </li><li> A gradual migration from on-prem  to  cloud is in progress. |
| **Cost Optimization Factors** | **Choose when:** <ul><li>  A predictable OpEx model aligns with budgeting preferences. </li><li> The costs of managing internal infrastructure exceed cloud service premiums.</li><li>  Rapid scaling without significant capital investment is a priority. | **Choose when:** <ul><li> Existing infrastructure can be leveraged effectively.</li><li> The long-term total cost of ownership favors capital investment. </li><li> Data volumes make cloud processing costs prohibitive. | **Choose when:** <ul><li> Cost optimization is possible by placing workloads in the most economical environment, as close to the source of data as possible to reduce egress costs.</li><li> Different business units have varying cost models. Risk distribution across cost models is desired for stability. |

