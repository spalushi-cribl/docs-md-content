# Cribl Edge 4.1.1


Looking for the 411 on Edge? You goat it!

> ##### Known Issue
>
> Due to a serious known issue, deploying config bundles to bootstrapped 4.1.1 customer-managed (on-prem or hybrid) Workers can (under certain conditions) cause them to stop processing data. Before you bootstrap Workers with v.4.1.1, please see [details and workarounds here](known-issues#CRIBL-17264). Cribl is accelerating a fix.
>
{.box .warning}

## New Features 

This release provides the following improvements:

CRIBL-16549 The Windows Event Logs Source now provides a configurable **Max event bytes** limit. 

## Corrections 

This release includes the following fixes:

### Sources, Collectors, and Destinations Fixes

CRIBL-16097 The Kubernetes Events Source's **Configure** tab no longer exposes the election polling interval. 

CRIBL-16355 The Journal Files Source now processes journal files with state `OFFLINE`. 

### Functional Fixes 

CRIBL-15367 Edge Nodes no longer violate a disabled **Enable automatic upgrades** setting. 

CRIBL-16117 The **Add Windows Node** modal now supports prompts for the installation directory and runtime user.

CRIBL-16126 The **Add Windows Node** modal now uses the correct file name. 

CRIBL-16094 The **Add Kubernetes Node** modal now supports version ranges, which can accommodate point releases.

CRIBL-16468 Edge Fleets now support Global Variables.

### UX/UI Fixes

CRIBL-16194 In a Fleet's **List View**, the **System Activity** drawer now correctly displays data for Windows Edge Nodes.
