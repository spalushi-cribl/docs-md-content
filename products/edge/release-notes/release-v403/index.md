# Cribl Edge 4.0.3


## New Features 

This release provides the following improvements:

CRIBL-13900 In the Kubernetes Logs Source, the new **General Settings** > **Enable timestamps** toggle provides an option for working with containers whose console output lacks proper timestamps (either because the timestamps are missing altogether, or because they specify time but not date).

When the option is enabled, Edge prefixes a timestamp to each line of the container's raw console output. Cribl recommends that you use the example **Kubernetes Logs** Event Breaker Ruleset, which sets the `_time` property based on the prefixed timestamps. You might need to further modify the Event Breaker to deal with more complicated logs.

CRIBL-14049 Windows Event Forwarder Source's XML queries now support `<Suppress>` elements.

CRIBL-5924 Splunk TCP Source's new **Use Universal Forwarder time zone** option enables Event Breakers to derive events' time zone from UF-provided metadata.

## Corrections 

This release includes the following fixes:

CRIBL-14179 On Windows, Edge Nodes now retain their GUIDs when the Node is restarted.

CRIBL-14157 The K8s Metrics Source no longer fails to collect Pod/Container metrics when it encounters Pods that lack a `containerStatuses` property.

CRIBL-13900 The Kubernetes Logs Source no longer collects redundant information for containers that don't emit their own timestamps. 

CRIBL-14047 When there are many Stream Workers and few Edge Nodes, or many Edge Nodes and few Stream Workers, the UI now displays them correctly, and no longer ignores Stream Workers or Edge Nodes that fall outside of the first 100 items in the list.

CRIBL-14169 The landing page now displays system metrics from Edge Nodes when teleporting to an Edge Node on a version older than 4.0.1 from the Leader.
