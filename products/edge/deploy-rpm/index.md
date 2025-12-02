# Installing Edge on Linux via RPM


Install Edge in a tightly managed Linux environment
by using the signed RPM package installation method.

---

The RPM will create a `cribl-edge` user and a `cribl-edge` group. The
`cribl-edge` user will own the `cribl-edge` configuration file (stored in the
`/etc/sysconfig` directory). The `config` file contains the Leader's auth token.
To keep the Leader's auth token secure, only `root` and the `cribl-edge` user
can read/modify the `config` file contents.

## Download Cribl Edge as an RPM Package

To download Cribl Edge as a signed RPM package:

1. Go to [cribl.io/download](https://cribl.io/download/).
1. Select the dropdown in the Software area labeled **Select Release Version**. 
1. Choose the Cribl Edge RPM version and type: `<version> x64 Cribl Edge RPM`, or `<version> ARM Cribl Edge RPM`.
1. Select **Download now**.

## Verifying the RPM Package Signature

To verify the RPM package signature, you'll need to enable local GPG package checking,
then download the GPG key, import it, and verify its signature. We'll go over how
to accomplish both of these steps below.

### Enable Local GPG Package Checking

Your environment may not have the GPG check for local packages enabled by default.

To enable the local package GPG checks for `yum` or `dnf`, 
add `localpkg_gpgceheck=1` to the `main` stanza in:

- `/etc/yum/conf` for `yum`
- `/etc/dnf/dnf.conf` for `dnf`

For example:

```
[main]
...
localpkg_gpgcheck=1
```

### Download and Import the GPG Key

Cribl's public GPG key ensures the authenticity and integrity of the Cribl RPM package.

1. Import the public GPG key to `rpm` using this command:

    ```
    rpm –-import https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG
    ```

1. Verify the package signature using this command:

   ```
   rpm -Kv <rpm-file>
   ```

   This command ensures the package's integrity and authenticity, 
   and returns information about the package.
   It also checks if anyone has tampered with the package since the original developer or distributor signed it. 
   This helps prevent you from installing potentially malicious or compromised packages.

## Installing Edge via RPM

Follow the installation steps to install the RPM package on a Cribl Linux Edge
Node. 

> Out of the box, the RPM will install the software into the `/opt` directory.
> The RPM package will configure but not enable the Systemd service. You will
> need to enable the software service separately. 
{.box .info}

To install Cribl Edge via RPM:

1. Open a CLI and run the following command, drawing the file name from the [cribl.io/download](https://cribl.io/download/) page:

    ```
    sudo yum install https://cdn.cribl.io/dl/<version>/<file name>
    ```

    > You can quickly get the CDN file name from the checksum links for each download.
    {.box .success}

    This command does the following:

    - Creates the `cribl-edge` user and group, if it doesn't exist.
    - Puts the contents of Cribl’s TGZ into `/opt/cribl-edge` and makes `root/root`
      the owner.
    - Creates the `cribl-edge` systemd service, which is disabled by default.
    - Creates `/etc/sysconfig/cribl-edge.conf` owned by `cribl-edge:cribl-edge` and
      only accessible by the `cribl-edge` user.
    - Creates `/var/lib/cribl-edge/` owned by `cribl-edge:cribl-edge` and only
      accessible by the `cribl-edge` user.
    - Sets `CRIBL_INSTALL_TYPE` to `RPM` in the service destination, to block
      distributed upgrades, as upgrading from the Leader bypasses the security of RPM.
    - Sets `CRIBL_VOLUME_DIR` to `/var/lib/cribl-edge` in the service destination, so
      configs and logs are saved to the `cribl-edge` directory.

1. Edit the environment variables in `/etc/sysconfig/cribl.conf`, in order for
   Cribl Edge to connect to a Leader. The RPM installs `cribl-edge` in standalone
   mode by default (`CRIBL_DIST_MODE = edge`).

   <br> To deploy Edge in a managed deployment, edit these environment variables:
    
     - `CRIBL_DIST_MODE = managed-edge`
     - `CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`

1. Enable the service:

  - `sudo systemctl enable cribl-edge` to enable the service on boot.
  - `sudo systemctl start cribl-edge` to start it manually.

## Upgrading Edge via RPM

See [Upgrade RPM Installations](upgrade-nodes-manually#rpm).

## Uninstalling Edge via RPM

To remove Cribl that you installed via RPM, use the command line:

```
sudo yum remove cribl-edge
```

Uninstalling Cribl removes the `cribl` or `cribl-edge` service, the `cribl` binary
from `/opt/`, and the service file from `/lib/systemd/system`.

Configuration files are maintained, along with any environment variables you
edited when you first installed Cribl. This means you can pull in your previous
configuration(s) if you decide to reinstall Cribl via RPM. 

The `cribl-edge` directory is also maintained, along with the data, logs, and
defaults of your configuration. 
