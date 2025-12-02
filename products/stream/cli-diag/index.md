# diag


Manages diagnostic bundles. For failover mode, see how to [prevent and troubleshoot CLI problems](cli-reference#failover-mode).

**Sub-commands:**

- [`diag create`](#diag-create)
- [`diag cpuprofile`](#diag-cpuprofile)
- [`diag heapsnapshot`](#diag-heapsnapshot)
- [`diag list`](#diag-list)
- [`diag perf`](#diag-perf)
- [`diag send`](#diag-send)

## Sub-commands and Options

### `diag create` {#diag-create}

Create a diagnostic bundle for this instance.

**Usage:**

```text
./cribl diag create
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run create in debug mode. |
| `-j` | Do not append '.txt' to js files. |
| `-k` | Include metrics for the top 10 Sources, Destinations, Pipelines, Routes, and Packs from the last 24 hours. |
| `-t <maxIncludeJobs>` | Latest number of jobs to include in bundle. |
| `-M` | Exclude metrics from bundle. |
| `-g` | Exclude git from bundle. |
| `-i` | Include install logs. |

{{<id windows>}} **Windows PowerShell usage:**

`PS > $env:CRIBL_VOLUME_DIR='c:\ProgramData\Cribl'; & 'c:\Program Files\Cribl\bin\cribl.exe' diag create`

> The Windows command requires the `CRIBL_VOLUME_DIR` environment variable. 
> If your deployment doesn't use the default path (`c:\ProgramData\Cribl`), 
> verify that `CRIBL_VOLUME_DIR` points to the correct directory.
{.box .info}

**Sample response:**

```
Created diagnostic bundle at c:\ProgramData\Cribl\diag\edge-DESKTOP-C9BEMQ2-20240925T222216.tar.gz
```

**Windows CMD usage:**

`> set "CRIBL_VOLUME_DIR=c:\ProgramData\Cribl" && "c:\Program Files\Cribl\bin\cribl.exe" diag create`

**Sample response:**

```
Created diagnostic bundle at c:\ProgramData\Cribl\diag\edge-DESKTOP-C9BEMQ2-20240925T222216.tar.gz
```

### `diag cpuprofile` {#diag-cpuprofile}

Collect a 30-second CPU profile and place in the `diag` directory.

**Usage:**

```text
./cribl diag cpuprofile -p 12345
```

**Sample response:**

```text
Created a Cribl diagnostic bundle at /opt/cribl/diag/<product>-zedborcdb72f-20210820T204405.tar.gz
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-p <pid>` | The pid of the process to dump the CPU profile. |

### `diag heapsnapshot` {#diag-heapsnapshot}

Generate heap snapshot of a Cribl process and place in the `diag` directory.

**Usage:**

```text
./cribl diag heapsnapshot -p 12345
```

**Sample response:**

```text
Heap-1672574400000-12345.heapsnapshot
```

> The response format is `Heap-<epoch-timestamp>-<pid>.heapsnapshot`.
>
{.box .info}

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-p <pid>` | The pid of the process to dump the heap snapshot. |


### `diag list` {#diag-list}

List existing diagnostic bundles.

### `diag perf` {#diag-perf}

Collect a 30-second CPU profile and a heapsnapshot and place both in the `diag` directory.

| Option | Definition |
| ------ | ---------- |
| `-p <pid>` | The pid of the process to dump both the CPU profile and heap snapshot. |

### `diag send` {#diag-send}

Send diagnostics bundle to Cribl Support.

```
./cribl diag send -c 00001234
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-c <caseNumber>` | Cribl Support Case Number. |
| `-p <path>` | Diagnostic bundle path. If empty, it creates a new bundle. |