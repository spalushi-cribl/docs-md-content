# Deploy Stream Configurations


Deployment is the last step after [configuration changes](config-bundle-management) have been saved and committed. *Deploying* here means **propagating updated configurations to Worker Nodes.** 

The typical workflow for deploying Cribl Stream configurations is the following:

1. Configure.
2. Save.
3. Commit (and optionally push).
4. Deploy.

You deploy new configurations at the Worker Group level: Locate the desired Worker Group and select **Deploy**. Worker Nodes that belong to the group will start **pulling** updated configurations on their next check-in with the Leader Node. 

> ##### Can't Log In to the Worker Node as Admin User?
>
> When a Worker Node pulls its first configs, the admin password will be randomized, unless specifically changed. This means that users won't be able to log in on the Worker Node with default credentials. For details, see [Set Worker/Edge Node Passwords](authentication#worker_pwd).
>
{.box .warning}

## Configuration and Lookup File Locations

| Node Type      | Configuration Location                                      | Lookup File Location                                      |
|----------------|-------------------------------------------------------------|-----------------------------------------------------------|
| Leader Node    | `$CRIBL_HOME/groups/<groupName>/local/cribl/`               | `$CRIBL_HOME/groups/<groupName>/data/lookups`             |
| Worker Node    | `$CRIBL_HOME/local/cribl/` (after pulling configurations)   | `$CRIBL_HOME/data/lookups` (after pulling configurations) |

When deployed via the Leader Node, [lookup files](lookups-library) are distributed to Worker Nodes as part of a configuration deployment.

If you want your lookup files to be part of the Cribl Stream configuration version control process, we recommended deploying using the Leader Node. Alternatively, you can update your lookup file on the individual Worker Nodes, which can be useful for larger lookup files (greater than 10 MB, for example), for lookup files maintained using some other mechanism, or for lookup files that are updated frequently.

For other options, see [Managing Large Lookups](large-lookups).

> Some configuration changes will require restarts, while many others require only reloads. See [Configurations and Restart](configuration-files#restart) for details. 
> 
> Restarts and reloads of each Worker Process are handled automatically by the Worker Node. Note that individual Worker Nodes might temporarily disappear from the Leader's **Workers** tab while restarting.
>
{.box .info}

## Worker Process Rolling Restart

During restarts that do not involve restarting the API process, Worker Processes on a Worker Node restart in a rolling fashion to minimize disruption and maintain as much processing capability as possible. **20% of running processes – with a minimum of one process – are restarted at a time.** All Worker Processes in a restarting batch must come up and report as **started** before the next batch is initiated. This rolling restart continues until all processes have restarted. If a Worker Process fails to restart, configurations will be rolled back.

If a restart does involve restarting the API process, Worker Processes on a Worker Node do not restart in a rolling fashion. All Worker Processes must stop before the API can restart, and once the API restarts, it starts up all Worker Processes.
