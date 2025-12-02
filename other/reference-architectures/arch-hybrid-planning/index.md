# Hybrid Deployment Architecture Planning

**Hybrid deployments** provide the flexibility to balance cloud scalability with on-prem deployment control. They are  ideal for organizations with diverse environments, gradual cloud migration strategies, or multi-environment needs. This guide outlines key considerations and patterns for planning hybrid architectures using Cribl Stream and Cribl Edge.

## When to Choose a Hybrid Architecture

Consider a hybrid model if you need to:

* **Balance cloud flexibility with on-prem infrastructure control:** You can manage your environment centrally in the cloud while keeping sensitive data processing on-premises for compliance or latency-sensitive workloads.  
* **Support gradual cloud migration:** A hybrid setup lets you migrate workloads in phases, ensuring operational continuity throughout the transition.  
* **Meet multi-environment requirements:** This model is perfect for organizations that operate across private clouds, public clouds, and on-premises data centers, as it provides unified management and observability.

## Hybrid Deployment Patterns

There are several ways to structure a hybrid deployment. The best pattern for you depends on your specific goals for control, scalability, and data residency.

| Deployment Pattern | Description | Use Case | Key Requirements | Advantages |
| :---- | :---- | :---- | :---- | :---- |
| **Cloud Leader with On-Prem Workers** | The Leader Node is in Cribl.Cloud, managing Workers on-premises. | Centralized cloud management with local data processing. | On-prem Workers must allow outbound communication to the Cribl.Cloud Leader on port `4200` and to Amazon S3 for bundles. Firewalls must permit outbound traffic on port `443` to `https://cdn.cribl.io` for telemetry. | Simplified management via Cribl.Cloud. Local data processing for compliance and latency. |
| **Cloud Leader with On-Prem Leaders and Workers** | A primary Leader in Cribl.Cloud manages additional Leaders and Workers deployed on-premises. | Large-scale, distributed environments requiring multiple control points. | Establish secure communication between the cloud Leader and on-prem Leaders. Configure Worker Groups to align with organizational needs. | Centralized control with distributed processing. Supports complex multi-region or multi-environment setups. |
| **Mixed Deployment Strategies** | A combination of various hybrid patterns to meet specific organizational needs. | Highly diverse environments or unique operational requirements. | Ensure proper segmentation of Worker Groups and secure communication. Use role-based access control (RBAC) to manage permissions. | Maximum flexibility for complex environments. Tailored architecture to meet specific business needs. |


## Cloud Leader with On-Prem Leaders and Workers

For organizations with expansive, complex environments, a multi-tiered approach can provide the perfect balance of centralized control and distributed processing. This is achieved by using a Cloud Leader with on-prem Leaders and Workers.  

