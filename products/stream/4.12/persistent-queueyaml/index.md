# persistent-queue.yml


`persistent-queue.yml` contains settings for configuring [persistent queues](persistent-queues).

```yaml {title="$CRIBL_HOME/default/cribl/persistent-queue.yml"}
maxBufferSize: # [number] Maximum number of events to hold in memory before writing the events to disk.
maxFileSize: # [string] Maximum size to store in each queue file before closing and optionally compressing.
maxSize: # [string] Maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops.
mode: # [string] With smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With always mode, PQ will always write events directly to the queue before forwarding them to the processing engine. One of: always | smart
```
