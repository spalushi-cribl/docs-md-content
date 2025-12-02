# Cribl Edge 4.2.2


## New Features 

This release provides the following improvements:

CRIBL-17135 Zip, zip, hooray! The [File Monitor Source](sources-file-monitor) in
Cribl Edge can now process `.zip` files.

CRIBL-19248 Need to grab a memory snapshot? Now it’s easier than ever with the
`diag heapsnapshot` command, available in Edge and Stream on-prem deployments.
For detailed instructions, check out [Including Memory Snapshots](/stream/diagnosing#memory-snapshots).
Happy snapping!

CRIBL-10895 The Windows Event Logs Source now supports rendering events in XML
format as well as JSON.

## Corrections 

This release includes the following fixes:

CRIBL-19154 Corrected File Monitor Source's memory leak.

CRIBL-19157 File Monitor events now display `host` property values.

CRIBL-18676 Update Windows Node: Corrected generated script.

CRIBL-18963, CRIBL-19189 Unable to view Edge Node settings when teleported.

CRIBL-19279 Upgrade & Share settings should not be visible when teleported.

CRIBL-18676 We fixed the UI dialog and script for updating an Edge Windows Node.
The update command no longer contains the trailing `/update`.

CRIBL-14488 Exec Source no longer fails or truncates data above `stdout` or
`stderr` thresholds.

CRIBL-19189 Teleporting into an Edge Node now displays the Node Settings.

CRIBL-19044 Since the sample_config has been restored, the AppScope Source no
longer disappears from the UI or causes validation errors for changes to other
sources.

CRIBL-18923 Members with the Fleet-level Editor Permissions can now use manual
file discovery.

CRIBL-18784 Edge members assigned to the User Permission can now access the
Subfleets below an authorized top level Fleet.