![Connected Environments](connected-env.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/connected-env.svg). Download Cribl stencils [here](https://cribl.io/wp-content/uploads/2022/07/Cribl-Stencil-Pack-ALL.zip).
>
{.box .success}

This architecture is ideal for large, distributed enterprises that want to leverage cloud-based centralized management while maintaining local control over processing. In this model, a primary Cribl-managed Leader in Cribl.Cloud oversees your entire deployment, connecting to and managing Customer-managed Leaders that are deployed on-premises.

The on-premises Leaders then manage local Worker and Edge Nodes, which handle the data processing. This setup allows for centralized configuration management, Fleet management, and a unified view of your entire infrastructure from the cloud, while sensitive data remains on-premises for compliance or performance reasons.

This pattern also provides a flexible, scalable solution that combines the benefits of both cloud and on-premises deployments. For example, the  Cribl Universal Subscription Deployment allows a customer-managed Leader to leverage a Cribl.Cloud Leader for products like Cribl Search and Cribl Lake. In this scenario, all Worker Nodes connected to the customer-managed Leader would be priced using the Hybrid Worker Node model, with a single common credit pool for billing.

For details, see [Learn About Connected Environments](https://docs.cribl.io/stream/cloud-connected-env/).


## Key Considerations for Hybrid Deployments

When planning a hybrid deployment, addressing the following areas ensures your architecture is secure, scalable, and resilient.

### Data Transport & Routing
To optimize performance and cost in hybrid deployments, you must choose the right data transport and routing method.

- Use a **Cribl HTTP** Destination for sending data from Cribl Edge to Cribl Stream Worker Groups, especially within the same Cribl.Cloud organization, to prevent double-billing.

- Use a **Cribl TCP** Destination for on-prem to on-prem transfers or for moving data from customer-managed Workers to Cribl.Cloud. This option reduces metered data ingress and supports streaming and persistent queues.

For more detailed information, including performance tradeoffs and security considerations, see the [Internal Destinations](arch-destinations#internal-destinations).


## Security

You must secure communication between all your Leader and Worker Nodes and control who has access to your resources.

* **Encrypt Communication:** Use **TLS** to secure all communication between your Leaders, Workers, and Edge Nodes. For even stronger security, implement  **Mutual TLS Authentication**, which ensures only authorized clients with valid certificates can connect to the Leader Node.   
* **Firewall Configuration:** Ensure your firewalls allow necessary outbound traffic. This includes allowing traffic to Cribl.Cloud on port `4200` for Leader communication and port `443` for telemetry and bootstrap operations. All customer-managed nodes also need connectivity to `https://cdn.cribl.io` for updates and diagnostics. For secure routing, use **System Proxy Configuration**. 

* **Role-Based Access Control (RBAC):** Implement **RBAC** to define granular permissions for users and groups, ensuring secure management of Cribl resources.



## Scalability

Your hybrid architecture must be designed to grow with your data. Proper planning here ensures you can handle varying data loads without performance issues.

* **Worker Group Design:** Organize your **Worker Groups** to handle different data loads across environments. To maintain better resource allocation and management, keep on-prem and cloud Workers in separate groups.  
* **Centralized Control:** Using Cribl.Cloud as your Leader Node provides a centralized point of control. This simplifies configuration updates and management across distributed environments. To prepare for future data demands, monitor utilization trends and adjust your Worker Groups accordingly.

## Telemetry

Telemetry is vital for monitoring the health and performance of your system, so ensuring proper connectivity is essential.

* **Connectivity Verification:** Nodes must have connectivity to `https://cdn.cribl.io/telemetry/` for critical monitoring and diagnostics. If traffic needs to go through a proxy, set the `HTTP_PROXY` and `HTTPS_PROXY `environment variables. For details, see [System Proxy Configuration](https://docs.cribl.io/stream/proxy-config).   
* **Metadata Usage:** Cribl uses telemetry data to provide insights into system performance. Make sure your organization is comfortable with the metadata transmitted to Cribl.Cloud. For details, see [Telemetry Data](https://docs.cribl.io/stream/licensing#telemetry-data).

## Upgrading and Compatiblity

Properly managing upgrades and version compatibility is essential for maintaining the stability of a hybrid deployment.

- **Version Compatibility**: Always keep your Leaders and Workers on the same version. While Workers can be one minor version behind the Leader, they should never be ahead of it to prevent unpredictable behavior. 
- **Manual Upgrades**: Since hybrid Workers are customer-managed, their upgrades must be performed manually.

For details, see [Upgrade](/stream/upgrading/).

## Disaster Recovery

To ensure operational continuity, you need a solid plan for disaster recovery.

* **Configuration Backups:** Use remote Git repositories (private ones are recommended) to back up your configurations. This is a valuable disaster recovery mechanism in case of system failures or data loss.  
* **Persistent Queues:** Configure **Persistent Queues** to ensure data durability during unexpected outages. They act as a buffer, temporarily storing data until destinations become available. Regularly test these queues to confirm they're working as intended.

For details, see [Hybrid Deployment](/stream/cloud-enterprise/#hybrid).

