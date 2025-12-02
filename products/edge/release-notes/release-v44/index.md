# Cribl Edge 4.4


November is here, and so are new Cribl Edge features! Gobble up these release 
notes – they're way more palatable than leftovers. 

## New Features 

This release provides the following improvements:

### Process Metrics

You can now collect process-specific information in the System and Windows
Metrics Sources. To configure which processes you want to collect metrics from,
use the new Process Metrics, available both in Stream and Edge. 

- [Process Metrics configuration for System Metrics](sources-system-metrics#process-metrics).
- [Process Metrics configuration for Windows Metrics](sources-windows-metrics#process-metrics). 

### Kubernetes Disk Spooling

You can now spool logs and metadata in the Kubernetes Logs Source. Cribl Search users
can access these logs and their metadata in order to find data that resides in
the spool (without having to forward certain logs to a Destination for storage). 

### Collect Mixed File Contents as Text

In the File Monitor Source configuration, you can toggle **Force text format**
on to display binary file contents as text in the event. See the [File
Monitor](sources-file-monitor#optionalsettings) docs for more details. 

### Forward Custom Data Points to Destinations using Tags

The `cribl` [metadata on the Host Info tab](explore-edge-win#host-info) now
includes tags defined on the Edge instance, so you can add custom data points to
each event and forward them to your Destinations. 

The Windows Event Logs Source can now collect events from the Forwarded Events log, when the
[Event format](sources-windows-event-logs#optional-settings) setting is XML. 

## Persistent Queues Configuration Changes

> CRIBL-18456 When you upgrade Fleets to Cribl Stream 4.4 and newer, any undefined PQ **Max queue size** values in Source/Destination configs on versions older than 4.4 will be set to the new 5 GB default value. This is a guardrail to control queues' consumption of host machines' disk space. This change does not affect any existing config with a defined **Max queue size**.
> 
> Adjust this default to match your needs. If some of your integrations'
> persistent queues had an undefined **Max queue size**, queues larger than 5 GB will trigger PQ fallback behavior until you resize this limit.
> 
> Also in Cribl Stream 4.4 and newer, this field's highest accepted value is
> defined in the new **Group Settings** > **General Settings** > **Limits** >
> **Storage** > **Max PQ size per Worker Process** field. The corresponding
> `limits.yml` [key](limitsyml) is `maxPQSize`. Consult Cribl Support before
> increasing that limit's 1 åTB default. 
>
{.box .warning}

## Corrections

This release includes the following fixes:

Using the Last Login Collector on Linux via the System State Source was causing
CPU spikes in certain cases. CRIBL-20326

The File Monitor Source was incongruously collecting duplicate events. We gave it a firm
talking to, and it is behaving again. CRIBL-20861 

Diagnostic bundles created for Edge on Mac systems now contain the correct
permissions when extracted from the tar archive, so you can access all diagnostic files. CRIBL-20037

