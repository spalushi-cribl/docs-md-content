# Cribl.Cloud Security Practices


The Cribl.Cloud security architecture is designed to provide a secure and reliable platform. 
The figure below is a simplified overview of the Cribl.Cloud security architecture. The actual implementation involves additional components and processes.

![Cribl.Cloud security architecture](cloud-security-diagram.png)
{border="true"}

## Key Security Architecture Components

* **Cribl Build:** This section represents the initial phase where code is built, scanned for vulnerabilities, and deployed to the Cribl.Cloud infrastructure.
* **Cribl.Cloud (Cribl Control Plane):** The core Cribl.Cloud infrastructure managed by Cribl, where customer data and applications reside. Cribl enforces isolation per tenant within the Cribl.Cloud environment.
* **Cribl Scanning Infrastructure**: Cribl.Cloud safeguards your data through a multi-layered monitoring and detection infrastructure.
* **Security Control Plane:** The centralized management and enforcement layer in the Cribl.Cloud environment that monitors security policies, configurations, and actions and integrates various security tools and services for incident reporting. 
* **Cribl Security:** The Security Team, responsible for managing the infrastructure and providing security services.

## Data Flow 

Data flows from the Build phase to the Cribl.Cloud infrastructure, where it is processed and analyzed by the Cribl security control plane. The Cribl Security team also proactively manages the infrastructure and implements additional security controls to protect customer data. Alerts and notifications are sent to the Cribl Security team.

## Security Controls

Cribl.Cloud prioritizes data security with a comprehensive set of controls that safeguard data at rest, in transit, and during processing. These controls encompass the following.

### Secure Development

Cribl.Cloud prioritizes secure development through: 

- **Application Security Testing**: Regular code and dependency scans for vulnerabilities during the build process and nightly for Cribl.Cloud deployments.
- **Image Hardening**: Images are built with security best practices in mind to minimize potential risks.

### Continuous Monitoring and Detection

Cribl.Cloud employs a robust monitoring and detection infrastructure to identify and address potential threats:

- **Runtime Sensor**: Continuously monitors system activity for anomalies and triggers alerts.
- **Cloud Event Logs**: Captures detailed logs from cloud providers, including API calls, resource modifications, and system events. Advanced machine learning analyzes these logs to proactively detect unusual patterns indicative of security threats.
- **Vulnerability Scanning**: Regular scans identify vulnerabilities within the infrastructure.
- **File Integrity Monitoring**: Monitors file integrity to detect unauthorized changes.
- **Malware Detection**: Continuously detects and analyzes potential malware attacks.
- **End of Life Technologies Detection**: Identifies and addresses outdated technologies to mitigate security risks.
- **Sensitive Data Identification**: Proactively identifies and protects sensitive data within the platform.

### Cribl Security Control Plane

The Cribl Security Control Plane empowers users with a centralized platform for comprehensive security management:

- **Software Bill of Materials (SBOM)**: Provides a detailed inventory of all software components used within the environment.
- **Asset Inventory**: Tracks and manages all assets within the Cribl.Cloud environment.
- **Security Misconfiguration Detection and Remediation**: Identifies and addresses security misconfigurations to strengthen the overall security posture.
- **Threat Detection and Response**: Proactively detects and responds to potential security threats.
- **Threat Intelligence Integration**: Leverages threat intelligence feeds to stay informed about the latest threats and vulnerabilities.
- **Alerting**: Sends timely alerts to security teams for prompt investigation and response.
- **Security Policy Enforcement**: Enforces pre-defined security policies to maintain a secure environment.
- **Cloud Events Integration**: Integrates with cloud events for enhanced security monitoring and improved threat detection capabilities.