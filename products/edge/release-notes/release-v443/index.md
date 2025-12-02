# Cribl Edge 4.4.3


Cribl Edge 4.4.3 corrects a critical issue found in Cribl Edge 4.4.2. 

This release includes the following change:

### CPU and Memory Utilization Fix

A change made to Packs in 4.4.2 caused high CPU and memory utilization
when using Packs in pre-processing Pipelines, post-processing Pipelines, and
QuickConnect. To mitigate this issue, the change made for CRIBl-18928 was reverted in this release.