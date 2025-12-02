# Use Datagens


Data generators for testing and troubleshooting

---

Cribl Edge's datagens feature enables you to generate sample data for the purposes of troubleshooting Routes, Pipelines, Functions, and general connectivity.

Several datagen template files ship with the product, out of the box. You can create others from [sample files or live captures](data-preview#add).

![Preview pane – add samples via paste,  attach/upload file, or live capture](cribl-datagens-landing.132e129979.png)
{border="true"}

As outlined in the following tutorial: Once you've created a template, you can configure a Datagen Source to use the template to generate real-time data at a given EPS (events per second) rate.

## Enable a Datagen

To see how datagens work, start by enabling a pair of Cribl Edge's out-of-the-box generators:

Navigate to **Sources** > **Datagens** and select **Add Source**.

After entering a unique  ID and an optional description, select a data generator file (such as `apache_common.log`) and set it at `4` **Events Per Second Per Worker Node**. Select **Add Datagen** to select another data generator file (such as `syslog.log`) and set it at `8` **Events Per Second Per Worker Node**. Select **Save**.

![Selecting datagen files and event rates](cribl-datagen-source-4101.png)
{border="true"}

In the sidebar, select **Monitoring**, select **Data** > **Sources**, and search for `datagen` to confirm that the Source is generating data.

## Create a Datagen Template from a Sample File

To convert a sample into a template:

From the **Data Routes** or **Pipelines** page, select **Import Data** in the right preview pane, and [paste a sample](data-preview#save) like the [AWS VPC Flow logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-records-examples.html) below:

``` {title="Sample VPC Flow Logs"}
2 123456789010 eni-abc123de 172.31.16.139 172.31.16.21 20641 22 6 20 4249 1418530010 1418530070 ACCEPT OK
2 123456789010 eni-abc123de 172.31.9.69 172.31.9.12 49761 3389 6 20 4249 1418530010 1418530070 REJECT OK
2 123456789010 eni-1a2b3c4d - - - - - - - 1431280876 1431280934 - NODATA
2 123456789010 eni-4b118871 - - - - - - - 1431280876 1431280934 - SKIPDATA
2 123456789010 eni-1235b8ca 203.0.113.12 172.31.16.139 0 0 1 4 336 1432917027 1432917142 ACCEPT OK
2 123456789010 eni-1235b8ca 172.31.16.139 203.0.113.12 0 0 1 4 336 1432917094 1432917142 REJECT OK
2 123456789010 eni-f41c42bf 2001:db8:1234:a100:8d6e:3477:df66:f105 2001:db8:1234:a102:3304:8879:34cf:4071 34892 22 6 54 8855 1477913708 1477913820 ACCEPT OK
```

From the **Select Event Breaker** drop-down, select **AWS VPC Flow** to ensure that:

* The pasted text gets broken properly into individual events (notice the Event Breaker on newlines).

* Timestamps are extracted correctly (text highlighted purple below).

Once you've verified these results, select **Create a Datagen File**.

![Creating a datagen template](cribl-datagen-create.2366b2707b.png)
{border="true"}

On the resulting **Create Datagen File** screen:

* Enter a file name, such as `vpc-flow-datagen.log`

* Ensure that the timestamp template format is correct: `${timestamp: %s}` <br/>
  `${timestamp: <format>}` is a template that the datagen engine uses to insert the current time – in each newly generated event – using the given format. In this case, `%s` is the desired `strftime` format for the timestamp (the epoch).

Once you've verified these results, click **Save as Datagen File**.

![Saving a named datagen template](cribl-datagen-create-2.466f9165f1.png)
{border="true"}

To confirm that the datagen file has been created, select the **Datagens** tab in the preview pane.

![Verifying datagen file creation](cribl-datagen-list-2.3.31b44404f7.png)
{border="true"}

Now, to start using your newly created datagen file, go back to **Sources** > **Datagens**. Add it using the drop-down shown below.

![Adding new template file to datagens Source](cribl-datagen-new-4101.png)
{border="true"}

## Modify a Datagen {#modify}

1. In the right Preview pane, select the **Datagens** tab.
2. Hover over the file name you want to modify. This displays an edit (pencil) button to its left.
3. Select that button to open the modal shown below. It provides options to edit the datagen, clone it, delete it, or modify its metadata (**File name**, **Description**, **Expiration time**, and **Tags**).

![Options for modifying a datagen](se-datagens-modal-3.5.2dbdac427b.png)
{border="true"}

4. To make changes to the datagen, select the modal's **Edit Datagen** button. This opens the **Edit Datagen** modal shown below, exposing the raw data that this datagen uses to generate events.
5. Edit the raw data as desired.
6. Select **Update Datagen** to resave the modified datagen, or select **Save as New Datagen** to give the modified version a new name.

![Editing a datagen](se-edit-datagen-3.5.f592f10a02.png)
{border="true"}
