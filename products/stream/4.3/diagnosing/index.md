# Diagnosing Issues


To help diagnose Cribl Stream problems, you can share a diagnostic bundle with Cribl Support. The bundle contains a snapshot of configuration files and logs at the time the bundle was created, and gives troubleshooters insights into how  Cribl Stream was configured and operating at that time. 

## What's in the Diagnostic Bundle

The following subdirectories (and their contents)  of `$CRIBL_HOME` are included:
- `.../default/*`
- `.../local/*` – except for `/local/cribl/auth/`, to exclude sensitive files.
- `.../log/*`
- `.../groups/*`
- `.../state/jobs/*` – will return all jobs if left empty. 

As a security measure, the bundle excludes all `.crt`, `.pem`, `.cer`, and `.key` files from all `$CRIBL_HOME` subdirectories.

## Creating and Exporting a Diagnostic Bundle

If you're managing your own  Cribl Stream deployment (single-instance [Stream](/stream/deploy-single-instance)/[Edge](/edge/deploy-single-instance#run-edge) or distributed [Stream](/stream/deploy-distributed)/[Edge](/edge/fleet-management)), you can create and securely share bundles with Cribl Support, either from the UI or via the CLI. 

> With a [Cribl.Cloud](deploy-cloud) deployment, create bundles and securely share them with Cribl Support directly from the UI. In the [Using the UI](#ui) instructions below, **Send to Cribl Support** will always be enabled, and the **Export**/download option will be unavailable.
>
{.box .cloud}

### Requirements {#reqs}

Using either medium, you'll need outbound internet access to `https://diag‑upload.cribl.io` and a valid support case number. The above URL works only when using the `cribl diag` command or uploading using the  Cribl Stream UI. (So connecting directly to it with your web browser will fail.)

Open your support case before sending a diagnostic bundle to Cribl Support. Then, when creating the bundle, be sure to use the case number provided. Case numbers are 8 digits in length, and you must include any leading zeros. For example, `00001234`. 

### Using the UI {#ui}

Depending on the issue you're debugging, you can create a bundle either from the Leader or from an individual Worker Node's host. 

#### From the Leader {#leader}

To create a bundle from the Leader, go to **Settings** > **Global Settings** > **Diagnostics** > **Diagnostic Bundle** and click **Create Diagnostic Bundle**. 
- To share the bundle with Cribl Support, toggle **Send to Cribl Support** to `Yes`. Enter your case number, and then click **Export**. 
- To download the bundle locally to your machine (on-prem only), click **Export**.

#### From Worker Nodes {#worker}

To create a bundle from individual Worker Nodes, you must be able to access the Worker Node's UI. You can easily use the Leader to get authenticated access to each Worker Node, if you first enable the parent Worker Group's UI Access (teleporting) setting:

1. Go to **Manage** > **Worker Groups**.
1. Make sure the relevant Worker Group's **UI Access** toggle is on. 
1. In the **Worker Nodes** column, click the displayed number to access the **Workers** (Cribl Stream) or **Edge Nodes** (Cribl Edge) page.
1. In the **GUID** column, click the link for the Worker Node you're debugging. 
1. Look for the purple border to confirm that you've teleported to that Worker Node.
1. Click **Worker Node Settings** (top right) > **Diagnostics** (lower left) > **Diagnostic Bundle**, and click **Create Diagnostic Bundle**.
1. Use the appropriate **Export** options listed [above](#leader) for the Leader.

On customer-managed/on-prem deployments, new and previously created bundles are stored in `$CRIBL_HOME/diag`. They're also listed in the UI, where you can re-download them or share them with Cribl Support.

#### UI Options {#ui-options}

The **Create & Export Diag Bundle** modal includes:
- **Rename .js files** toggle (defaults to `Yes`).
- **Include metrics** toggle (defaults to `Yes`).
- **Latest number of jobs to include** field (defaults to empty = unlimited).

Cribl Support will advise you on changing the defaults here. Disabling **Include metrics** can reduce the diag bundle's size, where necessary.

### Using the CLI {#cli}

To create a bundle using the CLI, use the `diag` command. (This option is on-prem only.) Depending on the issue you're debugging, you can create a bundle either from the Leader's host or from an individual Worker Node's host.

``` {title="diag command CLI"}
# $CRIBL_HOME/bin/cribl diag
Usage: [sub-command] [options] [args]

Commands:
list   - List existing Cribl diagnostic bundles
create - Creates diagnostic bundle for Cribl 
send   - Send Cribl diagnostic bundle to Cribl Support, args:
   -c <caseNumber> - Cribl Case Number
  [-p <path>]      - Diagnostic bundle path (if empty, then new bundle will be created)

## Creating a diagnostic bundle
# $CRIBL_HOME/bin/cribl diag create
Created Cribl Stream/Edge diagnostic bundle at /opt/cribl/diag/ <product>-<hostname>-<datetime>.tar.gz.

## Creating and sending a diagnostic bundle
# $CRIBL_HOME/bin/cribl diag send -c <caseNumber>
Sent Cribl diagnostic bundle to Cribl Support

## Sending a previously created diagnostic bundle
# $CRIBL_HOME/bin/cribl diag send -p /opt/cribl/diag/ <product>-<hostname>-<datetime>.tar.gz -c <caseNumber>
Sent Cribl diagnostic bundle to Cribl Support
```

For complete command syntax, see [diag](cli-reference#diag).

##  Including CPU Profiles  {#diag-profile}

If Cribl Support asks you to grab CPU profiles of Worker Processes, follow these steps:

1. Use `top` or `htop` on the Worker Node to identify Worker Node PIDs consuming a lot of CPU.
2. See [Measuring CPU or API Load](https://docs.cribl.io/docs/scaling#profiling) for instructions on accessing the UI's **Profile** options (for your deployment type), and generating and saving profiles.
3. Find the Worker Processes matching the PIDs you identified above.
4. Click **Profile** on each. Start with the default 10-second **Duration**.
5. Once the profile is displayed, save it to a `.json` file. (See details at the above link.)
6. Repeat steps 3–5 for other CPU-intensive Worker Processes.
7. Upload the profile `.json` files to Cribl Support.

> On an already CPU-starved Worker Node, profiling might fail with an error message, or just hang. In this case, you might need a few retries to get a successful profile.
>
{.box .info}

## Including Memory Snapshots {#memory-snapshots}

> Cribl.Cloud does not support taking memory snapshots. This feature is for on-prem deployments only.
>
{.box .success}

If you encounter a memory leak, Cribl Support might request a memory snapshot of a `cribl` process to better understand where the leak is occurring. To capture the memory snapshot:

1. Note the PID of the process of interest.
1. In a terminal, `cd` to the directory where  Cribl Stream is installed:
    ```shell
       cd $CRIBL_HOME/bin
    ``` 
1. Run the `diag heapsnapshot` command with the PID you noted in step 1:
    ```shell
       ./cribl diag heapsnapshot -p <pid>
    ```

Cribl will capture the memory snapshot in `$CRIBL_HOME/diag/`.

Example output:

```
Heap-1690906031148-2458958.heapsnapshot
```


