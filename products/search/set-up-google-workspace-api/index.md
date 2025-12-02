# Connect Cribl Search to Google Workspace API


Configure Cribl Search to query a Google Workspace API endpoint.

---

A [Google Workspace](https://workspace.google.com) plan offers business-specific email as well as collaboration tools
such as Gmail, Calendar, Meet, Chat, Drive, Docs, Sheets, Slides, Forms, and Sites.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) the activities of the [admin](https://developers.google.com/admin-sdk/reports/v1/guides/manage-audit-admin), [devices](https://developers.google.com/admin-sdk/reports/v1/guides/manage-audit-mobile), [drives](https://developers.google.com/admin-sdk/reports/v1/guides/manage-audit-drive), [logins](https://developers.google.com/admin-sdk/reports/v1/guides/manage-audit-login), and [tokens](https://developers.google.com/admin-sdk/reports/v1/guides/manage-audit-tokens) endpoints on your Google
Workspace account.

## Google Workspace API Authorization {#authorization}

To allow Cribl Search to read your data, do the following on the Google Workspace side:

1. Create or use an existing Google Workspace
   [service account](https://developers.google.com/workspace/guides/create-credentials#create_a_service_account).
1. Configure the service account with
   [domain-wide delegation](https://developers.google.com/workspace/guides/create-credentials#optional_set_up_domain-wide_delegation_for_a_service_account)
   for the following scope:
   ```html
   https://www.googleapis.com/auth/admin.reports.audit.readonly
   ```
   Read more on
   [domain-wide delegation](https://support.google.com/a/answer/162106?hl=en#zippy=%2Cset-up-domain-wide-delegation-for-a-client).
1. Obtain the service account
   [credentials](https://developers.google.com/workspace/guides/create-credentials#create_credentials_for_a_service_account).
   You'll need the API keys in JSON format when creating the [Google Workspace API Dataset Provider](#provider).
1. Create or use an existing Google Workspace [user account](https://support.google.com/a/answer/33310?hl=en). This can
   be a real user or just a service placeholder. You'll need the user account's email address when creating the
   [Google Workspace API Dataset Provider](#provider).
1. Authorize the user account with the [Super Admin](https://support.google.com/a/answer/2405986) role. Due to how the
   Google Workspace APIs work, the user account must have the full Super Admin permissions for the resources that you
   want to search.

## Add a Google Workspace API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains [access credentials](#authorization). Here, you will
add a Google Workspace API Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Google Workspace API**.
1. Select **Add Configuration** to specify your Google Workspace account(s).
   - **Account Name** is the name of your Google Workspace account.
   - **Impersonated Account's Email Address** is the email address of the **user account** that's used for searching the
     APIs. For details, see [Authorization](#authorization).
   - **Service Account Credentials** is the JSON keys of your Google Workspace **service account**. For details, see
     [Authorization](#authorization).
1. Select **Save** when finished.

## Add a Google Workspace API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Google Workspace Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. For details on the endpoints, see the
   [Reports API Overview](https://developers.google.com/admin-sdk/reports/v1/get-start/overview) reference docs. Your
   options are:
   - `admin `
   - `mobile`
   - `devices`
   - `drive`
   - `login`
   - `token`
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Google Workspace API

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

