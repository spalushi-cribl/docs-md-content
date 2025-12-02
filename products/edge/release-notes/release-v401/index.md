# Cribl Edge 4.0.1


## New Features 

This release provides the following improvements:

CRIBL-13472 In the Knowledge library, the default AppScope Config file now ships with all **Metrics** types available on the **Watch list**. The  Metrics include `StatsD`,`Filesystem`,`Network`, `HTTP`, `DNS`, and `Process`. 

CRIBL-13343 You can now add a Kubernetes node via the Cribl Edge UI. 

CRIBL-13773  A new environment variable, `CRIBL_K8S_TLS_REJECT_UNAUTHORIZED`, is available for Kubernetes deployments. `CRIBL_K8S_TLS_REJECT_UNAUTHORIZED` disables certification validation when connecting to the Kubernetes APIs. For details, see [Which Certificate Does kubelet Use?](https://serverfault.com/questions/1063490/which-certificate-does-kubelet-use/1063821#1063821).

## Corrections 

This release includes the following fixes:

CRIBL-14093 Corrected Amazon S3 Source's stopped data input.

CRIBL‑13505 Sorting the **Manage** > **Edge Nodes** page by columns no longer generates a blank list.

CRIBL-13595 Cribl Edge's honeycomb display now renders null values.

CRIBL-12397 On an Edge Node's landing page, the Node's Network and Disk cards now show average and total values over the given time period, with the graphs displaying `KBps` units instead of `KBs`.

CRIBL-13779 On the **Mappings** page, the Fleet/Subfleet drop-down now displays a nested hierarchy. 

CRIBL-12465 On the **Manage Fleets** page's **Fleets** tab, searching for a Subfleet name now returns the Subfleet along with information about its parent Fleets.

CRIBL-12602 The **Manage Fleets** page's **Pipelines** column no longer overstates the real number of Pipelines – it now excludes Packs from each Fleet’s Pipelines count. 

CRIBL-13696 Added a UI warning that Cribl Edge Windows Nodes currently can't be upgraded via the Leader's UI.
