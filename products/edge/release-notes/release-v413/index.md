# Cribl Edge 4.1.3


## New Features 

This release provides the following improvements:

A new native [Amazon Security Lake](destinations-security-lake) Destination is now generally available. 



## Corrections 

This release includes the following fixes:

CRIBL-17602 The Kubernetes Logs Source now continues reading log files after the container runtime rotates them.

CRIBL-17907 The Kubernetes Logs Source supports attaching a new `kubernetes_logs` pre-processing Pipeline. When you enable the K8s Logs Event Breaker's **Enable timestamps** option, this Pipeline removes conflicting timestamps prepended by the Source.
