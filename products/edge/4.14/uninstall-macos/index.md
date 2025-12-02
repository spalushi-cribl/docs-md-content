# Uninstall Cribl Edge from MacOS


Cleanly remove a Cribl Edge instance from macOS.

---

To cleanly remove a Cribl Edge instance from macOS:

1. Stop the service:

   ```shell
   sudo launchctl unload /Library/LaunchDaemons/io.cribl.plist
   ```

2. Remove the installed Edge Node:

   ```shell
   sudo rm -rf /Library/LaunchDaemons/io.cribl.plist /opt/cribl
   sudo pkgutil --forget io.cribl.pkg.edge
   ```
 