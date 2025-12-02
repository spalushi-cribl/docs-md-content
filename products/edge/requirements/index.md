# OS and System Requirements


For a successful Cribl Edge deployment, verify that your system meets the minimum hardware and software specifications outlined below.

> All the quantities listed below are **minimum** requirements. 
> Adjust them as needed based on your specific use case.
> 
{.box .info}



## OS Requirements

You can install Cribl Edge Nodes on both Linux and Windows systems.

### Linux Requirements

Cribl Edge Nodes can run on any common Linux distribution that fulfils the following criteria:

- 64-bit kernel >= 3.10
- glibc >= 2.17
- OpenSSL >= 3.0 (if FIPS mode is enabled)

Highly customized or rare Linux distributions might not be supported even when based on a supported kernel version.

Enforcing mode in SELinux is supported, but not required.

#### Tested Linux Platforms

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

### Windows Requirements



Cribl Edge supports these versions of Windows:

|<div style="width: 150px">Version</div>|Notes|
|-------------|----------------|
|Windows 10 and 11| You can deploy Cribl Edge on laptops, desktops, and workstations running Windows 10 and 11. <br>Keep these things in mind when deploying Cribl Edge on Windows 10 and 11 workstations:<ul><li>Cribl Edge might lose some data sent to TCP Destinations after a workstation resumes from a suspended or sleep state. This happens when the data is in local network buffers but hasn't been sent out to the Destination. Instead, you can send data to an HTTP Destination.</li><li>You might see partially broken events after a laptop resumes from a suspended state.</li><li>HTTP Destinations might contain duplicate events.</li><li>Some users have observed excessive logging when a Leader is unavailable to receive heartbeat messages from an Edge Node.</li></ul>|
| Windows Server 2016 <br> Windows Server 2019 <br> Windows Server 2022 <br> Windows Server 2025 | Functionality is the same as generally supported Windows Server platforms. |


### MacOS Requirements {#macosreq}

Cribl Edge supports the following macOS versions:
26 (Tahoe), 15 (Sequoia), 14 (Sonoma).

You can install Cribl Edge on both Intel and Apple silicon processors.

## System Requirements

Edge Nodes should have sufficient CPU, RAM, network, and storage capacity to handle your specific workload.
While minimum specifications are provided, optimal configurations vary widely.
Consider data volume, types, processing needs, and organizational standards when selecting hardware.

Make sure to test the performance of your Edge Nodes before deploying to production.

Minimum application requirements for Edge Nodes are:

- ~1Ghz processor
- 512MB RAM
- 5GB free disk space per volume for operational purposes

If your installation uses multiple volumes
(for example, one volume to store [persistent queues](persistent-queues)
and one for Destination [staging directories](destinations#staging-directory)),
5GB is the minimum disk space for each of those volumes.
You can configure the disk space limit per Fleet with the [**Min free disk space**](fleet-settings#storage) setting.

> See also [Cribl Stream Requirements](/stream/requirements) to learn about the requirements for the Leader Node.
{.box .info}

## Browser Support {#browser}

The Cribl Edge UI can run on Chrome, Firefox, Safari, and Microsoft Edge, on their five most-recent versions.