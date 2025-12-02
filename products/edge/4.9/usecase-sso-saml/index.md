# SSO with Okta and SAML (On-Prem)


Cribl Edge supports setting up SSO using SAML to provide user authentication (login/password)
and authorization (by mapping SSO users to Cribl [Roles](authentication#role-mapping)).

This page presents a walkthrough of setting up a SAML SSO, using Okta as the example.

> This page is a guide for configuring SSO for an on-prem installation.
> For Cribl.Cloud, see [SSO with Okta and SAML (Cribl.Cloud)](saml-okta-setup).
{.box .info}

## Create SAML 2.0 App Integration {#create-app-saml-okta}

To create your app integration:

1. In [Okta](https://help.okta.com/en-us/content/topics/apps/apps_app_integration_wizard_saml.htm), navigate to the **Applications** section and select **Create App Integration**. 
2. In **Sign‑in method**, select `SAML 2.0`.
3. Proceed with **Next**.

### General Settings

4. Configure the app integration's **General Settings** with the options below:
   
| Setting | Description |
| ------- | ----------- |
| **App integration name** | Your application name.
| **Logo** | (Optional) Upload the Cribl logo. You can use a logo from the Cribl [Press Kit](https://cribl.io/press-kit/).

### SAML Settings

5. In the **Configure SAML** tab configure the following options:

| Setting | Description |
| ------- | ----------- |
| **Single sign-on URL** | Cribl Edge **Sign‑on callback URL**.
| **Audience URI (SP Entity ID)** | Cribl Edge **Audience (SP entity ID)**.
| **Application username** | A plain username, an email, or a custom username. In the SAML assertion's `subject` statement, this is the value for `NameID`. By default, this value will be the username in Cribl Edge. Alternatively, you can set a custom attribute statement in Okta, then set the **Username field** in Cribl Edge to use that instead.

6. Define custom **Attribute Statements** that Okta will insert in the SAML assertions shared with Cribl Edge. This applies only when creating a custom **Application username**, as described in the previous step.
7. Configure **Group Attribute Statements**. Similar to the previous step, except that here, Cribl Edge supports creating a custom attribute whose value is one or more Okta groups that will populate Cribl Edge's **Group name field**.
8. To check the SAML assertion in XML form, click **Preview the SAML Assertion**.
9. Skip thought the **Feedback** pane and **Save** your application.

## Submit Your App Info to Cribl

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

1. In Cribl Edge, in the sidebar, select **Settings**, then **Global**.
1. In **Access Management**, select **Authentication**.
1. From the **Type** dropdown, choose `SAML 2.0`.
1. In the **Audience (SP entity ID)** field, enter the base URL of your Cribl Edge instance, for example, `https://yourDomain.com:9000`.  Do **not** append a trailing slash. 
   
   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (SP entity ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

1. Return to your Okta environment to the **Sign On** tab and in the right pane, select **View SAML setup instructions**.
   Use the provided fields to fill in the information in Cribl Edge:

| Cribl Edge field                    | Okta field                           |
| ----------------------------------- | ------------------------------------ |
| **Single sign-on (SSO) URL**        | Identity Provider Single Sign-On URL |
| **Single logout (SLO) URL**         | Identity Provider Single Logout URL  |
| **Issuer (IDP entity ID)**          | Identity Provider Issuer             |
| **Response validation certificate** | X.509 Certificate                    |

## Map Okta Groups to Cribl Edge Roles

Mapping groups to Roles is possible only for Cribl Edge deployments that are in Distributed mode, with an Enterprise license.
With a Standard license, all your external users will be imported to Cribl Edge in the `admin` role.

If you are running Cribl Edge in Single-instance mode, you cannot map Okta groups to Cribl Edge Roles, although you can still set up SSO with Okta.

As you think through how best to map your Okta groups to Cribl Edge Roles, keep these principles in mind:
- An Okta group can map to more than one Cribl Edge Role.
- A Cribl Edge Role can map to more than one Okta group. 
- If a user has multiple Roles, Cribl Edge applies the union of the most permissive levels of access.
- Cribl Edge automatically assigns the `default` Role to any user who has no mapped Roles.

For details on mapping your external identity provider's configured groups to corresponding Cribl Edge user access Roles, see [External Groups and Roles](roles#groups).

You can assign a Cribl Edge Role to each Okta group name, and you can specify a `default` Role for users who are not in any groups.

1. In Cribl Edge, in the sidebar, select **Settings**, go to **Access Management** and then select **Authentication**.
1. Scroll down to **Role Mapping**.

   Cribl recommends that you set the `default` Role to `user`, meaning that this Role will be assigned to users who are not in any groups.

1. Add mappings as needed.

   The Okta group names in the left column are case-sensitive, and must match the values returned by Okta (those you saw earlier when configuring Okta and SAML).

![Role mapping section in Cribl Edge with sample mappings.](st-usecase-sso-okta_06.9892fa386f.png)
{border="true" caption="Example Role mapping"}

## Verify that SSO with Okta Is Working {#verify}

1. Log out of Cribl Edge, and verify that Okta is now an option on the login page.
1. Select **Log in with Okta**.
1. You should be redirected to Okta to authenticate yourself.
1. The SAML connect flow should complete the authentication process.

## Get Temporary Access Credentials for AWS S3 Buckets {#temp-access}

You can use your SSO/SAML IDP to issue temporary access credentials so your on-prem Edge Node can access AWS S3 buckets.

Call the `AssumeRoleWithSAML` API endpoint. It will return the STS access, secret, and session tokens. These can be written into the `~/.aws/` credentials file, which Cribl Edge will pick up because it uses the native AWS SDK.

You can set up multiple S3 Sources with different credentials. Cribl Edge relies on the AWS SDK for authentication support, and the SDK evaluates credentials in the following order:

1. Loaded from AWS Identity and Access Management (IAM) roles for Amazon EC2.
1. Loaded from the shared credentials file (~/.aws/credentials).
1. Loaded from environment variables.
1. Loaded from a JSON file on disk.
1. Other credential-provider classes provided by the JavaScript SDK.

To enable the use of multiple Sources, set the S3 Source [**Authentication method** to **Auto**](/stream/sources-s3#authentication).

See the [AWS SDK documentation](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-node.html) and [AWS CLI documentation](https://repost.aws/knowledge-center/aws-cli-call-store-saml-credentials) for further information on setting credentials.

[Snippet not found: content/shared/4.9/snippets/_sso-fallback-on-prem.md]

## SAML/Okta Chiclet Setup (Optional) {#saml-chiclet-okta}

If you want to initiate login from an Okta instance on which you have configured SAML authentication,
an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.
1. Select **Browse App Catalog**.
1. From the resulting catalog, use the search bar to find and select the `Bookmark App` application.
1. From that application's page, select **Add Integration**.
1. On the **General settings** page, enter an **Application label** that will identify this app as supporting Cribl login. (`Cribl` is a good choice, but the label is arbitrary.)
1. In the **URL** field, enter the `<host>:<port>` of your Cribl Leader Node.
1. Confirm with **Done**.
1. Select **Assign** and assign all of the Cribl groups to the application.
1. The `Cribl` chiclet should now be available for all users in the Cribl groups you've assigned.
