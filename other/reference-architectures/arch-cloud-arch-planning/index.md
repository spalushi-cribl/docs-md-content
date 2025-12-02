# Cribl.Cloud Architecture Planning

This guide outlines the key architectural decisions and best practices for building a Cribl.Cloud deployment that meets the performance, compliance, and cost requirements of your organization.

## Workspace and Organizational Design

A Workspace in Cribl.Cloud is your primary organizational container that holds your configurations, Worker Groups, and data processing Pipelines. Think of it as your dedicated environment within the Cribl.Cloud platform. For details, see [Workspaces](/stream/workspaces/).

Setting up your Cribl.Cloud Workspaces to align with the operational boundaries and governance requirements of your organization is a best practice. Some common ways to design your Workspaces include the following.

### Organizational Alignment
Aligning your Workspaces with your business units, teams, or functions can give each group full ownership of its data pipelines and dashboards. This helps ensure that data is isolated and only accessible to the relevant team.

  - **Business Units**: A company might create a Sales Workspace and a Marketing Workspace, allowing each team to manage its own data independently.
  - **Team-Based**: A large organization could set up distinct Workspaces for a Security Operations team and an IT Infrastructure team to separate their data and access controls.

### Environment Separation
Using different Workspaces for development, testing, and production helps keep changes separate and lowers deployment risks. This is a common approach for managing configuration and ensuring stability.

A typical setup includes a Development Workspace for building and testing new configurations, a Staging Workspace for integration testing, and a Production Workspace for live data processing.

### Compliance Mapping
Design Workspaces with your compliance rules in mind to ensure data separation and proper access controls. This is critical for meeting business and legal requirements.

- **PCI Compliance**: A company that handles credit card data could create a dedicated PCI Data Processing Workspace to isolate this data and apply strict access controls.

- **PII Separation**: An organization might create a PII Data Workspace to ensure all personally identifiable information is handled in a separate environment with its own security and auditing policies.

### Worker Group Placement and Integration

A Worker Group is a collection of processing nodes (Workers) that execute your data transformation Pipelines. Worker Groups can be deployed across different cloud regions and are managed either by Cribl (Cribl-managed) or within your own infrastructure (Hybrid Workers). For details, see [Manage Worker Groups](/stream/worker-groups/).

Plan where you place Worker Groups for the best performance, compliance, and cost management. For details, see [Worker Group and Fleet Placement](worker-group-placement).

* **Workload Segregation**: Use separate Worker Groups for  distinct workloads to optimize workloads.   
* **Location Planning**: Deploy Worker Groups close to your data sources to optimize egress costs and minimize latency, while considering your cloud provider's regional capabilities.  
* **Cloud Provider Evaluation**: Consider how Worker Groups will integrate with your existing cloud infrastructure and network setup (VPC/VNet connections) when selecting regions for your Cribl.Cloud-managed Worker Groups. 

### Geographical and Regional Considerations

When you build a Cribl.Cloud deployment, your choices for regions are critical.

* **Leader Node Location**: When you create your Cribl.Cloud organization, the Organization region for your Leader Node is a permanent choice that you cannot change later. Cribl.Cloud Leaders are hosted only in AWS regions. For details, see [Cribl.Cloud organization Regions.](/stream/cloud-initial-setup/#region)  
* **Worker Group Regions**: You can create Cribl-managed Worker Groups in different AWS and Azure regions, which allows you to build a multi-region deployment. For supported regions, see [Cloud Worker Group Providers](/stream/cloud-workers/#providers).  
* **Native Service Integration**: Pick cloud regions close to your data Sources and Destinations. This cuts down on latency, bandwidth costs, and reduces egress costs when you connect to services like S3 or Blob Storage.  

## Architectural Decision Framework

Keep these key factors in mind when you design your Cribl.Cloud deployment.

### Resource Sizing

Size your Worker Groups and Leader correctly for the amount of data you expect to process. Making them too big costs more than you need, while making them too small can cause slowdowns and processing delays.

After initial setup, you should actively monitor the deployment for the first few weeks to establish a performance and sizing baseline. This data will help you make informed adjustments and confirm if those changes improve performance

Use the in-app Sizing calculator to determine the recommended size for your Worker Group. It will ask for your expected daily inbound and outbound data volume. You also need to select your processing load based on the complexity of your data transformations:

* Low: For simple tasks like [Masking](/stream/mask-function/) or [Renaming](/stream/rename-function/).  
* Medium: For functions like [Rollups](/stream/rollup-metrics-function/) or [Aggregations](/stream/aggregations-function/).  
* High: For resource-intensive operations like [Regex extraction](/stream/regex-extract-function/) or [custom code](/stream/code-function/).

A good rule of thumb is to consider the scope of the operation. If the function affects a single event without needing to read or evaluate its contents deeply, it is likely low-impact. If it requires a temporary memory of past events or complex logic to transform the data, it is a high-impact operation.

The calculator uses a 1:2 ingest:egress ratio to recommend a Worker Group size. Remember to account for peak loads, not just the average. You should also build in redundancy by planning to handle your peak workload even when some of your Worker Nodes are offline. For details, see [Sizing Calculator](/stream/cloud-workers/#calc).

### Data Residency

Make sure your deployment supports both business and legal rules for where data is stored and processed.

### Cost Optimization

Cribl deducts credits from your account based on the cost of the infrastructure you use and the volume of data you ingest. To minimize costs, you can deprovision a Worker Group when it is not in use. Doing so stops the charges for the underlying infrastructure and keeps the Worker Group's configuration for future use. However, it will permanently delete any data in its persistent queue. 

## Limitations and Considerations

Be aware of platform-specific limits that might affect your architectural decisions. 

* **Data Volume and Worker Limits:** The data volume and number of Worker Groups you can have depends on your plan. Free and Standard plans have daily data limits and are restricted to a single Worker Group, while Enterprise plans offer unlimited data volume and multiple Worker Groups. For details, see [Licensing](/stream/licensing). 

In an Enterprise Cribl.Cloud plan, you can create up to 10 Cloud Worker Groups per Workspace. If you need to increase this limit, you can contact Cribl Support to discuss options. Additionally, you can create Worker Groups in either AWS or Azure regions, depending on your preference. 

* **Hybrid Workers:** For deployments with strict data residency or network requirements, Hybrid Workers provide a solution that combines Cribl.Cloud management with customer infrastructure control. These are customer-managed Worker Groups that connect to your Cribl.Cloud Leader but run within your own environment (on-prem or your cloud infrastructure). 

While Cribl.Cloud manages the configuration and orchestration, you retain full control over the infrastructure and are responsible for maintenance. This approach allows you to meet compliance requirements, reduce data egress costs, and maintain network isolation while still benefiting from Cribl.Cloud's centralized management capabilities.