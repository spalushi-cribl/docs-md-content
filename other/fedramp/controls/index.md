# Security and Compliance

This topic provides an overview of the security controls and compliance measures implemented within Cribl.Cloud Government, highlighting its FedRAMP Moderate authorization and data protection mechanisms for Federal users.

## FedRAMP Moderate Controls Overview

Cribl.Cloud Government is authorized at the FedRAMP Moderate impact level, implementing all required controls across the following families:

| AC: Access Control               | AU: Audit and Accountability      | AT: Awareness and Training        |
| :------------------------------- | :------------------------------ | :------------------------------ |
| CM: Configuration Management     | CP: Contingency Planning        | IA: Identification and Authentication |
| IR: Incident Response            | MA: Maintenance                 | MP: Media Protection              |
| PE: Physical and Environmental Protection | PL: Planning                    | PS: Personnel Security          |
| RA: Risk Assessment              | CA: Security Assessment and Authorization | SC: System and Communications Protection |
| SI: System and Information Integrity |                               |                                 |

The authorization covers:
* All infrastructure components within the FedRAMP boundary
* Data storage and processing facilities
* Authentication and authorization services
* Administrative access mechanisms

## Authorization Status

Cribl.Cloud Government has achieved FedRAMP Moderate authorization through a thorough assessment process:
* Full Security Assessment by an accredited 3PAO
* Regular continuous monitoring activities
* Annual reassessment of all applicable controls
* Plan of Action and Milestones (POA&M) management

## Data Protection Mechanisms

Multiple layers of protection safeguard Federal data:
* **Data-in-Transit:** Encrypted using TLS 1.2+ with FIPS-validated ciphers
* **Data-at-Rest:** Encrypted using AES-256 with FIPS-validated modules
* **Data Isolation:** Multi-tenant architecture with strong logical separation
* **Data Minimization:** Controls to limit collection of unnecessary information

Additional protections include:
* Field-level masking for sensitive information
* Anonymization options for personally identifiable information
* Automated data lifecycle management
* Secure deletion procedures compliant with Federal requirements

## Data Flow Security

Data is protected throughout its lifecycle within Cribl.Cloud Government:
* **Collection:** Secure transport protocols with certificate validation
* **Processing:** Memory-safe operations with access controls
* **Storage:** Encrypted data stores with key management
* **Analysis:** Access-controlled query mechanisms
* **Archival/Deletion:** Secure data destruction procedures

## Encryption and FIPS Compliance

All cryptographic operations adhere to Federal standards:
* **FIPS 140-3:** Compliance with Federal Information Processing Standards
* **Encryption Algorithms:** AES-256, RSA-2048 or better
* **Digital Signatures:** SHA-256 or better
* **Key Management:** NIST-compliant key generation and rotation procedures

Cryptographic implementations feature:
* Regular validation against NIST standards
* Secure key storage in hardware security modules where applicable
* Automated key rotation procedures
* Comprehensive logging of cryptographic operations

## Encryption Implementation Details

| Data Type         | Encryption Method           | Key Management                  |
| :---------------- | :-------------------------- | :------------------------------ |
| Data-at-Rest      | AES-256-GCM               | AWS KMS (FIPS validated)        |
| Data-in-Transit   | TLS 1.2+ with FIPS ciphers | Automated certificate management |
| Authentication Data | Salted with PBKDF2          | Stored in secure credential store |
| Backup Data       | AES-256                   | Separate key management system    |

