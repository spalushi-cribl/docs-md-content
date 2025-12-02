# OS and System Requirements


For a successful Cribl Stream deployment, verify that your system meets the minimum hardware and software specifications outlined below.

> All the quantities listed below are **minimum** requirements. 
> Adjust them as needed based on your specific use case.
> 
{.box .info}

## OS Requirements

You can install Cribl Stream on any common Linux distribution that fulfils the following criteria:

- 64-bit kernel >= 3.10
- glibc >= 2.17
- OpenSSL >= 3.0 (if FIPS mode is enabled)

Highly customized or rare Linux distributions might not be supported even when based on a supported kernel version.

Enforcing mode in [SELinux](usecase-selinux) is supported, but not required.

### Tested Linux Platforms

Cribl tests the following Linux versions to confirm compatibility with the supported kernel.
This list does not constitute a comprehensive record of all supported distributions and versions.

- Ubuntu 24.04
- RHEL 9, 9.3
- CentOS Stream 9
- SUSE Linux Enterprise Server 15.6
- Amazon Linux 2023

> As of October 16, 2024, Cribl has deprecated support for the CentOS Linux 7 distribution, which reached its end of life on June 30, 2024.
>
{.box .info}

## System Requirements

The following specs are the minimum application requirements for Cribl Stream,
either a Leader, a Worker Node, or a Single-instance deployment.

Given the Leader's critical role in coordinating Worker instances,
we recommend deploying it on a stable and highly available infrastructure.

- 1 GHz (or faster), 64-bit CPU
- 4+ physical cores (beyond basic OS/VM requirements, 1 physical core is equivalent to 2 virtual/hyperthreaded CPUs (vCPUs).)
- 8 GB+ RAM (beyond basic OS/VM requirements)
- 5 GB free disk space

> See [Sizing and Scaling](scaling) for capacity planning details. 
{.box .info}

### Cloud VM Recommendations

To fulfill the requirements using cloud-based virtual machines, see [Recommended AWS, Azure, and GCP Instance Types](scaling#vms).

### Core Equivalency

We consider 1 physical core to be equivalent to:

- 2 virtual/hyperthreaded CPUs (vCPUs) on Intel/Xeon or AMD processors
- 1 (higher-throughput) vCPU on Graviton2/ARM64 processors

### Memory Requirements

- Each Worker Process can consume memory up to its maximum heap size, plus Node.js overhead, and additional memory that scales with configuration (e.g., Destination type and number).
- Ensure your total host memory allocation covers the memory usage of all Worker Processes and OS requirements.
- Continuously monitor Node memory usage, especially after configuration changes.
- For additional details, see [Estimating Number of Cores](/stream/scaling#estimating-requirements).

> Since version 4.13, due to an upgrade of the Node.js version used by Cribl Stream,
> baseline memory usage may increase on hosts that are not memory-constrained,
> because the new Node.js version is more conservative with memory release.
{.box .info}

## Browser Support {#browser}

The Cribl Stream UI can run on Chrome, Firefox, Safari, and Microsoft Edge, on their five most-recent versions.

## Additional Requirements

`git` must be available on the Leader Node. See details [below](#git).

## Set Capabilities for Cribl Stream

If Cribl Stream needs to listen on low ports 1–1024, it will need privileged access. You can set Linux capabilities that grant Cribl Stream sufficient rights to perform specific privileged tasks, based on kernel privilege. To run Cribl Stream  as non-root user, consider setting the following capabilities: 

| Set Linux Capabilities| Permissions|
|--|--| 
| `CAP_NET_BIND_SERVICE` | Allows Cribl Stream to push Sources that bind to TCP/UDP port numbers below `1024`. |
| `CAP_DAC_READ_SEARCH` | Allows the `cribl` user to access the File Monitor Source's [Manual mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. This capability bypasses the default Linux permissions for files and directories.|
| `CAP_SYS_PTRACE`  | Allows the `cribl` user to scan open files for running processes, and to access the File Monitor Source's [Auto-mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. | 

For details about setting these capabilities, see [Persisting Overrides](run-stream#persisting-overrides).

### Set the `CRIBL_HOME` Environment Variable

The `CRIBL_HOME` env is available in the Cribl Stream application, but not on your terminal. If you want to use `$CRIBL_HOME`, you can:

- Assign it once, using the `export` command: `export CRIBL_HOME=/opt/cribl`
- Set it as a default, by adding it to your to your terminal profile file.

## Version Control with `git` {#git}

Cribl Stream requires `git` (version 1.8.3.1 or higher) to be available locally on the host where the Leader Node will run. **Configuration changes must be committed to git before they're deployed.**

If you don't have `git` installed, see [Git Documentation](https://git-scm.com/download/linux) for details on how to get started.

The Leader Node uses `git` to:
* Manage configuration versions across Worker Groups.
* Provide users with an audit trail of all configuration changes.
* Allow users to display diffs between current and previous config versions.


## Port Requirements

- Configure firewalls on Leader and Worker hosts to allow outbound HTTPS connections to `https://cdn.cribl.io` on port 443 during installation.

- Refer to the list of required [ports](ports) for Distributed deployments to function, and verify that they are continously open.

- If your network requires a proxy, configure your system proxy settings as described in [System Proxy Configuration](proxy-config). Be sure to exclude cluster communication from the proxy to avoid performance issues.

- Cribl prioritizes S3 downloads for Worker Nodes, with automatic fallback and firewall allowlisting tips for optimal performance. See, [Optimized Bundle Delivery from S3](config-bundle-management#optimized-S3bundle) for details. 

## FIPS Mode Requirements {#fips-requirements}

Federal Information Processing Standards ([FIPS](https://www.nist.gov/standardsgov/compliance-faqs-federal-information-processing-standards-fips)) is a set of US government standards and guidelines for information security. You can deploy Cribl Stream in **FIPS mode**. This mainly restricts the cryptographic algorithms used within Cribl Stream, and also enforces stricter password requirements.

To learn about system and password requirements, as well as instructions for running Cribl Stream in FIPS mode, refer to the [FIPS Mode](fips-mode) document. 