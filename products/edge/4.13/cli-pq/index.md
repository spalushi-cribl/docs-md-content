# pq


Benchmark persistent queue (PQ) performance. Use `pq write` to simulate high-throughput event ingestion and `pq read` to simulate PQ consumption. This command is useful for internal benchmarking and evaluating disk performance in customer environments.

**Sub-commands:**

- [`pq read`](#pq-read)
- [`pq write`](#pq-write)

## Sub-commands and Options

### `pq-read` {#pq-read}

Read data from a persistent queue and discard it. Use this command to test PQ deserialization and consumer performance.

**Usage:**

```text
./cribl pq read -o /tmp/pq
```

**Sample response:**

This response prints every second during operation:

```text
{"time":"2025-07-15T22:38:37.978Z","cid":"api","channel":"IPersistentQueue","level":"info","message":"initializing persistent queue","conf":{"maxFileSizeBytes":1048576,"compress":"none","minFreeSpaceBytes":5368709120,"maxQueueSizeBytes":5368709120,"directory":"/tmp/pq"}}
{"time":"2025-07-15T22:38:37.982Z","cid":"api","channel":"IPersistentQueue","level":"info","message":"init complete","offset":1955629794,"size":1955629794}
{"time":"2025-07-15T22:38:38.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":166.14,"eluPerc":91.82,"mem":{"heap":148,"heapTotal":169,"ext":6,"rss":409,"buffers":3}}
{"time":"2025-07-15T22:38:38.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619118982,"mbPerSec":"111.35","eventsPerSec":978626}
{"time":"2025-07-15T22:38:39.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":95.3,"eluPerc":91.97,"mem":{"heap":144,"heapTotal":171,"ext":4,"rss":411,"buffers":1}}
{"time":"2025-07-15T22:38:39.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619119982,"mbPerSec":"116.63","eventsPerSec":1025010}
{"time":"2025-07-15T22:38:40.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":95.47,"eluPerc":92.04,"mem":{"heap":140,"heapTotal":173,"ext":2,"rss":413,"buffers":0}}
{"time":"2025-07-15T22:38:40.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619120982,"mbPerSec":"116.99","eventsPerSec":1028242}
{"time":"2025-07-15T22:38:41.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":95.41,"eluPerc":92.02,"mem":{"heap":142,"heapTotal":175,"ext":3,"rss":416,"buffers":0}}
{"time":"2025-07-15T22:38:41.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619121982,"mbPerSec":"116.60","eventsPerSec":1024748}
{"time":"2025-07-15T22:38:42.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":96.98,"eluPerc":92.09,"mem":{"heap":147,"heapTotal":177,"ext":3,"rss":420,"buffers":1}}
{"time":"2025-07-15T22:38:42.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619122982,"mbPerSec":"117.42","eventsPerSec":1032000}
{"time":"2025-07-15T22:38:43.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":96.09,"eluPerc":92.08,"mem":{"heap":148,"heapTotal":178,"ext":3,"rss":422,"buffers":1}}
{"time":"2025-07-15T22:38:43.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619123982,"mbPerSec":"117.53","eventsPerSec":1032939}
{"time":"2025-07-15T22:38:44.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":95.14,"eluPerc":92.07,"mem":{"heap":146,"heapTotal":180,"ext":6,"rss":424,"buffers":0}}
{"time":"2025-07-15T22:38:44.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619124982,"mbPerSec":"116.06","eventsPerSec":1020000}
{"time":"2025-07-15T22:38:45.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":95.31,"eluPerc":92.09,"mem":{"heap":153,"heapTotal":182,"ext":4,"rss":426,"buffers":1}}
{"time":"2025-07-15T22:38:45.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619125982,"mbPerSec":"115.88","eventsPerSec":1018435}
{"time":"2025-07-15T22:38:46.978Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":97.81,"eluPerc":92.09,"mem":{"heap":156,"heapTotal":184,"ext":4,"rss":396,"buffers":1}}
{"time":"2025-07-15T22:38:46.982Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619126982,"mbPerSec":"115.48","eventsPerSec":1014939}
{"time":"2025-07-15T22:38:47.146Z","cid":"api","channel":"IPersistentQueue","level":"info","message":"engage backpressure mode"}
{"time":"2025-07-15T22:38:47.148Z","cid":"api","channel":"commands:pq","level":"info","message":"pq read completed","totalEvents":9343308,"totalBytes":1114732074,"elapsedMs":9166}
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-o <path>` | **(Required)** Directory path to read persistent queue files from. |

> This command does not commit or remove files from the persistent queue. It's intended for read-only performance testing.
>
{.box .info}

### `pq-write` {#pq-write}

Read data from `stdin` and write to a persistent queue. Use this command to test PQ serialization and disk write performance.

**Usage:**

```text
cat ~/work/syslog_large.log | ./cribl pq write -o /tmp/pq -b 1000 -s 10485760 -c gzip
```

**Sample response:**

This response prints every second during operation:

```text
{"time":"2025-07-15T22:36:45.543Z","cid":"api","channel":"IPersistentQueue","level":"info","message":"initializing persistent queue","conf":{"maxFileSizeBytes":1048576,"compress":"none","minFreeSpaceBytes":5368709120,"maxQueueSizeBytes":5368709120,"directory":"/tmp/pq"}}
{"time":"2025-07-15T22:36:45.545Z","cid":"api","channel":"IPersistentQueue","level":"info","message":"init complete","offset":0,"size":0}
{"time":"2025-07-15T22:36:46.543Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":172.39,"eluPerc":90.9,"mem":{"heap":152,"heapTotal":188,"ext":6,"rss":402,"buffers":2}}
{"time":"2025-07-15T22:36:46.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619006545,"mbPerSec":"144.89","eventsPerSec":1273452,"pqSize":266239897}
{"time":"2025-07-15T22:36:47.543Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":102.6,"eluPerc":91.49,"mem":{"heap":170,"heapTotal":206,"ext":2,"rss":421,"buffers":0}}
{"time":"2025-07-15T22:36:47.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619007545,"mbPerSec":"149.13","eventsPerSec":1310633,"pqSize":540642861}
{"time":"2025-07-15T22:36:48.543Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":102.46,"eluPerc":91.05,"mem":{"heap":199,"heapTotal":227,"ext":5,"rss":442,"buffers":3}}
{"time":"2025-07-15T22:36:48.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619008545,"mbPerSec":"150.69","eventsPerSec":1324387,"pqSize":817766703}
{"time":"2025-07-15T22:36:49.542Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":102.85,"eluPerc":92.16,"mem":{"heap":216,"heapTotal":246,"ext":5,"rss":462,"buffers":2}}
{"time":"2025-07-15T22:36:49.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619009545,"mbPerSec":"153.84","eventsPerSec":1352032,"pqSize":1100751216}
{"time":"2025-07-15T22:36:50.543Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":102.46,"eluPerc":91.74,"mem":{"heap":232,"heapTotal":265,"ext":5,"rss":397,"buffers":2}}
{"time":"2025-07-15T22:36:50.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619010545,"mbPerSec":"155.26","eventsPerSec":1364560,"pqSize":1386666017}
{"time":"2025-07-15T22:36:51.543Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":102.48,"eluPerc":92.5,"mem":{"heap":253,"heapTotal":285,"ext":5,"rss":362,"buffers":3}}
{"time":"2025-07-15T22:36:51.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619011545,"mbPerSec":"153.40","eventsPerSec":1348219,"pqSize":1668603928}
{"time":"2025-07-15T22:36:52.542Z","cid":"api","channel":"ProcessMetrics","level":"info","message":"stats","cpuPerc":103.02,"eluPerc":92.28,"mem":{"heap":269,"heapTotal":305,"ext":5,"rss":381,"buffers":3}}
{"time":"2025-07-15T22:36:52.545Z","cid":"api","channel":"commands:pq","level":"info","message":"stats","timestamp":1752619012545,"mbPerSec":"153.34","eventsPerSec":1347675,"pqSize":1950751283}
{"time":"2025-07-15T22:36:52.563Z","cid":"api","channel":"commands:pq","level":"info","message":"pq write completed","totalEvents":9343308,"totalBytes":1114732074,"pqSize":1955629794,"elapsedMs":7018}
```

> Output includes the number of events and bytes processed every second, and a final summary on completion.
>
{.box .info}

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-o <path>` | **(Required)** Directory path to store persistent queue files. |
| `-b <size>` | Buffer size (in bytes) used during write. |
| `-s <size>` | The maximum amount of data stored (in bytes) in each persistent queue before the file closes and starts a new file. |
| `-c <type>` | Compression type. The default is `none` but `gzip` is also supported. |
