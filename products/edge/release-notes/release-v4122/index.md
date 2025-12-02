# Cribl Edge 4.12.2


Cribl Edge 4.12.2 includes experience improvements and bug fixes.

## Experience Improvements

- We updated the upgrade status message for Edge Nodes running in containers. It
  is now more descriptive and explains that you must upgrade the Edge Node
  manually.   

- We implemented several user interface improvements:

    - The **UI Access** toggle labels are now labeled **Teleport** to more
      accurately describe what the user is enabling. 
    - In the Node Info drawer, we changed the **Latest Heartbeat** label to
      **Details** to describe the key-value pair table.

## Corrections

This release contains the following bug fixes:

### Cribl Edge Exclusive Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-32360</div>|In certain circumstances, the System State Source on Windows Edge Nodes was returning Local Port values as `[object Object]`. This happened when users specified a list of ports in the Windows firewall, which Cribl interpreted as an object instead of a usable string.|
|CRIBL-32681|With the Kubernetes Logs Source configured in load-balanced mode, Kubernetes Logs events were not breaking properly at a newline.|
|CRIBL-32638|We fixed an issue where the leading whitespace was being stripped from indented lines when Cribl removed timestamps from Kubernetes Logs.|
|CRIBL-31919|We resolved error message log spamming on Linux Edge Nodes when loading data for Kubernetes Pods.|
|CRIBL-27188|We fixed a bug where trying to use a certificate in a Subfleet (in an output, input, or **Fleet Settings**/**General Settings**/**TLS**) that you defined in a parent Fleet would return the absolute path to the committed cache directory instead of the $CRIBL_HOME directory.|

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31772</div>|We fixed an issue that caused an error when adding certificates via **Settings > Global > Distributed Settings > TLS Settings**|
|CRIBL-32619|The Live Data and Pipeline previews now correctly expand to display arrays and objects, even if the field name contains a dot.|
|CRIBL-32824| Fixed an issue where generating a diagnostic bundle after teleporting into a Worker/Edge Node, the system created and uploaded two files instead of one. |
|AI-1859| Fixed an issue where large event samples in Copilot Editor could exceed API limits and cause errors. The system now truncates and filters tool messages before sending, improving reliability.|

### Security Fixes
|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-31567</div>|DOMPurify is now upgraded to the latest version.|
|CRIBL-32166|Users who used a Read Only role in Cribl Stream or Cribl Edge to fetch the Leader token will now have to use an Admin role for that purpose. <ul><li>If you were using `stream_reader`, you must now use `stream_admin`.</li><li>If you were using `edge_reader`, you must now use `edge_admin`.</li></ul>|

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-33036</div>| Azure Blob Storage integrations now provide an **Azure Cloud** setting to support cloud services in China. |