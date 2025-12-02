# Common SSO Setup Steps


Whether you're configuring Single Sign-On (SSO) via OIDC or SAML, these initial preparation steps are the same.

## Set Up Fallback Access {#fallback}

On your Cribl.Cloud Organization, ensure that at least one Owner creates a local account, using an email domain that's separate from the corporate domain on which you're configuring SSO. This will ensure backup access if SSO configuration breaks.

## Configure Groups {#configuring-groups}

In your IDP, configure user groups that match Cribl.Cloud [Roles](deploy-cloud#member-roles). Using Okta as an example, you'd map Okta groups (right side) to Cribl.Cloud Roles (left side) as follows. 

You can use either the open or the closed format in your IDP (right side). You can also add prefixes or suffixes, as outlined below in [Group Configuration Options](#fixes).

| Cribl.Cloud Role | IDP Group Name                                               |
|------------------|--------------------------------------------------------------|
| Owner            | Cribl Organization Owner /or/ CriblOrganizationOwner         |
| Admin            | Cribl Organization Admin /or/ CriblOrganizationAdmin         |
| Editor           | Cribl Organization Editor /or/ CriblOrganizationEditor       |
| Read-Only        | Cribl Organization Read Only /or/ Cribl OrganizationReadOnly |

### Group Configuration Best Practices

* A Cribl.Cloud Organization's Owner Role can be shared and transferred among multiple users. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios.
* Those users should be in both the Owner and Admin groups in your IDP.   
(This enables them to acquire all needed permissions across Cribl's two corresponding Roles.)
* Aside from dual-assigning the Owners, you should assign every other user only one group in your IDP. (Cribl's Admin and Editor Roles include all the permissions of the Roles below them.)

### Group Configuration Options {#fixes}

You can freely add prefixes or suffixes to Group Names that follow the above examples' formats. Cribl will ignore these additions when mapping IDP groups to Cribl Roles. Examples:
- `SOME-LABEL-12345-CriblOrganizationOwner`
- `CriblOrganizationEditor-420`

Here's an example of how groups configuration (at an early stage) might look in Okta's UI:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.af6acd976f.png)
{border="true"}