# Cribl Lake 4.11.0


This feature release of Cribl Lake includes bug fixes and experience improvements.

## Important Changes

### New Cribl.Cloud Domain

With this release, Cribl.Cloud Organizations will start moving under a new, unified domain. You will be able to log in to Cribl.Cloud through https://app.cribl.cloud/, and you'll find your portal under https://app.cribl.cloud/organizations/<organizationId>.
The changes will be rolled out gradually, and the old domain will still be functional, redirecting to the new one as it is introduced.

The changes do not affect the communication between Stream Workers or Edge Nodes and the Leader Node.
You do not need to take any action to ensure the communication keeps working.

## Cribl Lakehouse

[Cribl Lakehouse](lakehouse), a new addition to Cribl Lake, lets you accelerate querying your most relevant stored data. Create scalable Lakehouses and link them with datasets to enable searches that are optimized for short latency, while full-fidelity original data rests safely in the Lake. A smart search mechanism will automatically select the most efficient option, searching either Lakehouse or Cribl Lake, whenever you run a query.

### Lakehouse Searches

For long-running [Lakehouse](lakehouse) queries, Cribl Search provides previews of intermediate results.

## Lake Partitions

Accelerated fields in Cribl Lake Datasets have been renamed [Lake partitions](managing-datasets#lake-partitions), to better align with common domain terms. Lakehouse offers a similar ability to improve query performance even further, by defining Lakehouse Indexed fields.

## Invoices and Plan Update

In Cribl.Cloud, you'll see a newly updated Plan and Invoices page. This update provides
a clearer picture of your invoice, and provides a monthly breakdown of your
credits consumed by product and infrastructure. We've updated the [Understand Your Cribl.Cloud Bill and Plan](/search/cloud-billing) docs to match.

## Suggested Git Commits

In the Git Changes modal, [Cribl Copilot](/copilot/) (where enabled) can now automatically suggest commit messages by analyzing changes against the target branch. You can enable or disable this feature at Settings > Global > Git Settings > Copilot.