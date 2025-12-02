# Scale Configuration Management with the API

As your deployment grows, you need to develop mature configuration management systems that can scale along with your deployment. While the UI is ideal for initial setup and troubleshooting, to be able to scale a large, enterprise-level deployment, you should explore options for programmatic control using the Cribl API and other Cribl as Code tools.

## Cribl API

The Cribl API is the foundational HTTP/REST interface for all programmatic interaction, allowing for direct configuration and management of Cribl resources. It enables rapid feature building and testing by decoupling configuration from the UI.

While the API does not have full feature parity with the UI, it does offer a wide variety of workflows for many of the most common configuration tasks such as configuration updates, commits and deploys, Pack updates, and more. See [Cribl API Workflows](/cribl-as-code/workflows/) for a full list of available workflows.

## Cribl SDK

The Cribl SDKs (Software Development Kits) are a suite of language-native clients in Go, Python, and TypeScript. These SDKs can streamline the process of building sophisticated automation and custom tools for Cribl deployments. They move beyond the raw HTTP calls of the API, providing pre-built functions and objects that reduce developer effort, enforce consistency, and accelerate integrations. See [Cribl SDKs](/cribl-as-code/sdks/) for more information.

## Cribl Terraform Provider

An integration that exposes Cribl resources as declarative Infrastructure as Code (IaC) for Terraform. Defines, provisions, and manages a consistent, auditable, and repeatable infrastructure across all Cribl Organizations and Workspaces. See [Cribl Terraform Provider](/cribl-as-code/terraform/) for more information.


