# Managing Large Lookups





This page offers a general approach to managing large lookup files. While Cribl Stream's Git integration normally helps manage configuration changes, large lookups are exceptions. In many cases, you might want to exclude these files from Git, to reduce excessive deploy traffic. This approach can also prevent Git Push commands from encountering [large file errors](version-control#largefile).

Good scenarios for this approach are:

- Large binary files – like databases – which don't benefit from Git's typical efficient storage of only the deltas between versions. (With binary files, Git must replace the whole file for each new version.)

- Files updated frequently and/or files updated independent of Cribl Stream.

- Files replicated on many Worker Nodes.

> The steps below assume access to a command line and (more importantly) to your OS' filesystem. Where you lack such access – for example, in a [Cribl.Cloud](deploy-cloud) deployment – load lookup files of all sizes via Cribl Stream's UI, as outlined in [Lookups Library](lookups-library).
>
{.box .success}

## About the MaxMind GeoLite Example

We'll illustrate this with an example that often combines all three conditions: setting up the free, popular [MaxMind GeoLite2 City](https://dev.maxmind.com/geoip/geoip2/geolite2/) database to support Cribl Stream's [GeoIP](geoip-function) lookup Function. This example anticipates a Cribl Stream production [distributed deployment](deploy-distributed), where the GeoLite database is updated nightly across multiple Workers.

This example includes complete instructions for this particular setup. However, you can generalize the example to other MaxMind databases, and to other large lookup files – including large `.csv`'s that similarly receive frequent updates.

## Reducing Deploy Traffic

The general approach for handling large lookups is:

- Do not place these files in the standard `$CRIBL_HOME/data/lookups`.

- Instead, place them in a `$CRIBL_HOME` subdirectory that's excluded from Git version control, through inclusion in the `$CRIBL_HOME/.gitignore` file. Deploying the files to the Leader Node and all desired Workers will require a manual procedure and will be required for the initial deployment as well as subsequent updates.

The example below uses `$CRIBL_HOME/state` subdirectory, which is already listed in the default `.gitignore` file that ships with Cribl Stream.

> If you prefer, you can use a different path, which might be a path outside `$CRIBL_HOME`. If you choose this alternative, be sure to add that path to `.gitignore`. 
> 
> However, Cribl recommends using a `$CRIBL_HOME` subdirectory like `$CRIBL_HOME/state`, because this inherits appropriate permissions and simplifies backup/restore operations.
>
{.box .info}

Let's move on to the MaxMind GeoLite specifics.

## Download and Extract the Database

To enable the GeoIP Function using the MaxMind GeoLite 2 City database, your first steps are:

1. Create a free MaxMind account, at the page linked above.

2. Log in to your MaxMind [account portal](https://www.maxmind.com/en/account/login) and select **Download Databases**.

3. On the Download page, look for the database you want. (In this example, you'd locate the **GeoLite2 City** section.) Note the **Format: GeoIP2 Binary**, and select **Download GZIP**. 

![GeoLite2 City database: Download binary GZIP](geolite2-city-download-iframe.ba3ebccd13.png)

4. Extract the archive to your local system. 

5. Change to the directory created when you extracted the archive. This directory's name will correspond to the date you downloaded the file, so in the above `2020-10-06` example, you would use: `$ cd GeoLite2-City_20201006`

## Copy the Database File to the Leader and Worker Nodes (Recommended) {#recommended}

In distributed deployments, Cribl recommends copying the MaxMind database separately to the Leader and all Worker Nodes. For example, placing it in the `$CRIBL_HOME/state` path minimizes the Git commit/deploy overhead for nightly updates to the binary database file.

Once in the database's directory, execute commands of this form:

```shell
$ scp *.mmdb <user>@<leader-node>:
$ scp *.mmdb <user>@<worker-node>:

```



> Copy the file to each Worker in the Worker Group where you intend to use Cribl Stream's GeoIP Function.
>
{.box .warning}

The above commands copy the `.mmdb` database file into your user's home directory on each Node. Next, we’ll move it to `$CRIBL_HOME/state`  on each Node. Execute these commands on both the Leader and Worker Nodes:

```shell
$ sudo mv ~/*.mmdb <$CRIBL_HOME>/state/
$ sudo chown -R cribl:cribl <$CRIBL_HOME>/state/
```

Now that the database is in place, your Pipelines can use the GeoIP Function to enrich data. In the Function's **GeoIP file (.mmdb)** field, insert the complete `$CRIBL_HOME/state/<filename>.mmdb` file path. 

## Copy the Database File Only to the Leader (Alternative)

In smaller deployments, you might choose to copy this MaxMind database only to the Leader Node, and to let Workers receive updates via Git commit/deploy. In this case, the final commands above might look like this:

```shell {title="Shell"}
$ sudo cp ~/*.mmdb /opt/cribl/groups/<group-name>/data/lookups/
$ cd /opt/cribl/groups/<group-name>/data/lookups/
$ sudo chown cribl:cribl *.mmdb
```

## Automatic Updates to the MaxMind Database

Use the [GeoIP Update](https://github.com/maxmind/geoipupdate) program MaxMind provides to automatically update your databases. See MaxMind's [Automatic Updates for GeoIP2 and GeoIP Legacy Databases](https://dev.maxmind.com/geoip/geoipupdate/) documentation for details.

You'll need two modifications specific to Cribl Stream:

- GeoIP Update must be set up on the Leader, and on each Worker in any Group that uses GeoIP lookups.

- You need to configure GeoIP Update to point to the directory where the MaxMind database file resides. The program uses a configuration file named `GeoIP.conf` and you utilize the `DatabaseDirectory` parameter to define the location for storing the database file. See MaxMind's [GeoIP.conf](https://github.com/maxmind/geoipupdate/blob/main/doc/GeoIP.conf.md) documentation for details. 
    
    For example, the default setting in `GeoIP.conf` writes output to `/usr/local/share/GeoIP`. You must change this setting to the path where your databases actually reside. If you're using the [recommended architecture](#recommended) above, you'd set: `DatabaseDirectory <$CRIBL_HOME>/state/`.


> A Cribl Worker restart is necessary for the new data to be loaded. Even though the update happens automatically, the cached version loaded at the Worker's startup persists until the Worker is restarted. To ensure that updated MaxMind databases are used for processing, schedule regular Worker restarts to coincide with the automatic updates. This applies to all Cribl Stream deployments, both customer-managed and Cribl.Cloud.
>
{.box .warning}

## Memory Considerations

Storage aside, large lookup files can also require additional RAM on each Worker Node that processes the lookups. For details, see [Memory Sizing for Large Lookups](lookups-library#sizing).
