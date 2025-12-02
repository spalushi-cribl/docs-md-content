# Cribl Search 4.9


Starting with Cribl Search 4.9, you can import pre-built Dashboards from Search Packs (a Preview feature); schedule Dashboard search queries; use scheduled searches as base searches in Dashboards; optimize retrieval costs by limiting Datasets to specific storage classes; and more.

## Search Packs (Preview Feature)

Search [Packs](packs), in their first iteration,  enable you to accelerate Dashboard creation by importing, exporting, and sharing pre-built Dashboards. Selected Packs will include a secret decoder ring or toy surprise (just kidding about that part).

## Dashboard Scheduling

You can now avoid waiting for Dashboards to render on demand, by [scheduling](creating-adding-to-dashboards#scheduling) their underlying queries to rerun on a fixed interval. This option is ideal for Dashboards where you don't need to visualize up-to-the-minute values, but don't want to wait for results to display.

## Scheduled Searches as Base Searches in Dashboards

You can now [use scheduled searches](editing-dashboards#existing) as base searches for Dashboard visualizations, to render the visualizations quickly. After initial rendering, you  can customize the results as desired.

## Storage Classes Selection

You can now optimize retrieval costs by specifying which [storage classes](storage-classes) each Dataset should search. This selection is available on individual Amazon S3, Azure Blob Storage, and Google Cloud Storage Datasets. Newly created Datasets will default to the most basic storage class (`Standard` or `Hot`), with the option to add other desired classes. On existing Datasets, all storage classes are initially enabled, to minimize disruption. To optimize costs on these Datasets, you will need to disable unwanted classes through the UI.

## Improved Searching of Arrays

[Searching array values](common-examples#arrays) is now more efficient. You can reference specific elements in an array – using zero-based index notation – to avoid retrieving excess data. Where a Data Provider returns array columns as strings, we now parse these back to arrays.

## New IAM Role Options on Amazon/AWS Dataset Providers

When configuring Amazon S3 and Amazon Security Lake Dataset Providers with **AssumeRole** authentication, you can now make your IAM roles more secure by enabling attribute-based access controls [(ABAC) tagging](aws-access#trust-vpce).

## Built-in Datatypes Work with Cribl Lake Out of the Box

Cribl Search [Datatypes](datatypes) now work with [Cribl Lake Datasets](/lake/datasets) without requiring modification.

## New `range` Operator

Cribl Search now supports a version of the Kusto [`range` display operator](range-operator). This simplifies syntax when you want to create a set of sequential events in a virtual table.

## New User Interface

The UI changes that have been opt-in for Enterprise customers since release 4.5 are now the default for all users. This preview period allowed us to incorporate your feedback and optimize the layout. The main change you'll notice is that we've decluttered the top of your browser, consolidating most navigation in a collapsible left sidebar. This sidebar is context-sensitive, enabling you to access product-specific options, as well as Global Settings.

## Corrections

This release includes the following fixes:

* Corrected truncation of _time field values with fewer than 3 digits after the decimal point. (SEARCH-7460)

* On Visualizations, the Copy to Clipboard menu command now works on formerly incompatible browsers. (SEARCH-6951)

* On scheduled searches, re-enabling a disabled email notification now restores its inline results, as intended. (SEARCH-6776)
