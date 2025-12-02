# persistent-queue.yml


`persistent-queue.yml` contains settings for configuring [persistent queues](persistent-queues).

```yaml {title="persistent-queue.yml"}
# Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
# [number; default: 1000]
maxBufferSize:
# File size limit - The maximum size to store in each queue file before closing and optionally compressing.
# Enter a numeral with units of KB, MB, etc.
# [string; default: 10MB]
maxFileSize:
# Queue size limit - The maximum disk space that the queue can consume before queueing stops
# [string; default: 1GB]
maxSize:
# Mode - Queue operation mode
# One of: always | never | smart
# [string; default: always]
mode:
# Queue file path - The location for the persistent queue files
# [string]
path:
# Compression - Codec to use to compress the persisted data
# One of: gzip | none
# [string]
compress:
# Commit frequency - The number of events to send downstream before committing that Stream has read them
# [number]
commitFrequency:
```
