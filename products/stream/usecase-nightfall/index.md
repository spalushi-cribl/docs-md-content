# Nightfall Integration


> Thanks to Nightfall for contributing this page and the corresponding Nightfall Pack, which you can [install in Cribl Stream](#installing) and customize as described below.
>
{.box .info}

The Nightfall Pack relies on Nightfall's Data Loss Prevention (DLP) engine, which uses machine learning to detect sensitive information in events streamed through Cribl. The Pack enables you to monitor your Pipelines in real time for PII, PHI, PCI, and other sensitive data. Nightfall discovers and automatically redacts the sensitive content before it can reach any Destination. Aside from redaction, you can use the Pack to automatically label problematic event data.

The risk of sensitive data spreading across data pipelines grows as organizations work with data from more sources. Nightfall can help ensure that sensitive data doesn't accumulate in the first place, facilitating compliance with data-security regulations. This is important for Cloud-first organizations that must
scale observability while complying with regional or industry-specific  compliance regimes.

## Configure the Detection Engine {#configuring}

1. [Sign up](https://app.nightfall.ai/sign-up) for a free Nightfall Developer Platform account, if you have not already done so. This entitles you to scan up to 3GB of event data per month for free.
1. Log in to the [Nightfall Dashboard](https://app.nightfall.ai). 
1. Create an [API Key](https://docs.nightfall.ai/docs/creating-an-api-key).
1. Create the [Detection Rules](https://docs.nightfall.ai/docs/creating-detection-rules) and/or [Policy](https://docs.nightfall.ai/docs/creating-policies) that you want Nightfall to scan with.

   **Detection Rules** enable you to specify which Detectors will scan each event. A Detection Rule simply redacts sensitive information from events.
   
   A **Policy** governs alerts, which Nightfall can send via email, Slack, or webhook. (Assuming that you want to send data to a Cribl Stream Destination and/or a SIEM, use a webhook.) A Policy can include up to 20 previously defined Detection Rules. It can also turn redaction off, if desired.

## Install the Nightfall Pack {#installing}

1. Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to **Processing** > **Packs**.
2. Select **Add Pack**, then select **Add from Dispensary** to open the [Packs Dispensary](https://packs.cribl.io/) drawer.
3. Navigate to the **DLP by Nighfall AI** Pack's tile.
  ![Locating the Nightfall Pack](st-nightfall-integ-03a.4207954b05.png)
  {border="true"}
4. Select the Nightfall Pack's tile to display its details page.

  ![The Nightfall Pack details page](st-nightfall-integ-03b.3c71ff8dbb.png)
  {border="true"}
5.Select **Add Pack**. Shortly, you'll see a banner confirming that the Pack is installed.

## Configure the Nightfall Pack {#pack-config}

1. From the **Packs** page, select the new Pack. This opens the Pack's **Routes** tab.
2. In the **Pipeline** column, select `Nightfall`.
3. From the resulting Nightfall Pipeline configuration, expand the **DLP Scanner with Nightfall AI** Function.

  ![Selecting the Nightfall Pipeline](st-nightfall-integ-04-4101.png)
  {border="true"}
4. In the **Nightfall API Key** field, enter the API key you created in the Nightfall Dashboard.
5. Set the **Scan with a Policy or Multiple Detection Rules** drop-down to match your [detection engine](#configuring) configuration.

    Selecting **Policy UUID** exposes a corresponding field to enter the single UUID.

    Selecting **Detection Rule UUID** exposes a **Add Rule**. Click this to enter as many Detection Rule UUIDs as you want to specify.

6. (Optionally:) Label sensitive events. If you toggle **Label Log Lines with nightfall_violations** on, each event where Nightfall has detected sensitive information will have an added `nightfall_violations: true` field.

   

## Advanced Features

The Nightfall Pipeline also includes three limiting Functions, described here. These are disabled by default, but you can enable them as needed. 

### Sampling

To reduce the data usage that's billed against your Nightfall plan, you can enable the [Sampling Function](sampling-function). This way, Nightfall will scan only a subset of each streamed file, instead of every event.

### Log Batching

To reduce the number of requests that Cribl Stream makes to the Nightfall API, you can enable log batching. To do so, enable both the [Aggregations](aggregations-function) and [Unroll](unroll-function) Functions, then configure the Aggregation Function's **Time window**.

The Nightfall DLP plugin will automatically detect the aggregations, and unroll the batches, only after Cribl Stream sends them to the Detection Engine. Then, Nightfall will process each event separately, as it normally does.
