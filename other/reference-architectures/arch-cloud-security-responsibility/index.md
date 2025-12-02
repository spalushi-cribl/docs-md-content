# Cribl.Cloud/Customer Responsibilities


This matrix outlines the shared responsibilities between Cribl.Cloud and you for various aspects of the Cribl platform.

| Responsibility Area | Cribl.Cloud | Customer |
|---|---|---|
| Infrastructure & Network Management | Manage all configurations and network connections in the data plane to manage Cribl product suite | Manage customer-managed network devices and infrastructure (Hybrid). |
| Data Flow Management | We actively manage your data flow from Sources to Destinations, handling everything from backpressure events to data routing and load balancing. We also take care of all software updates and configurations to ensure your data Pipelines run smoothly and without interruption.  | Manage data flow configurations and software settings.|
| Security Management | Implement infrastructure security, audit, and compliance (Cribl.Cloud environment). | Implement security for customer-managed infrastructure (Hybrid). |
| Compliance  | Cribl.Cloud is SOC 2 and ISO 27001 compliant. | Implement secure data management practices, which means put safeguards in place for where your data is stored (data locality), how it's moved (transport), and when it's deleted (disposal). |
| Service Infrastructure | Set up default key management and certificate upgrades. | Configure Data Flow on Workers (Pipelines & Routes) (in Cribl.Cloud & Hybrid). |
| User Access Management | Provide authentication mechanisms. | Provision and assign user roles and permissions. Manage API credentials (keys, tokens, client secrets) used by integrations, including scoping permissions, rotation, and revocation. |
| Service Upgrades & Maintenance | Orchestrate service upgrades, patches, and maintenance for Cribl.Cloud. | Orchestrate service upgrades, patches, and maintenance for customer-managed infrastructure (Hybrid). |
| Monitoring & Incident Response | Implement continuous monitoring and incident response on Cribl.Cloud assets. | Implement monitoring and incident response on integration assets not hosted by Cribl.Cloud. |
| Cribl.Cloud Service Lifecycle | Develop software application to be deployed to Cribl.Cloud instances. | Sign up for Cribl.Cloud and initial setup.|
| Network Access Control | Manage inbound and outbound firewall rules (Cribl.Cloud can open ports based on customer action). | Manage inbound and outbound firewall rules. |

You are responsible for:
- Enforcing secure communication over secure channels using Cribl software
- Restricting application-level access controls within Cribl.
- Configuring integration accounts using least privilege access.
- Managing the lifecycle of API credentials (keys, tokens, client secrets) used by Cribl integrations: Create, scope permissions, rotate regularly, revoke when no longer needed, and store in approved secret management solutions.

## Cribl.Cloud Benefits 

Cribl is committed to providing a secure and reliable platform. We prioritize data privacy and security by implementing robust measures to protect your sensitive information at every stage.

Cribl.Cloud offers a comprehensive security posture built on several key pillars:

* **Robust Security Posture:**
    * Configuration and vulnerability management to proactively identify and address potential weaknesses.
    * Endpoint security to safeguard your data at its source.
    * Incident response plans to minimize the impact of security breaches.
    * Threat intelligence to stay ahead of evolving threats.

* **Comprehensive Data Protection:**
    * Limited metadata collection to ensure only essential data is analyzed.
    * Robust encryption techniques to protect data at rest and in transit.
    * Strict access controls to limit access to sensitive information.

* **Proactive Threat Management:**
    * Continuous infrastructure monitoring for potential threats and vulnerabilities.
    * Advanced threat detection tools for prompt identification of security incidents.
    * 24/7 managed security operations center for real-time response and mitigation.

* **Industry-Leading Compliance:**
    * Adherence to industry standards and regulations, including SOC 2, to ensure the security and privacy of your data.

By prioritizing security and data privacy, Cribl.Cloud provides a trusted platform for your data processing and analytics needs.