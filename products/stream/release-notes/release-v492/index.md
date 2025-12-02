# Cribl Stream 4.9.2


Cribl Stream 4.9.2 corrects a critical issue found in Cribl Stream 4.9.1:

### Splunk HEC Source Malformed Event Errors Fix

This release fixes a breaking change to HEC parsing in 4.9.1 that caused an
increase in `Dropping malformed HEC event, enable debug to see details` error
logs, even when the incoming HEC payloads were valid. This release rolls back
the changes that caused this error to occur.
