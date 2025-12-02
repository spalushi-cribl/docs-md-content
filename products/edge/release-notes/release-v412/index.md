# Cribl Edge 4.1.2


## New Features 

This release provides the following improvements:

Edge is edgier than ever before thanks to the new [Prometheus Edge Scraper](/edge/sources-edge-prometheus) Source. In addition to the functionality already supported in the existing [Prometheus Scraper](sources-prometheus), this Source is  designed to work seamlessly in Kubernetes environments; and no longer uses internal jobs framework allowing it to handle large-scale Cribl Edge deployments.

## Breaking Changes 

> As of 4.1.2, the [Prometheus Scraper](sources-prometheus) Source is deprecated on Cribl Edge, and will be deleted from Cribl Edge by v.4.2 in July 2023. Use the [Prometheus Edge Scraper](/edge/sources-prometheus-edge) instead.
>
{.box .warning}



## Corrections 

This release includes the following fixes:

CRIBL 16227 The Bootstrap script now supports special characters.

CRIBL 17107 The File Monitor Source can now detect logs in soft-linked directories.

CRIBL-16370 Multiple File Monitor Sources can now be configured, with data capture enabled.

SAAS-3981 [Cribl Internal](sources-cribl-internal) Source's CriblMetrics option is now available on Cribl-managed Workers in Cribl.Cloud, as well as customer-managed (on-prem and hybrid) Workers.
