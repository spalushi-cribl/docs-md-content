# Cribl Edge 4.8.1


This release corrects a critical issue in Cribl Edge and Cribl Stream 4.8 where Splunk Single Instance and Splunk Load Balanced Destinations might incorrectly detect that a downstream instance of Splunk is shutting down at frequent intervals. This only affects Destinations with the **Max S2S version** set to `v3` and the **Minimize in-flight data loss** toggle enabled. [CRIBL-26969] 

See the [Cribl Edge 4.8](release-notes/release-v48) release notes for the latest feature and correction updates.