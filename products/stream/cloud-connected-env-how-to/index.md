# How to Connect On-Prem Leaders to Cribl.Cloud


To connect existing on-prem Cribl Stream Distributed deployments to Cribl.Cloud,
select an on-prem Leader that is running Cribl Stream 4.8.2 or newer, then follow the steps.

>If you have multiple environments, such as staging and production environments
>when using GitOps, you must connect each environment individually.
{.box .info}

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Workspace**, and then select **Connections**.
1. Select **Configure New Environment** and fill out the following fields:

   |Field|Description|Example|
   |-----|-----------|-------|
   |<div style="width: 150px">Environment name</div>|A name for your environment. This name will display on the **Connected Environments** page. You can search for and filter environments by name.|Goat Farm East Production|
   |Workspace hostname/ID|<div style="width: 150px">Hostname of your Cribl.Cloud Workspace.</div>This field is read-only.|`example-goat-farm-prod.cribl.cloud`|
   |Workspace port number|Port number that the Workspace Leader uses to communicate with the on-prem Leader.|`4200`|
   |Workspace token|The auth token of the Cribl.Cloud Leader. This field is read-only and is automatically populated.|`a12bcDEFgHiJKLmnOpQRstuvw3x4z56`|
   |Script type|The format of the script to connect to the on-prem Leader: <ul><li>Environment variable</li><li>CLI</li></ul>|**Environment variable**: <pre><br>CRIBL_DEPLOYMENT="Goat Farm West Production" <br>CRIBL_CLOUD_WORKSPACE_URL="tls://a12bcDEFgHiJKLmnOpQRstuvw3x4z56@example-goat-farm-prod.cribl.cloud:4200"</pre> **CLI**: <pre><br>./cribl cloud-workspace <br>-d "Goat Farm West Production" <br>-H example-goat-farm-prod.cribl.cloud <br>-p 4200 <br>-u a12bcDEFgHiJKLmnOpQRstuvw3x4z56</pre>|

## Use a Command-Line Script

If you're using the CLI option, follow these steps, in order:

1. Select **Copy Script**.
1. Open a command-line window in the Leader environment that you want to connect to Cribl.Cloud.
1. Navigate to the `$CRIBL_HOME/bin` subdirectory.
1. Paste the script results onto the command line, and run the script.

## Use an Environment Variable

The **Environment Variable** script populates with the details you enter when
you're configuring a new environment. For example:

```
CRIBL_DEPLOYMENT="On-prem Stream"
CRIBL_CLOUD_WORKSPACE_URL="tls://a12bcDEFGHIjKLMnopPqr123@cribl-cloud-account-0a1bcde.cribl.cloud:4200"
```

If you're using the environment variable, copy its output,
and set it when – or before – you run `cribl start`.
How you set this variable depends on your deployment.

 - For example, if your Cribl Stream deployment is running in a container,
   set the environment variable in the container via run call or configuration.

 - If you're running your own server, you can set environment variables for the `cribl` user,
   or you can declare the variable along with the `start` command.
   For details, see our [CLI Reference](cli-cloud-workspace).

## High Availability Leader Configuration

Before you configure a connected environment with a High Availability (HA) Leader,
ensure you are comfortable setting up and managing Cribl Stream Leader HA. See
[Leader High Availability/Failover](deploy-add-second-leader#how-it-works)
for more information.

When you set up the primary Leader to connect to Cribl.Cloud in an HA environment,
set the `CRIBL_DEPLOYMENT` and `CRIBL_CLOUD_WORKSPACE_URL` environment variables.
You can do this in the Cribl.Cloud UI or using CLI.

For example:

```
Environment=CRIBL_DEPLOYMENT=aws_leader_prod
Environment=CRIBL_CLOUD_WORKSPACE_URL=tls://large-goat-farm.cribl.cloud:4200
```

The failover Leader will inherit the configuration from the filesystem changes
made by the primary Leader.

## Enable the On-Prem Leader Connection to Cribl.Cloud {#enable-connection}

You can connect a Cribl Stream Leader to an existing Enterprise Cribl.Cloud Organization.

1. In the sidebar, select **Settings**.
1. On the **Global** tab, select **Distributed Settings**.
1. Select **Cloud Connection**.
1. Toggle **Join Cribl.Cloud Workspace** on.
1. Fill in the required fields:

   |Field|Description|
   |-----|-----------|
   |**Connection URL**|The **Cribl.Cloud URL** to connect to this Leader. <ol><li>In your Cribl.Cloud Organization, locate the URL field value on the sidebar **Workspace** > **Access** tab.</li><li>Copy the URL and paste it into the **Connection URL** field, using the format: `main-random-goat-a12bcdef.cribl.cloud`.</li></ol> |
   |**Connection port**|The port for the Cribl.Cloud Leader. Default is `4200`.|
   |**Connection auth token**|The authentication token for the Cribl.Cloud Leader. You'll need to get this from the Cribl.Cloud environment: <ol><li>Navigate to **Workspace** > **Connections** and select **Configure New Environment**.</li><li>Copy the value in the **Workspace token** field.</li><li>In the on-prem environment, paste the Workspace token from Cribl.Cloud.</li></ol>|

1. Select **Save**.
1. In the Cribl Subscription Services Agreement modal, select **Agree** to accept the agreement.
1. In the confirmation modal, select **Yes** to save changes and restart.

    >Once you connect an on-prem Leader to a Cribl.Cloud Organization, the license
    >type changes to a Cribl.Cloud billing account. This means the **Licensing** page will not
    >be accessible. To return to using an on-prem license, toggle **Join Cribl.Cloud Workspace** off, select **Save**, and select **Yes** in the confirmation modal
    > to save changes and restart.
    {.box .info}

