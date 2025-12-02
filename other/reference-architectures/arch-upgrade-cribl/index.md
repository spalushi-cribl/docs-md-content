# Plan a Cribl Upgrade Strategy


It is vital to develop a strategy for upgrading your Cribl deployment when new releases come out. While Cribl is designed for high stability, product upgrades are essential for:

- Patching security vulnerabilities.
- Introducing critical performance optimizations.
- Enabling access to new features.

Ignoring new releases risks falling behind on security standards and missing out on features that could significantly lower operational costs. Therefore, you must establish a clear, documented plan that aligns Cribl's predictable release cadence with your internal change management processes.

## Cribl Release and Versioning Strategy

Cribl releases on a monthly release cycle, alternating between feature releases and maintenance releases. The release lifecycle is designed for stability and predictability, ensuring your configurations remain in place. Cribl avoids introducing breaking changes wherever possible. The goal is to give you confidence that your deployment won't suddenly stop working with routine product updates.

Cribl products follow a strict semantic versioning model:

- **Major:** Cribl reserves major version changes for significant architecture changes. They are rare and require advanced customer notification if they occur, especially if they introduce breaking changes.
- **Minor:** Cribl releases feature releases every other month as minor releases. These feature releases may introduce new functionality and improvements. Your existing integrations will usually continue to work without modification. When that is not possible, Cribl provides you with formal advance warning.
- **Patch:** Patch releases are for maintenance releases and security updates. Unplanned patches are rare and require customer notification if they occur.

## Upgrades to Cribl.Cloud and Customer-Managed Deployments

Cribl.Cloud deployments are upgraded automatically and require no action from you. Consider reading early access [product release notes](https://docs.cribl.io/results/?query=&searchOption=release-notes) or other customer communications to better understand the new features and other impacts to your system.

If your deployment is customer-managed, you are responsible for upgrading your deployment. The remaining sections explain recommended strategies for handling customer-managed upgrades.

## Best Practices for Customer-Managed Upgrades

For self-managed deployments, use a structured approach to minimize risk and downtime. Consider implementing these best practices to integrate Cribl upgrades into your existing operational processes.

### How to Decide When to Upgrade

The decision to upgrade should be driven by a balance between maintaining stability, security, and capturing new value. Not all customers upgrade with every monthly release. Instead, they use a strategy aligned with their internal compliance and risk policies:

- **Feature-Driven Approach:** Organizations using this approach closely monitor release notes to determine the optimal time to upgrade. They prioritize releases that contain critical security patches, performance optimizations, or new feature functionality that supports high-priority business requirements. This approach allows the organization to control the upgrade cadence and minimize change until they identify a compelling benefit for upgrading.
- **Policy-Driven:** Many organizations are governed by internal policies that dictate strict upgrade frequency. In this model, upgrades are treated as formal events, often requiring review and approval from a Change Advisory Board (CAB). This approach bundles several releases into a single, scheduled maintenance window to reduce the overall number of change events, ensuring the upgrade adheres to the company's mandated compliance and governance processes.

In general, Cribl recommends keeping your deployment on the latest version to ensure it is stable, secure, and that it can take advantage of the latest features.

### Ensure Leaders and Nodes Are on the Same Version

Where possible, ensure that the Leader Node and all its managed Worker Nodes or Edge Nodes are running the same software version. While Cribl is designed for high resilience, running mismatched versions introduces potential instability, especially when deploying new configuration features that may only be compatible with the latest binary. An inconsistent deployment can lead to potential configuration mismatches or unpredictable behavior.

Treat the upgrade of the Leader Node and the deployment of the new binary to all Worker Nodes as a single, coordinated change management event. Always confirm in the UI that every Worker Node or Edge Node reports the new, correct version before resuming full production load.

### Back Up Before Upgrading

Take a full, verified backup of the current Leader Node instance before initiating a major or minor upgrade. While Cribl tracks the configuration content and history locally in Git, the Leader Node holds critical state, system settings, and internal configuration files that should be protected from data loss.

### Phased Deployment and Testing

Consider using a phased approach to upgrades, such as testing upgrades in a staging environment before moving to production. Validate custom configurations, Packs, and integrations against the new binaries. Run realistic, high-volume data streams through the upgraded environment to stress-test for performance regressions or unexpected behaviors before impacting production.

Define clear exit criteria for testing each phase of the upgrade. These checks must be run both before and after the upgrade to establish a baseline and confirm success. Consider monitoring key operational metrics, including:

- Leader Node resource utilization.
- Data processing latency.
- External connection health to all major Sources and Destinations.

After the upgrade, confirm that:

- All Worker Nodes have successfully checked into the Leader and are running the correct new version.
- Data is flowing as expected, and latency metrics are within the acceptable baseline.
- The Cribl API is responding to health and status checks correctly.

## For More Information

For more information about upgrade strategies, see:

- [Upgrade Stream](/stream/upgrading/)
- [Upgrade Edge](/edge/upgrading/)
- [Better Practices for Upgrading Cribl Edge](/edge/upgrading-better-practices/)