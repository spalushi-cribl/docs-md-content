# Configuration Management

As you prepare to deploy Cribl, you need a clear configuration management strategy. Cribl products like Cribl Stream and Cribl Edge can operate as low-touch tools, which means that the heavy work usually occurs during their initial setup. However, you must approach the ongoing management and maintenance of these tools with a thoughtful, intentional strategy.

The core configuration challenge when working with Cribl tools isn't in optimizing a single set of configurations alone. Rather, it is the challenge of making configuration changes across a large deployment over time. Overcoming this challenge is crucial to preventing configuration drift and ensuring the integrity of your data pipelines.

## Configuration Management Goals

Identify your configuration management goals and objectives as you plan your strategy. An effective configuration management strategy is:

### Consistent

Configurations should be consistent across your deployment to prevent configuration drift. This ensures your data is handled uniformly across all Worker Groups and Fleets, eliminating unpredictable behavior and simplifying troubleshooting.

The Cribl Leader Node in the control plane enforces consistency. It acts as the single source of change management control for all Worker or Edge Node configurations in the data plane. Once you deploy a configuration from the Leader, all managed Nodes have consistent configurations and process data using the same identical specifications.

### Auditable

You need to provide a complete, version-controlled history of every configuration change. This creates a clear audit trail of who made a change, when they made it, and what they changed. Version control is critical for centralized change management because it allows for internal review and compliance with your company's change management policy.

Cribl manages all configuration files in a Git repository, either locally on the Leader Node or in Packs that are version-controlled in an external Git repository. In addition to providing the ability to roll back configuration changes as needed, version control provides an audit trail for all changes so that you can identify who made changes and when.

### Scalable

You need to efficiently deploy and update configurations quickly and at scale so that your Cribl deployment can grow or adapt with changing business needs and priorities or if a Git repository that is local to your Cribl deployment is sufficient.

Either from the initial design or over time, you can use the Cribl API, Cribl SDK, Cribl Terraform provide and CI/CD integration to make programmatic changes or use automation.

### Compliant

While planning and deploying Cribl is mostly a technical task, it also involves thinking about the larger business system and goals that your Cribl deployment will integrate with. Your strategy should harmonize with your company's current change management policies and procedures, which may mean researching your company's current workflows.

Cribl provides different approaches for integrating with existing business workflows that can be adapted as needed. See [Configuration Management Approaches](#configuration-management-approaches) for an explanation of the different approaches you can adapt to your business needs.

## Core Configuration Content to Manage

When planning your configuration management strategy, identify which configuration content needs to be managed and version controlled. There are three basic categories of configuration content that you typically need to manage in your Cribl deployment:

- **Data Flow Integrations:** The configurations for Sources, Collectors, and Destinations connect Cribl to your upstream data sources and downstream data destinations.
- **Processing Logic:**  The configurations for Routes, Pipelines, and Functions contain the rules and logic that filter and transform your data.
- **Supporting Knowledge Objects:** These are the reusable assets that apply context and structure to your raw data, including Event Breakers, lookup files, HMAC Functions, and more.

Your configuration management strategy should account for ways to manage all these different types of content.

## Cribl Configuration Management Components

In the [Cribl Three-Plane Model](core-arch-concepts), the control plane is the architectural layer that handles configuration management. It includes the Leader Node, the UI, and the API:

- **Leader Node:** The central hub for all configuration management. It connects to the UI and API where the configuration changes are made. It tracks configuration changes in a Git repository and then pushes changes to the data plane. It also distributes any Knowledge Objects that are necessary for data handling, such as Event Breakers or lookup files.

- **UI:** The Leader Node connects directly to the user interface. The UI is particularly useful for guiding you through the initial configuration setup and for making ad-hoc changes.

- **API:** The programmatic engine for automation. When your initial configurations are working as expected, you can begin optimizing and rolling out configurations at scale using the API. While you can handle the initial setup or make ad-hoc changes through the UI, the API is the recommended interface for deploying changes in a production environment at scale.

When you commit and deploy configuration changes, the Leader propagates changes to the data plane. The data plane is the layer of architecture that handles data, depending on the product:

- **Cribl Stream:** Worker Groups that contain Worker Nodes that process data as configured. Cribl Workers distribute events across available Worker Processes (CPUs) on each Worker.

- **Cribl Edge:** Edge Nodes that collect data from the local filesystem, local APIs, and local system state. Edge Nodes can also handle data similar to Cribl Stream with lower resource needs.

- **Cribl Search:** Operators that execute search queries across different Dataset Providers.

- **Cribl Lake:** Storage buckets and Datasets, which are unique partitions within a storage bucket.

## Configuration Management Approaches {#configuration-management-approaches}

Cribl offers two primary approaches to configuration management. The choice between these two approaches depends on your existing workflows and business requirements. Before deciding on an approach, working with key stakeholders from within your company, define the hard and soft requirements that your change management system must meet. An effective change management plan complies with company policies and integrates Cribl into existing business workflows and tools.

Once you have identified your hard and soft requirements, select and approach that best meets your needs.

> These two approaches are not mutually exclusive. You can use them together or you can start with the standard approach to build your initial configuration management system and then expand to the advanced approach later.
>
{.box .info}

### Standard Approach: The Cribl Control Plane

In this approach, the Leader Node acts as the central configuration management hub. You make configuration changes through the Cribl UI or direct API calls to the Leader Node. The Leader tracks changes in its local Git repository and distributes the configuration bundle to Worker Groups or Fleets upon a commit and deploy.

The advantages of this approach:

- It's the out-of-the-box solution.
- Simple initial setup.
- Immediate access to rollback features built into the UI.

This approach is good for smaller deployments where you need to prioritize simplicity and ease of use over scalability.

Read an overview of the [Standard Configuration Management](arch-config-manage-standard) for more information about this approach.

### Advanced Approach: Pack-Based Configuration Management

In this advanced approach, Cribl Packs contain end-to-end configuration content or segments of Cribl configuration content. You track and sync Packs in an external Git repository for version control. As needed, you can share Packs or port them to other environments. You can also automate Pack deployments to development, testing, or production environments using CI/CD automation tools provided by third-party Git platforms such as GitHub or GitLab.

The advantages of this approach:

- It's an enterprise-grade solution.
- Supports three-branch environment promotion and segregation workflows (`dev`, `test`, `prod`).
- Allows multiple developers or teams to collaborate on Cribl configurations.
- Enables full auditability.
- Integrates with existing DevOps toolchains (such as CI/CD).

This approach is good for decentralized, automated, and highly compliant change management systems.

Read an overview of the [Advanced Configuration Management](arch-config-manage-advanced) for more information about this approach.

