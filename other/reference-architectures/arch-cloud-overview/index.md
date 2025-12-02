# Cribl.Cloud Architecture Overview


Cribl.Cloud is Cribl's fully managed, Software-as-a-Service (SaaS) deployment model. It provides the complete Cribl platform without the operational complexity of self-managed infrastructure. This cloud-native approach enables you to shift you focus from infrastructure management to data strategy and outcomes.

## Key Terminology

| Term | Definition |
| ----- | ----- |
| **Account** <br> (AWS Account, Azure Subscription) | A container for Cribl.Cloud Organizations. An AWS Account or Azure Subscription is created specifically to host the infrastructure for Cribl.Cloud Organization resources.  |
| **Management Plane** <br> | Orchestrates the services of all Organizations and Workspaces within Cribl.Cloud. This encompasses the internal admin API and the end-user facing API.|
| **Control Plane** <br>(Leader)| The configuration interface (UI & API) for Stream, Edge, and Search. From an API perspective, interacting with the Control Plane means configuring the Workspace. |
| **Data Plane** <br> (Worker Nodes, Worker Group, Edge Nodes, Fleets) | The components responsible for data ingest, processing, and egress. |
| **Member** | A User who has been granted access to an Organization with a set of Roles that define their access privileges. |
| **Organization** <br> (Tenant) | The top-level Cribl.Cloud entity created to provide access to Cribl Products. An Organization is hosted within an AWS Account, and its primary identifier (organizationId) cannot be changed after creation. It has an immutable 1:1 relationship with an AWS Account during its active lifecycle. |
| **Owner** | The first User assigned to an Organization, who has access to higher-order administration functions such as Billing Information and inviting new Members. |
| **Plan**  <br> (License) | The pricing and billing plan the product is on. |
| **Product** <br> (Stream, Edge, Search, Lake) | A standalone Cribl offering made available to members of an Organization. |
| **Profile** | A User profile within Cribl.Cloud. |
| **Role** | A function assumed by a Member of an Organization that defines their access privileges. |
| **User** <br> (Identity, Auth0 User) | A person who has registered for access to Cribl.Cloud. |
| **Workspace**  | A logical location within an Organization where instances of multiple Cribl Products are deployed. A Workspace is comprised of the Stream Control Plane, Edge Control Plane, Data Plane, and Search. For details, see [Workspaces](/stream/workspaces/). |

## Understanding Cribl.Cloud Architectural Model {#model}

The platform maintains the familiar three-plane architecture while introducing specific cloud-native operational characteristics:

* **Management Plane:** Exclusive to Cribl.Cloud, the management plane includes the services required to create new Organizations, provide access control and authorization, provision and manage Workspaces, and handle billing, metering, and cost controls. This plane is largely independent of individual products and services.  
* **Control Plane:** The Leader manages the platform's underlying infrastructure, updates, and operations. You should retain complete control over your data processing configurations, pipeline logic, and routing decisions.  
* **Data Plane:** Customer data processing occurs exclusively within customer-controlled cloud environments. The data plane (Worker/Edge Nodes) captures and processes data, then sends it directly back to the control plane (Leader) without passing through Cribl's management infrastructure. This architecture ensures data sovereignty compliance while allowing centralized configuration management through the control plane.    

The platform operates across multiple cloud regions and providers, enabling organizations to process data close to its source. Commands flow from the control plane through the management API, which handles authorization and orchestration, while the actual captured data flows directly between Workers and Leaders, bypassing the management infrastructure entirely.

### Domains

Cribl.Cloud is deployed across multiple domains, which act as distinct, isolated environments for the service. 

The platform does not use traditional "environments" like `prod`, `staging`, and `dev`; these are instead logical constructs for human use. The code only recognizes different domains, which are all built with the naming scheme `cribl-${HumanName}.cloud`. 

This foundational construct enables a unified approach to managing and replicating environments. The domain field is propagated throughout the system and is used for tagging all observability data, ensuring it is routed correctly.

| Domain | Purpose |
| ----- | ----- |
| `cribl.cloud` | Commercial Production |
| `cribl-gov.cloud` | Federal Production |

### High-Level Components

Cribl.Cloud is designed as a hybrid of multi-tenant and single-tenant services. The platform's high-level components are structured as follows:

* **Multi-tenant Services:** The Cribl.Cloud Management Plane is a multi-tenant service hosted in the Cribl AWS account. All end-users interact with this single deployment of services, which orchestrates the management of all Organizations and Workspaces.  
* **Single-tenant Services:** Core Cribl Products (Stream, Edge, Search, Lake, and LakeHouse), along with their supporting infrastructure, are deployed on a per-Organization basis into a dedicated AWS account. This account is associated with only one Organization, and end users are not granted direct access to it. Access to these products is provided through ingress and egress services within a Workspace, which is composed of the Stream Control Plane, Worker Groups (Data Plane), Search, and Lake services.

Because distinct Stream services are provisioned as a Workspace for each Organization, a sub-domain is created for each Organization as needed. The naming scheme is as follows:

| FQDN | Purpose | Example |
| ----- | ----- | ----- |
| `manage.cribl.cloud` | Cloud Portal UI |  |
| `api.cribl.cloud` | Management Plane API |  |
| `manage.cribl.cloud/${organizationId}` | Organization & Workspace Management UI | `manage.cribl.cloud/goofy-panini` |
| `${workspaceName}-${organizationId}.cribl.cloud` | Control Plane UI & API | `main-goofy-panini.cribl.cloud` |
| `{groupName}.{workspaceName}.{organizationId}.cribl.cloud:{port}` | Ingest Address (Worker Group/Fleet) | `default.main.goofy-panini.cribl.cloud:10080` |