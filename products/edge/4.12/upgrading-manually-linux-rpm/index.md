# Upgrading RPM Installations of Cribl Edge


## Considerations for Upgrading RPM Installations of Cribl Edge  {#rpm-upgrades}

Cribl Edge Leader cannot directly upgrade Edge Nodes installed using RPM packages. Upgrading through the Leader might disrupt the RPM package manager.

**Recommended method**: Manual Upgrade with Package Manager.  

### Manually Upgrading RPM Installations of Cribl Edge 

1. Download the new Cribl Edge RPM package for your desired version.

1. Use your system's package manager (e.g., `yum`, `apt-ge`t) to upgrade the existing Cribl Edge package. This approach offers more granular control but requires manual intervention on each node.
   
  You must use the command line:
   ```
   sudo yum update <CDN URL for new RPM>
   ```

We’ve disabled UI upgrades, because upgrading from the Leader bypasses the
security of RPM. 

Any changes you make to environment variables, like editing or adding new
variables, are maintained when you upgrade, as well as when the service starts
(on boot or manually).

For large organizations, it's recommended to establish an internal repository for RPM packages. This centralized approach streamlines upgrades by eliminating the need to download individual RPMs to each node, improving efficiency and consistency.




