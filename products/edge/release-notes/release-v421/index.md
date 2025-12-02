# Cribl Edge 4.2.1


Cribl Edge 4.2.1 corrects a critical issue in Cribl Edge 4.2.0.

## Known Issue: File Monitor Memory Leak

> The File Monitor Source, when run in Manual Discovery mode, causes a memory leak that eventually causes Edge Nodes to crash. For details, fix timeline, and workaround, please see [Known Issues](known-issues#CRIBL-19154).
>
{.box .warning}

## Corrections

This release includes the following fix:

### Credential Encryption Fix

SAAS-4823 Corrected a critical regression that caused Fleets to become unresponsive, preventing access to Sources and Destinations. This issue was caused by incorrectly encrypted credentials.
