# Cribl Stream 4.12.1


Cribl Stream 4.12.1 corrects a critical issue found in Cribl Stream 4.12.0:

### Fix for Collection job artifacts' accumulation on Worker hosts

This release fixes a critical issue in Cribl Stream 4.12.0 that caused obsolete internal files from completed Collection jobs to accumulate on the machines hosting Stream Workers. The fix, and the underlying issue, affect only customer-managed (on-prem) and hybrid Workers; Cribl is directly mitigating the issue for Cribl-managed Cloud Workers. (SAAS-17499)
