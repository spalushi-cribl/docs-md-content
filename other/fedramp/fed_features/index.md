# Feature Availability Matrix

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

The following matrix compares feature availability between Cribl.Cloud Government and the commercial Cribl.Cloud offering:

| Feature                         | Cribl.Cloud Government | Commercial Cribl.Cloud |
| :------------------------------ | :--------------------- | :--------------------- |
| **Core Products** |                 |                        |
| Cribl Stream                    | ✅             | ✅                    |
| Cribl Search                    | ✅             | ✅                    |
| Cribl Edge                      | ✅             | ✅                    |
| Cribl Lake                      | ✅              | ✅                    |
| Cribl Copilot                        | ❌              | ✅                    |
| Cribl Lakehouses                | ✅              | ✅                    |
| Cribl Guard (For details, see [Cribl Guard in Cribl.Cloud Government](#guard))               | ✅              | ✅                    |
| **Worker Group Hosting**       |                 |                        |
| AWS-Hosted Worker Group         | ✅             | ✅                    |
| Azure-Hosted Worker Groups       |   ❌               | ✅             | ✅        |
| **Cribl Stream/Edge Features** |                 |                        |
| Data collection                 | ✅             | ✅                    |
| Routing                         | ✅             | ✅                    |
| Transformation                  | ✅             | ✅                    |
| Sources enabled by default      | ❌ (Manual activation) | ✅                    |
| External API integrations       | ✅ (Limited)    | ✅ (Full)             |
| **Cribl Search Features** |                 |                        |
| Unified search                  | ✅             | ✅                    |
| Visualization                   | ✅             | ✅                    |
| Time-Series analysis            | ✅             | ✅                    |
| AI-Assisted search              | ❌             | ✅                    |
| Query auto-complete             | ✅             | ✅                    |
| **Security & Compliance** |                 |                        |
| FedRAMP authorization           | ✅             | ❌                    |
| Government-approved authentication | ✅             | ❌                    |
| Hardware MFA support            | ✅             | ✅                    |
| FIPS 140-3 compliance           | ✅             | ❌                    |
| Data sovereignty controls       | ✅ (Enhanced)   | ✅ (Standard)          |

## Splunk FIPS Considerations {#splunk}

When integrating Cribl.Cloud Government with Splunk, FIPS (Federal Information Processing Standards) compatibility may be a concern for on-premises deployments. While Cribl.Cloud Government Worker Groups use the newer FIPS 140-3 standard, older versions of Splunk Enterprise (pre-10.0) support only the older FIPS 140-2 standard.

To address this, customers with Splunk Enterprise versions older than 10.0 may need to deploy a hybrid Worker Group using an operating system that still supports FIPS 140-2.

Splunk Enterprise `10.0` and newer are compatible as they now support FIPS 140-3.

This concern is most relevant for on-premises Splunk deployments. Cloud-to-cloud configurations are less likely to experience significant issues. Therefore, customers with on-premises Splunk installations should plan for upgrades to ensure seamless integration.

## Cribl Guard in Cribl.Cloud Government {#guard}

There are differences between [Cribl Guard](/guard/) in Cribl.Cloud commercial and Cribl.Cloud Government environments.

### License Support

Cribl Guard is a premium feature that requires an Enterprise license for both commercial and government customers. It is not supported for Standard or Free licenses, and the Cribl.Cloud Government environment does not have these lower-tier licenses available.

### Feature Differences
In the Cribl.Cloud Government environment, all existing features of Cribl Guard work the same as in Cribl.Cloud commercial environments, with two key differences related to AI functionality:

- **AI indicators**: On the Cribl Guard homepage, indicators that show whether AI is enabled or disabled will always appear as disabled. Cribl.Cloud Government users cannot enable these features.

- **AI-powered rule building**: The **Add Rule with Copilot** option is not available. Government customers must manually build all custom Cribl Guard rules.

These AI-related features are not supported in Cribl.Cloud Government environments because Cribl AI functionality is disabled by default in FedRAMP environments. While indicators of disabled features cannot be removed, they do not hinder the overall user experience.