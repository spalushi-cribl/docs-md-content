# Cribl Edge 4.7.1


Cribl Edge 4.7.1 corrects three critical issues we found in Cribl Edge 4.7.0. 

This release includes the following changes:

## Google Cloud Security Operations Destination Fix
Upgrading to Cribl Edge 4.7 with an existing Google Chronicle Destination (now
Google Cloud Security Operations) configured on a Fleet caused all
Destinations in that Fleet to fail to load and process data. CRIBL-25404

## Throttling Fix for TCP Destinations
Setting a throttle on various TCP Destinations did not work as expected. The
actual data throughput was higher than the configured throttle rate, which
contributed to a metrics reporting issue in Cribl Edge. CRIBL-25119

## Syslog UDP Parsing Fix
Syslog messages sent via UDP were incorrectly parsed. This caused parsing errors
and caused a single UDP message to generate multiple messages. CRIBL-25400

## Splunk Load Balanced Index Discovery Fix for Managed Nodes
In Cribl Edge, Managed Nodes could not access Splunk Load Balanced index
discovery. CRIBL-25407