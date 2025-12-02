# Manage SSO Authentication for Multiple Organizations


Administrators can maintain multiple Cribl.Cloud [Organizations](members#organizations) that use [single sign-on (SSO)](sso-cloud) authentication.

If you manage SSO for multiple Organizations that use the same email domain, users may be directed to the wrong Organization when they log in. That's because Cribl.Cloud's universal login page, `https://manage.cribl.cloud`, directs users to the first Organization that had SSO configured for the email domain that they enter when logging in.

To properly manage user Permissions and direct users to the correct Organization when they authenticate with SSO, expose an Organization-specific login URL as a bookmark app (or "chiclet") in your Identity Provider (IdP).

## Configure SSO Authentication for Multiple Organizations {#configure-multi-org-sso}

To configure SSO authentication for multiple Organizations that share the same email domain:

1. Confirm that you have already [configured an OIDC or SAML application](sso-cloud) for the Organization in the IdP. You must have an individual OIDC or SAML application configured for each of your Organizations in the IdP to configure Organization-specific bookmark apps. 

1. Log in to Cribl.Cloud as an Admin or Owner.

1. On the top bar, select **Products**.

1. In the sidebar, under **Cribl**, select **Organization** to find your Organization ID.

    - The Organization ID is also available from **Organization Details** in the sidebar.

1. In your IdP, [create a bookmark app](#create-idp-bookmark-app) and point it to your Organization-specific login URL. The login URL uses the following format: `https://manage.cribl.cloud/login?connection=<organizationId>` (replace `<organizationId>` with the correct ID for your Organization from the previous step).

1. Name the bookmark app, add a logo if desired, and assign the app to IdP groups and users as needed.

1. If the IdP displays a tile for the existing SAML or OIDC application for Cribl.Cloud, hide the tile and show only the bookmark app so that users always use the Organization-specific login URL. **Users should always log in through the bookmark app instead of using Cribl.Cloud's universal login page.**

> If just-in-time (JIT) provisioning is enabled for an Organization, users are provisioned after they authenticate for the first time using the bookmark app. Cribl adds them as Members of the Organization, and they can access the Organization in Cribl.Cloud.
{.box .info}

### Create Bookmark Apps {#create-idp-bookmark-app}

The process for creating a bookmark app varies for different IdPs. The basic instructions and links listed in this section are intended for general reference. Instructions and links may change without notice. Refer directly to the documentation for your IdP for the most current information.

- **Okta**: Add a Bookmark App, set the **URL** to the Organization-specific login URL, and assign to users/groups. See [Create a Bookmark App integration](https://help.okta.com/en-us/content/topics/apps/apps-create-bookmark.htm) for more information.
- **Microsoft Entra ID**: Create a non-gallery Enterprise app, set **Home page URL** to the Organization-specific login URL, and assign to users/groups.
- **Ping Identity**: **Add Application > Bookmark** (or simple web app), set the **URL** to the Organization-specific login URL, and assign to users/groups. See [Register a custom application or service](https://docs.pingidentity.com/pingoneaic/latest/app-management/register-a-custom-application.html#register_a_custom_application_or_service) and [Bookmark](https://docs.pingidentity.com/pingoneaic/latest/app-management/register-a-custom-application.html#bookmark) for more information.
- **OneLogin**: **Add App > Bookmark/Shortcut (QuickLink)**, set the **URL** to the Organization-specific login URL, make visible in portal if desired, and assign to groups/users.

## Custom IdP Groups and Permission Mapping {#custom-idp-mapping}

Cribl bases user Permissions on the [IdP groups](sso-initial-steps#idp-group-name-mapping) that users belong to. If you do not want users to have the same Permissions in every Organization that uses the same IdP, configure custom IdP groups.

For example, suppose that your company has a regional office in the US and EU. You maintain a separate Cribl Organization for each, but both offices share the same IdP. Your IdP administrators want to manage Owners separately for the two Organizations. In this case, you can configure two custom IdP groups:

- Create `Cribl Organization Owner - US` in the US Organization's bookmark app and add users who should have Owner Permissions in the US Organization only.
- Create `Cribl Organization Owner - EU` in the EU Organization's bookmark app and add users who should have Owner Permissions in the EU Organization only.

Add any users who should have Owner Permissions in **both** the US and EU Organizations to the automatically mapped `Cribl Organization Owner` IdP group. Use caution when adding users to automatically mapped groups to avoid giving them higher-than-intended Permissions.

> When you create a custom IdP group, make sure to preserve the required [core pattern](sso-initial-steps#idp-group-name-mapping) in the group name. Add only prefixes and suffixes. Permission mapping will not work if your group name does not maintain the core pattern.
{.box .warning}

## Login Experience with Multiple Organizations {#multi-org-login-experience}

Users who are Members of multiple Organizations that use SSO authentication exclusively should always log in to each Organization using the correct IdP bookmark app. This streamlines the login process and enhances security by ensuring that users are always directed to the correct Organization after logging in.

If users are Members of one Organization that uses SSO and other Organizations that use local authentication, and they log in through the universal login page, Cribl logs them in to the SSO Organization by default. This is due to Cribl's Home Realm Discovery (HRD). The users will be prompted to re-enter their username and password credentials each time they [switch](cloud-portal#switch-organizations) to an Organization that uses local authentication.

