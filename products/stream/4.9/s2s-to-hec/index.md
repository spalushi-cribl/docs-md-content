# Switch Cribl Stream Destinations from S2S to Splunk HEC


This guide is for users that want to change their Splunk data flow from
using Splunk to Splunk (S2S) to HTTP Event Collector (HEC). The guide will
provide step-by-step instructions for making this change.

## Prerequisites

You must have the following prerequisites before following this guide:

- A Cribl Stream instance
- A Splunk instance

### Configure the `queue` Setting

If your Splunk Technology Add-ons (TAs) include many index-time operations like SEDCMD or TRANSFORMS, be aware of potential data quality impacts. By default, Splunk HEC sends data through a queue where these operations (often referred to as extractions) may be applied, potentially altering your original data.

To bypass these index-time operations (extractions) and ensure data integrity, configure the `queue` setting in the `inputs.conf` file in Splunk to use the `rulesetQueue` on a per-token stanza level. This approach allows for newer Ingest Actions to be applied through the [Ruleset queue](https://docs.splunk.com/Documentation/SVA/current/Architectures/IngestActions#Rulesets).

> Modifying the `queue` setting in the **global** stanza of `inputs.conf` will impact all HEC tokens and may have unintended consequences for other HEC-based integrations.
{.box .warning}
  
  1. Locate and open the `inputs.conf` file on your Splunk instance. This file is typically located in the `$SPLUNK_HOME/etc/system/local` directory.
  1. Add or modify the following within the file:
      
      ```
      [http://cribl]
      disabled = 0
      index = main
      token = ${SPLUNK_HEC_TOKEN}
      queue = rulesetQueue
      ```
  1. Replace `${SPLUNK_HEC_TOKEN}` with your actual HEC token.
  1. Save the `inputs.conf` file and restart the Splunk instance for the changes to take effect.

> If you are using Splunk Cloud, you must open a ticket with Splunk to request a
> change to the `inputs.conf` file. Make sure you request that they change the 
> `queue` setting only for the Cribl integration, not on a global level.
{.box .info}

## Create the Splunk HTTP Event Collector Token

Once you complete this section, you will have a new HEC token in Splunk
that can receive data from Cribl Stream. 

1. Log in to Splunk.
1. Navigate to **Settings** then **Add Data**.
1. Select **Monitor** from the **Add Data** menu.

### Select a Source

1. Choose **HTTP Event Collector**.
1. Enter a name for your token (`cribl-HEC` for example).
1. Click **Next**.

### Validate Input Settings

1. For **Source type**, select **Automatic** (unless you are setting this token for a specific Sourcetype).
1. For **Index**, select your desired Allowed Indexes. 

    > If you want to send Splunk logs such as data from `_internal`, set the
    > Allowed Indexes to `N/A`. You may want different tokens for
    > different types of data.
    {.box .warning}

1. Click **Next**.

### Review HEC Settings

1. Ensure that **Allowed indexes** it set to `N/A`, unless you want to restrict users
   to only send data to specific indexes using this token.
1. Click **Submit**.

You should now have a new, valid HTTP Event Collector (HEC) token that you can
use with Cribl Stream. This token allows Cribl Stream to receive events for any
index that is available in Splunk.

## Create a Splunk HEC Destination in Cribl Stream

Using the token you just created in Splunk, create a [Splunk HEC
Destination](destinations-splunk-hec) in Cribl Stream.

1. Log in to Cribl Stream. 
1. Click **Data** then **Destinations**.
1. From the resulting page's tiles or the **Destinations** left nav, select **Splunk**, then > **HEC**
1. Click **Add Destination** to open a **New Destination** modal and fill in the required fields:

    |Field|Configuration|
    |-----|-----------|
    |Output ID|Enter a descriptive name for your Destination.|
    |Load balancing|Decide whether to enable [Load Balancing](destinations-splunk-hec#general-settings) for this Destination.|
    |Splunk HEC Endpoints|Enter the endpoint for your Splunk instance, in the following format: `https://http-inputs-<CLOUD_ORG>.splunkcloud.com/services/collector/event`|
    |Authentication method|Set to **Manual**.|
    |HEC Auth token|Enter the Splunk HEC API access token from the [Create the Splunk HTTP Event Collector Token](#create-the-splunk-http-event-collector-token) section.|
1. Depending on your data volume and event size, you may also want to tune Advanced Settings (**Max body size**, **Request concurrency**, **Flush period (sec)**). 
   See [Splunk HEC Advanced Settings](destinations-splunk-hec#advanced-settings) for specific guidance.
1. Click **Save**. 
1. In a Distributed deployment, you'll also need to **Commit & Deploy** to push
   the configurations to your Workers.

### Test the Destination Connection

1. Open the Splunk HEC Destination you just created and click the **Test** tab.
1. Add the index field to **Test input** and click **Run test**. 
1. Check the bottom of the **Test** tab. If the banner shows a green Success indicator,
   your endpoint and token are valid.

## Switch from S2S to Splunk HEC

Now, you can switch from Splunk S2S to HEC Destination.

1. In Cribl Stream, navigate to **Data** then **Destinations**.
1. Choose either **Splunk Single Instance** or **Splunk Load Balanced** depending
   on which applies to you.
1. Open the Splunk S2S Destination and check for any **QuickConnects** Sources.
1. Click directly on a Source in this column to it. 
1. Switch all **QuickConnects** Sources from the Splunk S2S Destination to the Splunk HEC Destination.

### Check Routes and Packs

Once you've switched all QuickConnects references to Splunk HEC:

1. Navigate to **Processing**, **Packs** and **Routing**, **Data Routes** and
   look for references to the Splunk S2S Destinations:

   - On the **Data Routes** page.
   - On the **Routes** page for the Pack.
1. Change the Splunk S2S Destinations you find to the Splunk HEC Destination.
1. Once you're finished, **Commit & Deploy** the changes. In standalone instances,
   the changes take place as soon as you save them.

>We recommended that you make these changes incrementally and validate
>each change inside of your Splunk instance for any data irregularities
>before you move on.
{.box .info}

Now, you've successfully switched all Splunk S2S Destinations in Cribl Stream to
the Splunk HEC Destination.