# All-in-One Reference Architecture


This reference architecture shows one possibility for ingesting and replaying diverse data types from multiple senders. The Sources shown at left are just representative examples. The replay workers at right could be hosted on Linux servers, in Kubernetes Pods, etc.

![All-in-one reference architecture](ref-arch-all-in-1.a42bd4fc93.png)
{border="true"}

## Sizing Considerations

- An all-in one Worker Group works best with fewer than 2000 Sources (agents, syslog senders, etc.).
- Syslog over TCP should be under 200 GB/day from any one sender.
- Account for up to 50% bursts in traffic.

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.
