# Syslog Reference Architecture


This reference architecture shows one Cribl Stream deployment possibility for collecting syslog data from multiple geographic regions. This particular example processes all syslog data through a single Worker Group.

![Syslog reference architecture](ref-arch-syslog.7e1d0c7cfa.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-syslog.svg) or [draw.io](/downloads/ref-arch-syslog.drawio) format. Download Cribl stencils [here](https://assets.ctfassets.net/xnqwd8kotbaj/2OnWVALZxgnpdWhiTV40Ey/c68757ee18aa7ef411e9ef12a5fd0db7/Cribl-Stencil-Pack-ALL.zip).
> 
> Note that the load balancer shown above between the Leader and Worker Group should be dedicated only to communication between the Leader and Workers. At the lower left, the diagram shows separate load balancers dedicated to communication between the syslog senders and the Stream Worker Nodes.
>
{.box .success}

## General Sizing Considerations

- Syslog over TCP should be under 200 GB/day from any one sender.
- Account for up to 50% bursts in traffic.
- General sizing guideline is maximum 200 GB/day throughput per vCPU, or 400 GB/day/CPU core.
- Size for n+50% (to account for bursts and additional high availability).
- Sources that use long-lived TCP connections should be converted to UDP, or to protocols that batch data in different TCP sessions (e.g., most [HTTP Push Sources](/stream/sources#push)). Sources that send over long-lived TCP connections are subject to "TCP pinning." They can send up to 200 GB/day (if the Stream Worker is built on CPU cores).

## Single or Multiple Worker Groups?

The single-Group architecture shown here is one approach. It might or might not be desirable for your use case. 

Pros of using a single Worker Group:
- Commit and deploy only once per change.
- Manage all syslog configuration in one Group.

Cons of using a single Worker Group:
- Additional filtering required if your data needs to be sent to diverse Destinations.

## Cribl Source Config Considerations

- Syslog over UDP close to senders is ideal.
- If using Syslog over TCP, a single source can send up to 200 GB/day (if workers are built on CPU cores), or up to 100 GB/day (if workers are built on hyperthreaded vCPUs).
* The Worker Group can also process other data (such as from Filebeat source, Splunk forwarders, HTTP listeners, etc.). However, if the agent count reaches the thousands, a dedicated Worker Group per data type is better.
* Preferred practice: Different upstream technologies should send syslog on different syslog ports (e.g., PAN on 5514, Cisco ASA on 6514, Zscaler on 7514).

## Cribl Route Config

Map this Route Filter example by technology: PAN, ASA, Windows, etc. This example assumes a Palo Alto Networks host that includes `PAN` or `FW` in its name:

- `__inputId.startsWith('syslog') && (host.indexOf('FW')> -1 || host.indexOf('PAN')> -1)` ‑> Palo Alto Pack as Pipeline.
- `__inputId=='PAN_Syslog'` ‑> Palo Alto Pack as Pipeline.
- `__inputId=='Imperva_Syslog'`‑> Imperva Pack as Pipeline.

## Cribl Output Router Config

This example shows sending to multiple instances of the same Destination type – data in DC1 should send to Splunk indexers in DC1, data in DC2 should send to Splunk indexers in DC2), etc. Stream's [Output Router](destinations-output-router) Destination works well for this use case.

### Filter and Destination Examples

- Region==`East`, route to Splunk Virginia indexers.
- Region==`Central`, route Splunk Austin indexers.
- Owner==`Team420`, route to Elastic indexers.

## Cribl Load Balancers

The load balancer in front of the Leader UI can be an Application Load Balancer or a Network Load Balancer.

The load balancer between the Cribl Workers and the Cribl Leader listening on port 4200 must be a Network Load Balancer [or equivalent](deploy-add-second-leader#loadbalancers). 

## Third-Party Load Balancer Configs

This section gathers LB configuration examples and resources for several syslog senders.

### F5 BIG-IP Example

```
ltm virtual udpsyslog_514_vs { 
destination 10.10.10.10:514 
ip-protocol udp 
mask 255.255.255.255 
pool udpsyslog_514_pool 
profiles { 
    udp { } 
} 
Vlans-disabled 
}

ltm pool udpsyslog_514_pool { 
members { 
    10.10.20.10:514 { 
        address 10.10.20.10 
        session monitor-enabled 
        state up 
    } 
    10.10.20.20:514 { 
        address 10.10.20.20 
        session monitor-enabled 
        state up 
    } 
} 
monitor tcp 
service-down-action reset 
}
```

### Citrix Netscaler Example

Reference: [Load Balancing syslog Servers](https://docs.citrix.com/en-us/citrix-adc/current-release/system/audit-logging/load-balancing-syslog-server.html).

```
add service service1 192.0.2.10 SYSLOGUDP 514
add service service2 192.0.2.11 SYSLOGUDP 514
add service service3 192.0.2.11 SYSLOGUDP 514
add service service4 192.0.2.10 SYSLOGTCP 514
add service service5 192.0.2.11 SYSLOGTCP 514
add service service6 192.0.2.11 SYSLOGTCP 514
add lb vserver lbvserver1 SYSLOGUDP -lbMethod AUDITLOGHASH
add lb vserver lbvserver1 SYSLOGTCP -lbMethod AUDITLOGHASH
bind lb vserver lbvserver1 service1
bind lb vserver lbvserver1 service2
bind lb vserver lbvserver1 service3
bind lb vserver lbvserver1 service4
bind lb vserver lbvserver1 service5
bind lb vserver lbvserver1 service6
add syslogaction sysaction1 -lbVserverName lbvserver1 -logLevel All
add syslogpolicy syspol1 ns_true sysaction1
bind system global syspol1
```

### AWS NLB Guide

Reference: [UDP Load Balancing for Network Load Balancer](https://aws.amazon.com/blogs/aws/new-udp-load-balancing-for-network-load-balancer/).


### NGINX Plus Guide

Reference: [TCP and UDP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/).


### HAProxy (Including TCP Break-Apart)

Reference: [haproxy speaks syslog](https://jmagnin.github.io/2020/10/10/syslog-load-balancing-with-haproxy.html).

This example resides at `/usr/local/etc/haproxy/haproxy.cfg`:

```
ring worker1
format rfc5424
maxlen 1500
size 32764
timeout connect 5s
timeout server 10s
server worker1 worker1.example.com:1514
ring worker2
format rfc5424
maxlen 1500
size 32764
timeout connect 5s
timeout server 10s
server worker2 worker2.example.com:1514

# Add as many ring buffers as needed here

log-forward syslog-lb
# UDP Listener (IPv4/IPv6)
dgram-bind :::1514

# TCP Listener (IPv4/IPv6)
bind :::1514

# Load-balance outgoing messages on 2 TCP outputs
log ring@worker1 sample 1:2 local0
log ring@worker2 sample 2:2 local0

maxconn 65536
timeout client 10s
```

## Leader High Availability

Configuring Leader [high availability](deploy-add-second-leader) requires a standby server, a load balancer ([details here](deploy-add-second-leader#loadbalancers)) between users and the Cribl UI port, and a Network Load Balancer between the Cribl Leader and Workers.

Leader high availability is optional. Many customers opt instead for a single Leader, and a remote Git repo for scheduled backups. If the Leader becomes unavailable, you can use the Git repo to provision a new Leader, restoring configuration from a backup. 

However, a single Leader is reliable only when Workers are running Cribl Sources that fully leverage a shared-nothing architecture (such as Raw HTTP, Splunk HEC, Elasticsearch API, Amazon S3 Source, etc). Here, Workers can process data even if the Leader is unavailable. A Leader without failover will not reliably support [Collectors](collectors), which require a Leader's ongoing orchestration.

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.

