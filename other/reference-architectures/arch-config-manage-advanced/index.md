# Advanced Configuration Management

Cribl offers [two approaches to configuration management](arch-configuration-management#configuration-management-approaches). The choice between these two approaches depends on your existing workflows and business requirements.

This guide explains the Pack-based configuration management approach. Cribl Packs contain end-to-end or segments of Cribl configuration content. You track and sync Packs in an external Git repository for version control. As needed, you can share Packs or port them to other environments. You can also automate Pack deployments to development, testing, or production environments using CI/CD automation tools provided by third-party Git platforms such as GitHub or GitLab.

## When to Use Pack-Based Configuration Management

Pack-based configuration management is helpful when you need standard configuration management workflows to manage multiple Worker Groups, Edge Nodes, instances, or environments (such as development, testing, staging, and production).

Prior experience with Git is helpful for this workflow. For users familiar with Git, this workflow can streamline version control, change management, and CI/CD for your Cribl environments.

This workflow is particularly beneficial for organizations that require:

- **The ability for multiple developers or teams to collaborate on configurations.** Use this workflow if you need developers to work on configurations in parallel. Each developer can work on isolated feature branches, merge changes systematically, and resolve conflicts before they impact production.
- **Two- or three-branch environment promotion and segregation workflows (Dev, Test/Staging, Production).** This workflow supports the classic development three-branch model where changes flow from `dev` to `test` to `prod` branches and environments in a controlled manner. You could also use a simpler two-branch model flow from `dev` to `prod` instead.
- **Formal configuration approval from a central Change Advisory Board (CAB).** Git workflows keep configuration changes visible so that stakeholders can review and approve them. An external Git repository can provide additional change traceability and a clear audit trail of who approved what and when.
- **Automation of configuration deployment (CI/CD integration).** Git workflows can automatically trigger deployment pipelines using CI/CD tools such as GitHub Actions, GitLab CI, Jenkins, and other tools. This enables faster, more consistent, and reliable deployments.

However, if these benefits are not important to your deployment and you would rather prioritize simplicity and ease of use, consider using the [Standard Configuration Management](arch-config-manage-standard) approach instead.

## Overview of the Pack-Based Configuration Management Approach

The recommended Pack-based configuration management workflow uses:

- **Environments:** This workflow uses an environment-based model where each environment is a separate, isolated instance of Cribl. In this context, an environment is a self-contained deployment unit where a dedicated Leader manages a consistent set of Workers or Edge Nodes. For example, you might have three instances of Cribl: a development, testing, and production environment.
- **Packs:** Packs contain the actual configuration content that you will track. They can contain the necessary data flow integrations (Sources, Collectors, Destinations) that connect Cribl to your upstream data sources and downstream data destinations. They can also contain the processing logic (Routes, Pipelines, Functions) and the supporting Knowledge Objects required to run an end-to-end deployment. You can determine the scope of the content you want to track, from full end-to-end configurations to simple segments of configurations (such as a single Pipeline or integration).
- **External Git repository:** A centralized Git repository serves as the single source of truth, tracking all configuration changes in your Pack. The repository can host branches that correlate with one the different environments: `dev`, `test`, and `prod`.

Within this Git repository, you maintain distinct branches. Each branch of your Pack correlates with a specific Cribl environment. Teams push changes through these branches in sequence: from `dev` to `test` to `prod` or in another order that matches your company's change management policy and workflow.

From there, the Leader Node distributes the configuration changes to the Worker Nodes or Edge Nodes in the data plane.

## For More Information

For more information about this approach, see:

- [Stream: Packs](/stream/packs/)
- [Stream: Collaborate on Developing Packs](/stream/packs-collaborate/)
