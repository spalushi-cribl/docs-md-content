# Splunk Cloud Self Storage (DDSS) Direct Access


Bring your Splunk data straight into Cribl Lake, bypassing Cribl Stream processing.

---

You can use the Splunk Cloud Self Storage option to archive Splunk Cloud DDSS (Dynamic Data Self Storage) data directly to Cribl Lake. This option bypasses routing or processing it through Cribl Stream. Once your data is integrated with Cribl Lake, Cribl Stream and Cribl Search can access it as a Lake Dataset.

## Requirements for Splunk Cloud Direct Access {#requirements}

To set up this integration, you must create a new DDSS index. 

You can configure one Splunk Cloud Direct Access Source per Workspace.

See other [Limitations](#limitations) later in this topic.

## Configure Splunk Cloud Direct Access {#configure}

To configure this integration, you'll work back and forth between Splunk Cloud and Cribl Lake to share linking identifiers. Keep your eyes on the shore! The following subsections list the major steps, in sequential order:

1. [Create Splunk Index and Storage](#splunk-storage)
1. [Create Cribl Lake Source](#lake-setup)
1. [Connect Splunk Cloud to Your Lake Bucket](#splunk-link)
1. [Provision Your Lake](#provision)
1. [Test and Submit the Connection](#test-submit)
1. [Create Cribl Lake Dataset](#dataset)

### Create Splunk Index and Storage {#splunk-storage}

Start in your Splunk Cloud Platform, where you'll create a new index and corresponding storage details:

1. Select **Settings** > **Indexes** > **New Index**.

1. In the **Dynamic Data Storage** field, select **Self Storage**.

1. Select **Create a self storage location**.

1. On the resulting **Dynamic Data Self Storage** page, give your location a **Title** and (optionally) a **Description**.

1. Below the **Amazon S3 bucket name** field, select the **Splunk Cloud ID**. Copy it to your clipboard for the next section.

### Create Cribl Lake Source {#lake-setup}

In Cribl Lake, create the integration with your DDSS bucket:

1. From the sidebar, select **Direct Access**.

1. Select the **Splunk Cloud Self Storage** Source, and select **Next**.

1. In the **Splunk Cloud ID** field, paste the ID that you copied from Splunk Cloud in the previous section.

1. Optionally, enter a **Description** of this Lake integration's purpose.

1. From the **Region** drop-down, select the AWS Region of your Splunk Cloud Platform.

1. Select **Next**.

1. Copy the Cribl Lake bucket name to your clipboard for the next section.


![Configuring Cribl Lake matching bucket](lake-ddss-2-configure.png)
{scale="100%"}

### Connect Splunk Cloud to Your Lake Bucket {#splunk-link}

Next, return to your Splunk Cloud Platform, to point this environment to the bucket you've created in Cribl Lake:

1. In the **Amazon S3 bucket name** field, paste Cribl Lake bucket name you copied in the previous section.

1. Select **Generate** to generate a bucket policy.

1. Copy the resulting bucket policy to your clipboard for the next section.

### Provision Your Lake {#provision}

Return to Cribl Lake to link your bucket policy and provision the new Lake:

1. In the **AWS S3 bucket policy** pane, paste the Splunk Cloud bucket policy you copied in the previous section.

1. Select **Save**.

1. Wait a few minutes for your new Lake to be provisioned.


![Cribl Lake modal for bucket name and policy](lake-ddss-3-policy-paste.png)
{scale="95%"}

### Test and Submit the Connection {#test-submit}

After provisioning is complete in Cribl Lake, test the connection from Splunk Cloud:

1. In the **Self Storage Locations** dialog, select **Test**.

> Splunk Cloud will write a 0 KB test file to the root of your S3 bucket, to verify that Splunk Cloud Platform has permissions to write to the bucket. 

2. If you see a success message, select **Submit** to finish linking your DDSS location to Cribl Lake.

3. If the test fails, see [Troubleshooting Direct Access](#troubleshooting).

### Create Cribl Lake Dataset {#dataset}

Once you've verified the connection from Splunk Cloud, your final setup step is to create a Lake Dataset to contain your data. Follow the steps in [Create a New Lake Dataset](managing-datasets#create-a-new-lake-dataset), being careful to add these specific requirements to handle DDSS data:

- Give the Dataset an **ID** that exactly matches the Splunk index name you assigned in [Create Splunk Index and Storage](#splunk-storage) earlier on this page. (Otherwise, you will be unable to search or replay your data.)

- Set the [**Storage Location**](byos) to the default `Cribl Lake`, or make no selection on this drop-down.

- Set the **Data format** to `DDSS`.

Save your new Dataset, and your Splunk data should begin flowing into it. That's it!

## Breaking a Direct Access Connection {#break}

After you link Splunk and Cribl resources, their interdependence means that deleting resources on either side has the following consequences:

- In Cribl Lake, deleting a Splunk Cloud Self Storage Source means that you will lose all your Splunk data archived here. Also, Splunk Cloud will no longer be able to write to Cribl Lake through this integration. However, this deletion will not affect your data flow and storage within Splunk Cloud.

- In Splunk Cloud, deleting a Self Storage location that's linked to a Cribl Lake Dataset means that Splunk Cloud will no longer archive data to Cribl Lake. However, this deletion will not affect your data already stored within Cribl Lake.

- However, if you cancel your Splunk Cloud **account**, your Splunk Cloud Self Storage Source in Cribl Lake can still remain intact. This will allow Cribl Stream (Collector) and Cribl Search (queries) to continue reading your Dataset's current contents. However, no new data can be written to the Dataset.

## Limitations of Splunk Cloud Direct Access {#limitations}

A Lake Dataset created and populated via Splunk Cloud Direct Access is read-only to other Cribl products. So, although both Cribl Stream and Cribl Search can process data from the Dataset, neither product can write to it.

A Lake Dataset populated via Direct Access must use the default `Cribl Lake` [Storage Location](byos), not an external bucket.

A Lake Dataset created via Splunk Cloud Direct Access cannot be assigned to a [Lakehouse](lakehouse).

## Troubleshooting Direct Access {#troubleshooting}

If Splunk Cloud fails to write the [test file](#test-submit) to your Lake Dataset, look for the following possible sources of error. Correcting each of these requires re-creating your integration from scratch:

- Regions mismatched between Splunk Cloud and Cribl.Cloud. 

- Bucket name does not start with the Splunk Cloud ID.

- Amazon S3 bucket policy not [pasted in](#splunk-link) accurately.
