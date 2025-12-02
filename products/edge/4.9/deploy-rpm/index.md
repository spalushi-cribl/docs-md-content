# Installing Edge on Linux via RPM


If you need to install Edge in a tightly managed Linux environment, use
the signed RPM package installation method.

The RPM will create a `cribl-edge` user and a `cribl-edge` group. The
`cribl-edge` user will own the `cribl-edge` configuration file (stored in the
`/etc/sysconfig` directory). The `config` file contains the Leader's auth token.
To keep the Leader's auth token secure, only `root` and the `cribl-edge` user
can read/modify the `config` file contents.

## RPM Download Links

|<div style="width: 150px">Cribl Edge Version</div>|<div style="width: 200px">RPM Download Link</div>|<div style="width: 200px">GPG Key</div>|
|------------|-----------------|-------|
|4.9.3       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.3/cribl-edge-4.9.3-25d56bdd-linux-arm64.rpm.md5)</li></ul></ul>|[4.9.3 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.9.2       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.2/cribl-edge-4.9.2-f4fd0d71-linux-arm64.rpm.md5)</li></ul></ul>|[4.9.2 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.9.1       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.1/cribl-edge-4.9.1-e2787645-linux-arm64.rpm.md5)</li></ul></ul>|[4.9.1 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.9.0       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.9.0/cribl-edge-4.9.0-9ebe3193-linux-arm64.rpm.md5)</li></ul></ul>|[4.9.0 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.8.2       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.2/cribl-edge-4.8.2-fe2c201d-linux-arm64.rpm.md5)</li></ul></ul>|[4.8.2 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.8.1       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.1/cribl-edge-4.8.1-8a1f8821-linux-arm64.rpm.md5)</li></ul></ul>|[4.8.1 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.8.0       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.8.0/cribl-edge-4.8.0-acf5cf96-linux-arm64.rpm.md5)</li></ul></ul>|[4.8.0 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.7.3       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.3/cribl-edge-4.7.3-6f48361f-linux-arm64.rpm.md5)</li></ul></ul>|[4.7.3 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.7.2       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.2/cribl-edge-4.7.2-5b281048-linux-arm64.rpm.md5)</li></ul></ul>|[4.7.2 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.7.1       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.1/cribl-edge-4.7.1-b46d574d-linux-arm64.rpm.md5)</li></ul></ul>|[4.7.1 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.7.0       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.7.0/cribl-edge-4.7.0-de5462cb-linux-arm64.rpm.md5)</li></ul></ul>|[4.7.0 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.6.1       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.6.1/cribl-edge-4.6.1-aafa694f-linux-arm64.rpm.md5)</li></ul></ul>|[4.6.1 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.6.0       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.6.0/cribl-edge-4.6.0-b495abf6-linux-arm64.rpm.md5)</li></ul></ul>|[4.6.0 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.5.1       |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.5.1/cribl-edge-4.5.1-388536d4-linux-arm64.rpm.md5)</li></ul></ul>|[4.5.1 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|
|4.5         |<ul><li>[**x64**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-x64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-x64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-x64.rpm.md5)</li></ul><li>[**arm64**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-arm64.rpm)</li><ul><li>[**SHA256**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-arm64.rpm.sha256)</li><li>[**MD5**](https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-arm64.rpm.md5)</li></ul></ul>|[4.5.0 RPM Public Key](https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG)|

## Verifying the RPM Package Signature

To verify the RPM package signature, enable local GPG package checking.
Next, download and import the GPG key and verify its signature.

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

1. Download the GPG key: https://cdn.cribl.io/dl/CRIBL_RPM_PUBLIC_GPG.
1. Manually import the public GPG key to `rpm` using this command:

    ```
    rpm --import <URL/filepath>
    ```

    For example:

    ```
    rpm –-import https://cdn.cribl.io/dl/4.5.0/CRIBL_RPM_PUBLIC_GPG
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

1. Open a CLI and run one of the commands, depending on the processor:

    ```
    sudo yum install https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-x64.rpm
    ```

    Or

    ```
    sudo yum install https://cdn.cribl.io/dl/4.5.0/cribl-edge-4.5.0-1a628515-linux-arm64.rpm
    ```

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

  - `systemctl enable cribl-edge` to enable the service on boot.
  - `systemctl start cribl-edge` to start it manually.

## Upgrading Edge via RPM

See [Upgrading Edge via RPM](upgrading-manually-linux-rpm).

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
