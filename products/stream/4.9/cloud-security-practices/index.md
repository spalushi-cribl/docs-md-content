# Cribl.Cloud Security Practices


The Cribl.Cloud security architecture is designed to provide a secure and reliable platform for customers. 
The figure below is a simplified overview of the Cribl Cloud security architecture. The actual implementation involves additional components and processes.

![Cribl.Cloud security architecture](cloud-security-diagram.png)
{border="true"}

### Key Security Architecture Components

* **Cribl Build:** This section represents the initial phase where code is built, scanned for vulnerabilities, and deployed to the Cribl Cloud infrastructure.
* **Cribl.Cloud (Cribl Control Plane):** The core Cribl.Cloud infrastructure managed by Cribl, where customer data and applications reside. Cribl enforces isolation per tenant within the Cribl.Cloud environment.
* **Cribl Scanning Infrastructure**: Cribl.Cloud safeguards your data through a multi-layered monitoring and detection infrastructure.
* **Security Control Plane:** The centralized management and enforcement layer in the Cribl Cloud environment that monitors security policies, configurations, and actions and integrates various security tools and services for incident reporting. 
* **Cribl Security:** The Security Team, responsible for managing the infrastructure and providing security services.

### Data Flow 

Data flows from the Build phase to the Cribl.Cloud infrastructure, where it is processed and analyzed by Cribl’s security control plane. The Cribl Security team also proactively manages the infrastructure and implements additional security controls to protect customer data. Alerts and notifications are sent to the Cribl Security team.


### Security Controls

Cribl.Cloud prioritizes data security with a comprehensive set of controls that safeguard data at rest, in transit, and during processing. These controls encompass the following.

#### Secure Development

Cribl.Cloud prioritizes secure development through: 

- **Application Security Testing (AST)**: Regular code and dependency scans for vulnerabilities during the build process and nightly for Cribl Cloud deployments.
- **Image Hardening**: Images are built with security best practices in mind to minimize potential risks.

#### Continuous Monitoring and Detection

Cribl.Cloud employes a robust monitoring and detection infrastructure to identify and address potential threats:

- **Runtime Sensor**: Continuously monitors system activity for anomalies and triggers alerts.
- **Cloud Event Logs**: Captures detailed logs from cloud providers, including API calls, resource modifications, and system events. Advanced machine learning analyzes these logs to proactively detect unusual patterns indicative of security threats.
Vulnerability Scanning: Regular scans identify vulnerabilities within the infrastructure.
- **File Integrity Monitoring**: Monitors file integrity to detect unauthorized changes.
- **Malware Detection**: Continuously detects and analyzes potential malware attacks.
- **End of Life Technologies Detection**: Identifies and addresses outdated technologies to mitigate security risks.
- **Sensitive Data Identification**: Proactively identifies and protects sensitive data within the platform.

#### Cribl Security Control Plane

The Cribl Security Control Plane empowers users with a centralized platform for comprehensive security management:

- **Software Bill of Materials (SBOM)**: Provides a detailed inventory of all software components used within the environment.
- **Asset Inventory**: Tracks and manages all assets within the Cribl.Cloud environment.
- **Security Misconfiguration Detection and Remediation**: Identifies and addresses security misconfigurations to strengthen the overall security posture.
- **Threat Detection and Response**: Proactively detects and responds to potential security threats.
- **Threat Intelligence Integration**: Leverages threat intelligence feeds to stay informed about the latest threats and vulnerabilities.
- **Alerting**: Sends timely alerts to security teams for prompt investigation and response.
- **Security Policy Enforcement**: Enforces pre-defined security policies to maintain a secure environment.
- **Cloud Events Integration**: Integrates with cloud events for enhanced security monitoring and improved threat detection capabilities.

### Cribl.Cloud/Customer Responsibilities

This matrix outlines the shared responsibilities between Cribl.Cloud and the customer for various aspects of the Cribl platform.

| Responsibility Area | Cribl.Cloud | Customer |
|---|---|---|
| Infrastructure & Network Management | Manage all configurations and network connections in the dataplane to manage Cribl product suite | Manage customer-managed network devices and infrastructure (Hybrid). |
| Data Flow Management | Manage data flow from Sources to Cribl.Cloud and Cribl.Coud to Destinations | Manage data flow configurations and software settings |
| Security Management | Implement infrastructure security, audit, and compliance (Cribl.Cloud environment) | Implement  security for customer-managed infrastructure (Hybrid). |
| Compliance  | Cribl is SOC 2 and ISO 27001 compliant | Implement secure data management practices (data locality, transport, disposal) |
| Service Infrastructure | Set up default key management and certificate upgrades | Configure Data Flow on Workers (Pipelines & Routes) (in Cribl.Cloud & Hybrid) |
| User Access Management | Provide authentication mechanisms for customers | Provision and assign user roles and permissions |
| Service Upgrades & Maintenance | Orchestrate service upgrades, patch, and maintenance for Cribl.Cloud | Orchestrate service upgrades, patch, and maintenance for customer-managed infrastructure (Hybrid). |
| Monitoring & Incident Response | Implement continuous monitoring and incident response on Cribl. Cloud assets | Implement monitoring and incident response on integration assets not hosted by Cribl Cloud |
| Compliance | Maintain compliance certifications and attestations for Cribl.Cloud | Comply with relevant security standards and regulations |
| Cribl.Cloud Service Lifecycle | Develop software application to be deployed to Cribl.Cloud instances | Sign up for Cribl.Cloud and initial set up |
| Network Access Control | Manage inbound and outbound firewall rules (Cribl.Cloud can open ports based on customer action) | Manage inbound and outbound firewall rules |

Keep in mind the following: 
- Customers are responsible for enforcing secure communication over secure channels using Cribl software.
- Customers are responsible for restricting application-level access controls within Cribl.
- Customers are responsible for configuring integration accounts using least privilege access

### Cribl.Cloud Benefits 

Cribl Cloud is committed to providing a secure and reliable platform for our customers. We prioritize data privacy and security by implementing robust measures to protect your sensitive information at every stage.

Cribl Cloud offers a comprehensive security posture built on several key pillars:

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

