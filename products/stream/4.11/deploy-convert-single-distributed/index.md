# Converting a Single Instance to Distributed Deployment


If you've configured a Cribl Stream [Single-instance deployment](deploy-single-instance) and now want to promote it to [Distributed](Deploy-distributed), here are a couple of approaches to doing so, while retaining the configuration you've already created.

## Simple Copy

We'll start with the simplest scenario, in which you plan to set up distributed mode with a single Worker Group. By default, this group will literally be named `default`. (We'll also explain how to extend this scenario.)

1. If you haven't already installed `git` (required for the Leader), do so as outlined [here](requirements#git).

1. Stop the Cribl Stream server (`./cribl stop`).

1. Your single instance's configs are under Cribl Stream's `local/` subdirectory. So, copy `$CRIBL_HOME/local/cribl/*` to `$CRIBL_HOME/groups/default/local/cribl/`. 

    This stages your configs for the `default` Worker Group.

1. Restart Cribl Stream, selecting **Distributed Mode**: `Leader`.

1. At this point, the Leader should have inherited your previous single-instance settings. Commit and deploy these settings to the `default` group, which should resume the same data processing that your single instance was executing.

1. If you want to replicate the same configs to additional Worker Groups, add those groups now on the **Worker Groups** page. Then repeat the preceding four steps, targeting the new subdirectories that have been created on the filesystem for the new groups.

> Creating multiple Worker Groups requires certain license tiers. For details, see [Pricing](https://cribl.io/pricing/).
>
{.box .info}

## rsync

This alternative approach uses `rsync` to replicate your single-instance configs.

1. Use this command to rsync your single-instance configuration to each of your distributed Groups (replacing the `<group‑name>` placeholder here):
    ```shell
    rsync -a $cribl/local/cribl newmaster:$cribl/groups/<group‑name>/local/
    ```

1. Restart Cribl Stream, selecting **Distributed Mode**: `Leader`.

1. Commit and deploy the new configuration.
