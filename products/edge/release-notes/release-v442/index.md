# Cribl Edge 4.4.2


## New Features

This release contains a new feature:

### AWS EC2 Metadata on Edge Nodes

AWS metadata for EC2 is now available as part of Edge Node information. You can use
this metadata to create specific Fleet mappings in **Manage > Mappings**.
For more information, see [Manage Edge Nodes](https://docs.cribl.io/edge/managing-edge-nodes#collecting-aws-ec2-tag-metadata). CRIBL-14303 

## Corrections

This release includes the following fixes:

We fixed a few pesky problems in the Kubernetes Logs Source, and made an improvement. 
See the fixed issues below. CRIBL-20877

- Setting the timezone on a cluster Node to anything but UTC made the Source
  incorrectly process raw logs data from the Kubelet.
- Kubernetes log entries were getting missed due to incorrect handling of log
  rotation on Nodes with high-EPS from containers
- Improved throughput performance of the Kubernetes Logs Source.

CPU profiles collected on Windows instances of Edge were not being generated due
to an invalid character in the filename. CRIBL-20516

Edge-only Sources were unavailable in standalone (not distributed) instances of
Edge. CRIBL-21443

We updated Go to [version 1.21](https://go.dev/doc/go1.21). CRIBL-21155

`CriblPack` now manages Routes independently so that Routes do not improperly drop events going to Packs. CRIBL-18928

> The fix for CRIBL-18928 introduced an issue that caused high CPU and memory utilization when using Packs in pre-processing Pipelines, post-processing Pipelines, and QuickConnect. To avoid this issue, Cribl recommends that you upgrade to version 4.4.3 or older. Cribl Stream v.4.4.3 reverts this fix. 
>
{.box .warning}