# Connect Cribl Search to Microsoft Graph API


Configure Cribl Search to query a Microsoft Graph API endpoint.

---

With [Microsoft Graph](https://learn.microsoft.com/en-us/graph/overview), you can access data across all Microsoft 365
services.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to [search](search-overview) your Microsoft Entra ID or Microsoft 365 account(s) supporting the following endpoints:

- [agreementAcceptances](https://learn.microsoft.com/en-us/graph/api/resources/agreementacceptance?view=graph-rest-1.0)
- [agreements](https://learn.microsoft.com/en-us/graph/api/resources/agreement?view=graph-rest-1.0)
- [applicationTemplates](https://learn.microsoft.com/en-us/graph/api/resources/application?view=graph-rest-1.0)
- [applications](https://learn.microsoft.com/en-us/graph/api/application-list?view=graph-rest-1.0&tabs=http)
- [authenticationMethodConfigurations](https://learn.microsoft.com/en-us/graph/api/resources/authenticationmethods-overview?view=graph-rest-1.0)
- [certificateBasedAuthConfiguration](https://learn.microsoft.com/en-us/graph/api/resources/certificatebasedauthconfiguration?view=graph-rest-1.0)
- [chats](https://learn.microsoft.com/en-us/graph/api/chat-list?view=graph-rest-1.0&tabs=http)
- [connections](https://learn.microsoft.com/en-us/graph/api/resources/externalconnectors-externalconnection?view=graph-rest-1.0)
- [contacts](https://learn.microsoft.com/en-us/graph/api/resources/orgcontact?view=graph-rest-1.0)
- [contracts](https://learn.microsoft.com/en-us/graph/api/resources/contract?view=graph-rest-1.0)
- [dataPolicyOperations](https://learn.microsoft.com/en-us/graph/api/resources/datapolicyoperation?view=graph-rest-1.0)
- [devices](https://learn.microsoft.com/en-us/graph/api/user-list-owneddevices?view=graph-rest-1.0&tabs=http)
- [directoryObjects](https://learn.microsoft.com/en-us/graph/api/resources/directoryobject?view=graph-rest-1.0)
- [directoryRoleTemplates](https://learn.microsoft.com/en-us/graph/api/resources/directoryroletemplate?view=graph-rest-1.0)
- [directoryRoles](https://learn.microsoft.com/en-us/graph/api/resources/directoryrole?view=graph-rest-1.0)
- [domainDnsRecords](https://learn.microsoft.com/en-us/graph/api/domain-list-verificationdnsrecords?view=graph-rest-1.0&tabs=http)
- [domains](https://learn.microsoft.com/en-us/graph/api/resources/domain?view=graph-rest-1.0)
- [drives](https://learn.microsoft.com/en-us/graph/api/resources/drive?view=graph-rest-1.0)
- [filterOperators](https://learn.microsoft.com/en-us/graph/filter-query-parameter?context=graph%2Fapi%2F1.0&view=graph-rest-1.0&tabs=http)
- [functions](https://learn.microsoft.com/en-us/graph/api/synchronization-synchronizationschema-functions?view=graph-rest-beta&tabs=http)
- [groupLifecyclePolicies](https://learn.microsoft.com/en-us/graph/api/group-list-grouplifecyclepolicies?view=graph-rest-1.0&tabs=http)
- [groupSettingTemplates](https://learn.microsoft.com/en-us/graph/api/resources/groupsettingtemplate?view=graph-rest-1.0)
- [groupSettings](https://learn.microsoft.com/en-us/graph/api/resources/groupsetting?view=graph-rest-1.0)
- [groups](https://learn.microsoft.com/en-us/graph/api/group-list?view=graph-rest-1.0&tabs=http)
- [identityProviders](https://learn.microsoft.com/en-us/graph/api/identityproviderbase-availableprovidertypes?view=graph-rest-1.0&tabs=http)
- [invitations](https://learn.microsoft.com/en-us/graph/api/resources/invitation?view=graph-rest-1.0)
- [localizations](https://learn.microsoft.com/en-us/graph/api/agreement-list-files?view=graph-rest-1.0&tabs=http)
- [oauth2PermissionGrants](https://learn.microsoft.com/en-us/graph/api/group-list-permissiongrants?view=graph-rest-1.0&tabs=http)
- [organization](https://learn.microsoft.com/en-us/graph/api/resources/intune-onboarding-organization?view=graph-rest-1.0)
- [permissionGrants](https://learn.microsoft.com/en-us/graph/api/group-list-permissiongrants?view=graph-rest-1.0&tabs=http)
- [places](https://learn.microsoft.com/en-us/graph/api/place-list?view=graph-rest-1.0&tabs=http)
- [schemaExtensions](https://learn.microsoft.com/en-us/graph/api/resources/schemaextension?view=graph-rest-1.0)
- [scopedRoleMemberships](https://learn.microsoft.com/en-us/graph/api/user-list-memberof?view=graph-rest-1.0&tabs=http)
- [servicePrincipals](https://learn.microsoft.com/en-us/graph/api/identityprotectionroot-list-riskyserviceprincipals?view=graph-rest-1.0&tabs=http)
- [shares](https://learn.microsoft.com/en-us/graph/api/printer-list-shares?view=graph-rest-1.0&tabs=http)
- [sites](https://learn.microsoft.com/en-us/graph/api/site-list?view=graph-rest-1.0&tabs=http)
- [subscribedSkus](https://learn.microsoft.com/en-us/graph/api/subscribedsku-list?view=graph-rest-1.0&tabs=http)
- [subscriptions](https://learn.microsoft.com/en-us/graph/api/resources/subscription?view=graph-rest-1.0)
- [teamsTemplates](https://learn.microsoft.com/en-us/graph/api/resources/groupsettingtemplate?view=graph-rest-1.0)
- [teams](https://learn.microsoft.com/en-us/graph/api/associatedteaminfo-list?view=graph-rest-1.0&tabs=http)
- [users](https://learn.microsoft.com/en-us/graph/api/printershare-list-allowedusers?view=graph-rest-1.0&tabs=http)

## Add a Microsoft Graph API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Microsoft Graph API Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Microsoft Graph API**.
1. Select **Add Configuration** to specify your Microsoft Graph account(s).
   - **Account Name** is the Microsoft Graph account name.
   - **Tenant ID** is the ID of the
     [Microsoft Entra ID](https://learn.microsoft.com/en-us/graph/api/resources/azure-ad-overview?view=graph-rest-1.0)
     or [Microsoft 365](https://learn.microsoft.com/en-us/graph/use-the-api) to retrieve information from.
   - **Client ID** is the ID of the application that will connect to Microsoft Entra ID or Microsoft 365 account.
   - **Client Secret** is the key that will be used as the secret in the connection to Microsoft Entra ID or Microsoft
     365 account.
1. Select **Save** when finished.

## Add a Microsoft Graph API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Microsoft Graph API Dataset Provider](#provider).
1. Select **Add endpoint** to select the endpoints for your Dataset.
1. **Enabled endpoints**: Select an endpoint from the drop-down menu. For details on the endpoints, see the
   [Microsoft Graph Rest API](https://learn.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0) reference docs.
   Your options are:
   - `invitations`
   - `users`
   - `applicationTemplates`
   - `authenticationMethodConfigurations`
   - `identityProviders`
   - `applications`
   - `certificateBasedAuthConfiguration`
   - `contacts`
   - `contracts`
   - `devices`
   - `directoryObjects`
   - `directoryRoles`
   - `directoryRoleTemplates`
   - `domainDnsRecords`
   - `domains`
   - `groups`
   - `groupSettings`
   - `groupSettingTemplates`
   - `localizations`
   - `oauth2PermissionGrants`
   - `organization`
   - `permissionGrants`
   - `scopedRoleMemberships`
   - `servicePrincipals`
   - `subscribedSkus`
   - `places`
   - `drives`
   - `shares`
   - `sites`
   - `schemaExtensions`
   - `groupLifecyclePolicies`
   - `filterOperators`
   - `functions`
   - `agreementAcceptances`
   - `agreements`
   - `dataPolicyOperations`
   - `subscriptions`
   - `connections`
   - `chats`
   - `teams`
   - `teamsTemplates`
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search Microsoft Graph API

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

