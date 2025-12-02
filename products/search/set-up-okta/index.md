# Connect Cribl Search to Okta API


Configure Cribl Search to query an Okta API endpoint.

---

[Okta](https://www.okta.com/) is a cloud-based identity solution that links your apps, logins, and devices.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the [users](https://developer.okta.com/docs/reference/api/users/),
[apps](https://developer.okta.com/docs/reference/api/apps/),
[devices](https://developer.okta.com/docs/reference/api/devices/),
[groups](https://developer.okta.com/docs/reference/api/groups/), and
[logs](https://developer.okta.com/docs/reference/api/system-log/) endpoints in your Okta account.

## Add an Okta API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Okta API
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Okta API**.
1. Select **Add Configuration** to specify your Okta account. In the **Account Configuration** table, enter:
   - **Account Name**: Okta account name.
   - **Domain Endpoint**: URL for the subdomain of your organization. For example, `subdomain.okta.com`. For details,
     see [Find your Okta domain](https://developer.okta.com/docs/guides/find-your-domain/main/#find-your-okta-domain).
   - **API Token**: API token for authorizing requests. For details, see
     [Create an API token](https://developer.okta.com/docs/guides/create-an-api-token/main/).
1. Select **Save** when finished.

## Add an Okta API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of an [Okta Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. Your options are:
   - users
   - apps
   - devices
   - groups
   - logs
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Okta API

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.
