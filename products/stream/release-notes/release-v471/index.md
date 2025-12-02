# Cribl Stream 4.7.1


Cribl Stream 4.7.1 corrects critical issues found in Cribl Stream 4.7.0 

This release includes the following changes:

### Google Cloud Security Operations Destination Fix

Upgrading to Cribl Stream 4.7 with an existing Google Chronicle Destination (now Google Cloud Security Operations) configured on a Worker Group caused all Destinations in that Worker Group to fail to load and process data. CRIBL-25371

### Throttling Fix for TCP Destinations

Setting a throttle on various TCP Destinations did not work as expected. The actual data throughput was higher than the configured throttle rate. CRIBL-25119

### Syslog UDP Parsing Fix

Syslog messages sent via UDP were incorrectly parsed. This caused parsing errors and resulted in multiple messages being generated from a single UDP message. CRIBL-25400

### Splunk Load Balanced Index Discovery Fix for Managed Nodes

In Cribl Stream, Managed Nodes could not access Splunk Load Balanced index
discovery. CRIBL-25407
