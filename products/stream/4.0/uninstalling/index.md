# Uninstalling


Removing an Cribl Stream installation, whether for a clean reinstall or permanently

----

## Uninstalling the Standalone Version
- (Optional) To prevent systemd from trying to start Cribl Stream at boot time, run the following command:  
    `sudo $CRIBL_HOME/bin/cribl boot-start disable`

    If you're running both Cribl Stream and Cribl Edge on the same host, be sure to execute this command in the same mode (Stream or Edge) and the same directory in which you originally ran `cribl boot‑start enable`.

- Stop Cribl Stream (stopping the main process).
- Back up necessary configurations/data.
- Delete the `cribl` user: `userdel cribl`
- Delete the user group: `groupdel cribl`
- Remove the directory where Cribl Stream is installed.
- Directly delete any local files that Cribl Stream added while running – e.g., the `.dat` file and other local configs.

In a [distributed deployment](deploy-distributed), repeat the above steps for the Leader instance and all Worker instances.


## Uninstalling the Splunk App Version
- Stop Splunk.
- Back up necessary configurations/data.
- Remove the Cribl App in `$SPLUNK_HOME/etc/apps`.
- Remove the Cribl module in `$SPLUNK_HOME/etc/modules/cribl` (some versions).
