# Migrate from Third-Party Agents to Cribl Edge


Migrate your data collection infrastructure from third-party agents to Cribl Edge

---

This guide provides strategic considerations for migrating your data collection infrastructure from traditional third-party agents to Cribl Edge. Whether you're migrating from Splunk Universal Forwarders (UFs), Fluentd, Filebeat, or other agent technologies, the fundamental migration principles remain consistent.

Any migration to Cribl Edge generally follows the same path:

1. Decide on your migration strategy  
2. Complete pre-migration planning  
3. Implement your Edge configuration  
4. Rollout your changes in a controlled environment

We'll examine each phase and what you need to consider along the way.

> This is a considerations guide, not step-by-step instructions. Complex migrations require careful planning and benefit from professional services engagement.
>
{.box .info}

For specific details on UF migrations, see: [Migrating from Splunk Universal Forwarders to Cribl Edge](https://knowledge.cribl.io/edge-55/migrating-from-splunk-universal-forwarders-to-cribl-edge-1722).

## Migration Strategy Overview

Your migration success depends heavily on choosing the right approach from the start. The complexity of your current deployment, available resources, and timeline constraints will all influence which path you should take. Understanding these options upfront will save time and reduce risk throughout the migration process.

### Choosing Your Migration Approach

Before beginning any migration, you must decide between two primary approaches:
- Customer-managed migration
- Professional services engagement

#### Customer-managed Migration

* **Best for:** Simple deployments with limited configuration complexity.  
* **Challenges:** Trying to migrate complex agent deployments, beyond simple deployments,  can be challenging and time-consuming. Requires deep understanding of both current infrastructure and Cribl Edge architecture.   
* **Suitable for:** Organizations with dedicated resources and Cribl Edge expertise, or proof-of-concept implementations.

#### Professional Services Engagement

* **Best for:** Large-scale or complex agent deployments.  
* **Provides:** Automated migration tools, in depth analysis,  and enhanced support.  
* **Recommended for:** Organizations requiring minimal downtime, deployments with extensive customization, time-sensitive migrations. 

## Pre-Migration Planning

This phase involves understanding your current environment, identifying potential challenges, and ensuring you have the necessary resources in place.

### Environment Assessment

The first step in migration planning is gaining a complete understanding of your current deployment. This comprehensive assessment will inform every subsequent decision and help identify potential migration challenges before they become problems.

Begin by conducting a thorough inventory of your current deployment, document both essential infrastructure components and organizational factors that could impact your migration strategy.

**Required inventory items:**

* OS types and versions across your infrastructure.  
* Current data volumes and peak load patterns.  
* Existing input configurations and custom settings.

**Additional considerations that may impact your migration:**

* Company span, subsidiaries, separate entities.  
* Network topology, connectivity, and bandwidth constraints.  
* Change control processes for your infrastructure.  
* Rollback plan documentation before beginning the migration.

### Configuration Analysis

The goal of configuration analysis is to determine the data flow and topology of your current system, so that you can use this information to inform your Fleet design. For example, in environments using centralized configuration management (like Splunk's Deployment Server), the way your current server classes are organized will significantly impact your Edge Fleet design decisions.

Key areas to document include: 

* Centralized configuration management systems and their hierarchies  
* Application or configuration packages deployed to agents  
* Client-to-configuration mappings  
* Custom configurations and transformations

For example, in Splunk UF environments specifically, you would examine:

* Deployment Server configuration using serverclass.conf  
* Splunk Apps stored in $SPLUNK\_HOME/deployment-apps  
* Client-to-serverclass mappings

### Resource Requirements Planning

Inadequate resource planning is one of the most common causes of migration failures. Cribl Edge introduces new components and data flow patterns that require careful capacity planning. Understanding these requirements upfront prevents performance issues and data loss during the migration.

#### Leader  Scaling

For Cribl.Cloud deployments, first assess your current Leader's performance before adding Edge Nodes. Evaluate whether your Leader is already overtaxed or operating smoothly, examining CPU utilization, memory usage, disk space availability, and disk I/O performance. For details, see [Monitoring Health and Metrics](monitoring).

Once you've confirmed your Leader can handle additional load, ensure Leader resources are scaled to support expected Fleet/Worker Group counts. To spin up the right number of connection processes, add the number of Edge Nodes you will deploy into the [Edge Nodes Count field](deploy-planning#configure-edge-node-count). This field crunches numbers in the background and automatically spins up the correct number of Edge connection processes for you. For on-prem deployments as well as other considerations, see [Deployment Planning](deploy-planning). 

#### Cribl Stream Worker Nodes

Ensure Stream Worker Nodes receiving data from Edge Fleets are properly sized for peak load. Undersized infrastructure can cause performance bottlenecks and data loss during high-volume periods.

#### Data Routing and Flow

Cribl Edge offers more sophisticated data routing capabilities than traditional agents, but this flexibility requires careful planning to avoid performance bottlenecks and ensure reliable data delivery.

Key considerations:

* Configure appropriate outputs based on your data routing needs  
* Consider routing complexity when routing from Cribl Edge to Cribl Stream then to Destinations  
* Plan for backpressure management, especially when routing to multiple Destinations

## Understanding Architectural Differences

Most third-party agent migrations involve fundamental architectural changes that affect every aspect of your migration planning. Understanding these differences is critical for designing an effective migration strategy.

### Configuration Management Models

Traditional agents often use different configuration management approaches than Cribl Edge.

#### Composition vs. Inheritance

* **Traditional agents:** Many use composition models where a single agent can belong to multiple configuration groups or classes  
* **Cribl Edge:** Uses inheritance where one Edge Node belongs to a single Fleet/Subfleet

For example, in Splunk UF environments, one UF can belong to multiple serverclasses, but in Cribl Edge, one Edge Node belongs to a single Fleet/Subfleet.

#### Fleet Design Strategy

Fleet design is perhaps the most critical decision in your migration. Poor Fleet architecture can lead to management complexity, performance issues, and operational challenges. The goal is to balance functionality with maintainability while leveraging Cribl Edge's strengths.

##### Fleet Rationalization Approach

* Review your current configuration management to understand what needs to be collected  
* Use tags, Pipelines, and Cribl Stream to reduce configuration variability  
* Consolidate similar configurations to minimize Fleet count  
* Group by function rather than direct configuration translation

##### Fleet Count Optimization

* Keep Fleet counts to a minimum for optimal performance  
* Avoid direct translation of existing configuration groups to Edge Fleets (leads to Fleet proliferation)

> Direct translation of existing configuration management structures to Edge Fleets often leads to management complexity and potential performance issues.
>
{.box .warning}

* Consider creating Fleets for unique combinations of configuration requirements only when necessary

### Configuration Translation Challenges

Understanding these caveats upfront allows you to plan alternative approaches and avoid migration roadblocks.

#### Single-Source Support Limitations

Some Cribl Edge input sources allow only one Source per Fleet:

* System State  
* Windows Metrics

Agent configurations that convert to these equivalents must be merged, potentially requiring conflict resolution.

#### Automatic Metadata Population

One advantage of Cribl Edge is its automatic handling of common metadata fields, reducing configuration complexity compared to traditional agents. Cribl Edge automatically populates these metadata fields without additional configuration:

* `sourcetype`  
* `index`  
* `source`  
* `host`  
* `connection_host`

### Configurations Requiring Alternative Approaches

Some agent configurations have no direct equivalent in Cribl Edge and require additional steps.  Common examples include:

* **Path-based host extraction:** Some agents (like Splunk UF) can automatically extract the hostname from the file path itself. For example, the `host_regex` and `host_segment` settings from UF don't translate directly. However, you can achieve the same functionality using Cribl Edge's Pipeline processing to extract and transform the hostname data as needed.  
* **File handling behaviors:** Some agents often have granular control over how they interact with files. Cribl Edge may not have equivalent settings for every file handling behavior. For example, symbolic linking or file opening behavior.   
* **Custom transformations:** Some agents can perform data transformations at the collection point. Cribl Edge operates on a different model where most data transformation happens in Cribl Stream rather than at the edge. While Cribl Edge can do some processing, it's not designed to replicate all the transformation capabilities that agents might have built-in.

#### Alternative Implementation Methods

* Create corresponding Pipelines in Cribl Edge 
* Use Edge Node metadata mapping for system information 
* Implement data transformation through Routes (all inputs forward to Routes)

## Migration Implementation Considerations

Once you've completed your planning phase, the implementation phase requires attention to configuration details and platform-specific considerations.

### Configuration Review Areas

Each configuration area requires careful review to ensure optimal performance and functionality.

#### File Monitor Settings

* Determine whether to start from the beginning or end of files  
* Consider the impact of large file sizes or many old events  
* Assess network bandwidth and Destination capacity implications

#### System State Configuration

The System State Source is enabled by default on Cribl Edge Fleets, but you can disable the Source or specific collectors if System State data is not needed.

#### Pipeline Configuration

* Review existing agent configurations and enable similar functionality in Cribl  
* Optimize regexes for performance  
* Consider data transformation requirements previously handled in agents

#### Persistent Queue Management

* Review Persistent Queue settings in your agents, Cribl Edge, and Stream  
* Ensure proper queue sizing for expected data volumes

### Platform-Specific Considerations – Windows Environments

Windows environments present unique challenges during migration, particularly around policy management.

#### Group Policy Objects

Check for Group Policy Objects (GPOs) that might automatically reinstall or enable removed third-party agents, and update policies before migration to prevent conflicts.

#### PowerShell Usage

For optimal performance on Windows Edge Nodes, ensure the "Use Windows Tools" setting is disabled for all data Sources. When disabled, the system uses native Windows Management Instrumentation (WMI) API methods for faster and more reliable data collection. When enabled, the system falls back to legacy PowerShell cmdlets.

The PowerShell-based collection methods will be removed in future releases as native methods provide superior speed and reliability.

#### Policy Management

Some sites might have policies to re-enable/re-install agents if missing/removed. Update or exclude policies before migration.

#### Windows Event Log Read Mode

When configuring the **Windows Event Log** Source, always review the **Read mode** setting. The default is **From last entry**, which only collects new events. While this is the recommended setting, you have the option to change it to read the **Entire log**. For machines that have been in use for a long time, this can cause a massive, one-time spike in data ingestion as it sends years of historical events. This sudden influx can easily overwhelm your infrastructure, causing performance issues and data loss. Sticking with the default setting helps you avoid this risk.

### Addressing Inheritance Limitations

When working with Edge's inheritance model, be aware of these challenges:

* **Configuration integrity**: Changes to a Source/Destination in a Subfleet can break inheritance from the parent  
* **Limited flexibility**: Sources that allow only one configuration at a time create inheritance challenges when customization is needed

## Migration Rollout Strategy

The rollout phase is when your plans become reality. A good rollout ensures things go smoothly, problems are fixed fast, and your data collection isn't interrupted.

### Controlled Rollout Process

A controlled rollout allows you to validate your migration approach on a small scale before committing to full deployment. This approach significantly reduces the risk of widespread issues and provides opportunities to refine your process.

#### Progressive Batch Sizing

* Start with test servers  
* Then proceed to small batches (e.g., 200 machines)  
* Progress to larger batches (400, 800, 2000 machines, etc.)  
* Validate system performance and data flow at each stage  
* Only proceed when previous stage is validated

#### Validation Requirements

* Execute controlled rollout with proper validation at each stage  
* Address any issues early in rollout before expanding  
* Maintain visibility into migration rollout progress

### Continuous Monitoring

To ensure a smooth migration, you need to monitor everything. This means watching both how things are performing technically and making sure your important data is flowing correctly. Additionally, monitor the Edge Nodes themselves as they get onboarded to track their connectivity status. Cribl Edge v.4.13 introduced Disconnected Node Tracking to help you identify and troubleshoot nodes that lose connection during the migration process.

#### Resource Utilization

Monitor Cribl Edge, Leader, and Stream components continuously to watch for performance bottlenecks or data loss indicators. Validate data flow and collection accuracy throughout the process.

#### Rollback Capabilities

Maintain (previously-documented) rollback capabilities in case issues are discovered. 

## TL;DR 

Migrating from third-party agents to Cribl Edge requires careful planning and a systematic approach. The key to success lies in understanding the architectural differences between your current system and Cribl Edge, properly planning your Fleet design, and executing a controlled rollout process.

Remember that this is a considerations guide, and complex migrations often benefit from professional services engagement to ensure success and minimize risk to your data collection infrastructure.