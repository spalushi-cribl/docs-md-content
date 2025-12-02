# iometrics.yml


`iometrics.yml` is a configuration file that defines the metrics levels for individual Sources and Destinations in Cribl Stream. It allows you to control the granularity of metrics emitted by each integration. These correspond to the [**Metrics Levels** settings](internal-metrics#integration-metrics-levels) of a Worker Group.

```yaml {title="iometrics.yml"}
# Default metrics level - Default level for I/O metrics collection if not otherwise specified
# One of: minimal | basic | detailed | comprehensive
# [string]
DEFAULT:
# Input/Output specific metrics levels - Maps specific input/output IDs to their metrics collection levels
# One of: minimal | basic | detailed | comprehensive
<metric>:
```

Example `iometrics.yml` configuration:

```yaml {title="Example iometrics.yml configuration"}
DEFAULT:
  level: detailed
input:in_splunk_tcp:
  level: minimal
input:in_syslog:
  level: detailed
output:syslog_loopback_dest_udp:
  level: detailed
input:eh_in:
  level: basic
input:in_cribl_http:
  level: minimal
```
