# Connect Cribl Search to Google Cloud Platform API


Configure Cribl Search to query a Google Cloud Platform API endpoint.

---

[Google Cloud Platform](https://cloud.google.com/) provides a suite of computing services including data management, web
and video delivery, AI and machine learning tools.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the [instances](https://cloud.google.com/compute/docs/reference/rest/v1/instances),
[disks](https://cloud.google.com/compute/docs/reference/rest/v1/disks),
[security policies](https://cloud.google.com/compute/docs/reference/rest/v1/securityPolicies), and [locations](https://cloud.google.com/functions/docs/reference/rest/v1/projects.locations) endpoints in your Google Cloud
Platform account.

## Add a Google Cloud Platform API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Google Cloud
Platform API Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Google Cloud Platform API**.
1. Select **Add Configuration** to specify your Google Cloud Platform account. In the **Account Configurations** table,
   enter:
   - **Name**: Google Cloud Platform account name.
   - **Service Account Credentials**: The contents of your
     [Google Cloud service account](https://cloud.google.com/iam/docs/service-account-creds#key-types) (JSON keys) file.
     You can also drag and drop a file into the field, or upload a file by clicking the upload button.
1. Select **Save** when finished.

## Add a Google Cloud Platform API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Google Cloud Platform Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. For details on the endpoints, see the
   [Compute Engine API](https://cloud.google.com/compute/docs/reference/rest/v1) reference docs. Your options are:
   - `v1.instances`
   - `v1.disks`
   - `v1.securityPolicies`
   - `v1.projects.locations.functions`
   - `v2.projects.locations.functions`
   1. **Region**: The region of your Google Cloud Platform account, provided for certain endpoints, indicates its
      location.
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Google Cloud Platform

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

