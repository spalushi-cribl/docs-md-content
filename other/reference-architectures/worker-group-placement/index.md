# Worker Group and Fleet Placement

From an architectural standpoint, one of your most important decisions is where to place your Worker Groups or Fleets for your Cribl deployment. This foundational decision directly impacts performance, network efficiency, security, and compliance. For that reason, this is a critical discussion to have with your security teams and C-level stakeholders.

The guiding principle when making this decision is simple: **place your Worker Groups logically and physically near the data they will process.**

This ensures that you process your data close to its source, which minimizes network latency and egress costs. A strategic placement also allows you to align your Cribl deployment with existing organizational and security boundaries, creating an architecture that is both efficient and compliant with business and regulatory requirements.

> For simplicity, this document will refer primarily to Cribl Stream Worker Groups. However, these concepts apply to Cribl Edge Fleets as well.
>
{.box .info}

## Key Constraints

As you plan the architecture for your Cribl deployment, you must first ask some critical questions to define your key architectural constraints. The answers to these questions will inform your Worker Group placement strategy and ensure you build your deployment for success from the ground up.

### Data Access and Proximity

- Where are your primary data sources located? Are they located in specific data centers or cloud regions?
- What are the network latency and bandwidth limitations between these data sources and potential Worker Group locations?

### Geographic and Jurisdictional Constraints

- Do you operate in countries with data residency or sovereignty laws that dictate where data can be stored and processed?
- How do these regulations affect your data pipelines? Can data cross borders, or must it remain within a specific country?

### Organizational Structure

- How does your corporate structure influence your data architecture, including any subsidiaries or recent mergers and acquisitions?
- Do different business units have distinct data pipelines or operational requirements that necessitate separate Worker Groups?

### Network Boundaries and Security

- What are the existing network topologies, firewalls, and VPNs that will affect communication between your data sources, the Leader Node, and the Worker Nodes?
- What are the security and access control requirements for each environment? How do you manage user access for different teams and locations?

## Recommendations for Strategic Placement

The answers to these questions should guide your final architecture. While these principles might not apply to every situation, following these general best practices will ensure your Worker Group strategy is scalable, efficient, and compliant.

### Align With Logical and Physical Boundaries

Where possible, place Worker Groups within existing organizational or geographical boundaries. As a simple rule of thumb, create one Worker Group per public cloud region or data center. This approach helps contain network traffic, simplifies access controls, and makes it easier to comply with data residency laws.

For a global organization, this might mean having a `US-East-1` Worker Group, an `EU-Central` Worker Group, and an `APAC` Worker Group. For example, if your company has a subsidiary in Germany, creating a dedicated Worker Group for its data ensures all processing remains within EU borders and satisfies GDPR requirements.

### Prioritize Network Proximity

To minimize latency and control egress costs, place your Worker Groups as close as possible to their data sources. This means a Worker Group in a specific AWS region should process data from S3 buckets, EC2 instances, or other services within that same region.

### Decouple for Strategic Control

If your reason for using Cribl products is to strategically decouple data sources from data destinations, make that a factor when placing your Worker Groups. If you have to choose between placing Worker Groups closer to a data source or a destination, then place it closer to the data source. This tends to reduce complexity and ensures better buy-in from data source owners.

### Plan for a Clear Access Model

Aligning Worker Groups with organizational structure allows you to delegate access control effectively. You can grant specific teams permissions to manage only the Worker Groups relevant to them, ensuring a clear division of responsibility and reducing the risk of unauthorized changes.
