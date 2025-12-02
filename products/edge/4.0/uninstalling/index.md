# Uninstalling Cribl Edge from Linux


Removing a Cribl Edge installation, whether for a clean reinstall or permanently

----

## Uninstalling a Single Edge Node

- (Optional) To prevent systemd from trying to start Cribl Edge at boot time, run the following command:  
    `sudo $CRIBL_HOME/bin/cribl boot-start disable`

    If you're running both Cribl Edge and Cribl Stream on the same host, be sure to execute this command in the same mode (Edge or Stream) and the same directory in which you originally ran `cribl boot‑start enable`.  

- Stop Cribl Edge (stopping the main process).
- Back up any necessary configurations/data.
- Delete the `cribl` user: `userdel cribl`
- Delete the user group: `groupdel cribl`
- Remove the directory where Cribl Edge is installed.
- Directly delete any local files that Cribl Edge added while running – e.g., the `.dat` file and other local configs. 

## Uninstalling a Distributed Deployment

In a [distributed deployment](fleet-management), repeat the above steps on: 
- The Leader instance. 
- All Edge Node instances.
