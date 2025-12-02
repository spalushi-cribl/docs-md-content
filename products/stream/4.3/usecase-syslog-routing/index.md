# Syslog Sources/Destinations Routing


Assume that you need to ingest a specific type of data (for example, Cisco ASA) via syslog and output it to a matching file (for example, `/syslog/asa/10.1.2.3/syslog.log`). If this data comes directly to Cribl, you won't have a source file name to match against. But you'll know the incoming port number and the sender's IP address. 

Here are three approaches you can take, followed by some load-balancing best practices.

## Lookup Against Syslog/UDP Port 514

You could ingest syslog data via UDP port 514, and build a lookup file to map sender IPs to sourcetype, index, etc.

#### Pros 

This "just works," requiring no modification to the sender's destination port.

#### Cons

Requires use of a [pre-processing Pipeline](pipelines#input-conditioning-pipelines), and requires updating the [lookup file](lookups-library). Both are minor drawbacks.

## Port and Source per Sender

Use a different port, with a separate Source, for each type of sender.

#### Pros

- This allows you to directly map the index, Source, and sourcetype for each sender right from the Source definition. 

- If you have specific event breaking (of multi-line events), you can attach the line breakers to that specific Source.

- You can assign time zones differently per Source.
    
#### Con

Not all syslog senders support use of ports other than UDP 514.

## Why Not Both?

A combination of both the above approaches, to meet specific needs: Combine separate ports/Sources with a lookup table.

#### Pro

If you're already using multiple ports, this allows you to continue doing so – without the need to change your syslog senders.

#### Con: 

You must attach a pre-processing Pipeline to *all* of the syslog Sources. (All Sources should share the same pre-processing Pipeline.) The con part is that you must remember to set this up for each Source, as well as maintain the lookup table.

## Best Practices

Here are some best practices, focusing on efficient load balancing. Our overall advice – motivating several specific tips below – is to avoid continuously tying high-volume senders to any one Cribl Stream Worker Process.

- For syslog, use TCP (not UDP) whenever possible, *except* for high-volume senders.

- Send TCP connections through a load balancer, with data balanced among your available Worker Nodes.

- For high-volume senders (like Palo Alto Networks), assign a dedicated port.

- For extremely high-volume senders, use UDP, and send to a dedicated Cribl Stream Worker that's sized for that workload. (For more on optimizing workloads, see [Sizing and Scaling](scaling).)

- Event breaking of multi-line events must be validated. Anything using syslog's normal approach (indentation of lines 2–N) is supported out-of-the-box. But a sender that ignores the syslog RFC for multi-line might require a custom event breaker.
