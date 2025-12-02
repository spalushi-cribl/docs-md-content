# Access Management


Cribl Edge provides a range of access-management features for users with different security requirements. 

## Where Can I Find Access Control Details? {#details}

See the following topics, according to your needs:

- [Authentication](authentication): Authenticating users via local basic auth or external options (SSO, Splunk, LDAP).
- [Members and Permissions](members): Available in Cribl Edge 4.2 and later. Fine-grained access control configurable at separate levels (Organization, product, Fleet, and lower-level resources like Cribl Stream Projects and Cribl Search Datasets).
- [Local Users](local-users): Cribl Edge's original Role-based model for creating users, and for managing their access across a Cribl deployment.
- [Roles](roles): Cribl Edge's original RBAC model for managing Roles and Policies, and for assigning them to users.

## Prerequisites (Restrictions on Restrictions) {#prerequisites}

Permission- and Role-based access control can be enabled only on distributed deployments ([Stream](/stream/deploy-distributed), [Edge](/edge/setting-up-leader-and-edge-nodes)) with an [Enterprise license](licensing). With other license types and/or single-instance deployments ([Stream](/stream/deploy-single-instance), [Edge](/edge/deploy-single-instance)), note that **all users will have full administrative privileges.**



## Which Access Method Should I Use? {#restrictions}

Cribl currently supports both the new Members/Permissions and the legacy Users/Roles models, and these models are cross-compatible for many use cases. However, certain purposes require you to choose a specific model:

- **Cribl.Cloud** now relies only on Members/Permissions. See Cribl.Cloud Organization-level Permissions starting at [Inviting Members](cloud-managing#inviting-members), and product- and lower-level Permissions starting at [Product‑Level Permissions](members#permissions-product).

  > Cribl.Cloud's [Organization-level](cloud-managing#member-roles) Permissions include an Owner superuser. This option currently has no counterpart at the [on-prem](members#permissions-org) (customer-managed) Organization level.
  {.box .cloud}

- **Stream Projects** and Subscriptions, in Cribl Stream 4.2 and later, rely only on Members/Permissions. See [Project‑Level Permissions](/stream/sharing-projects#permissions-list).

- **GitOps** [integration](/stream/gitops) authorization requires the legacy `gitops` [Role](roles#general-roles). This legacy Role currently has no counterpart Permission.

- **Collectors**: The `collect_all` [Role](roles#group-level-roles) specifically enables creating, configuring, and running [Collection jobs](/stream/collectors) on all Stream Worker Groups. This legacy Role currently has no counterpart Permission.

- **Notifications**: The `notification_admin` [Role](roles#general-roles) specifically enables creating and receiving all Notifications. This legacy Role currently has no counterpart Permission.

- **Sources**, **Destinations**, **Pipelines**, and **Routes** are examples of other lower-level resources (below the product level) that can be shared with Local Users only by configuring custom access in legacy `policies.yml` [configuration files](policiesyml).

  > Customizing these files is currently supported only with on-prem (customer-managed) deployments, not on Cribl.Cloud.
  {.box .cloud}

- **Search granular resources** (Datasets, Dataset Providers, and search results) can be shared via Members/Permissions. For details, see the Search [Sharing](/search/sharing) topic.
