# Access Management


Cribl Stream provides a range of access-management features for users with different security requirements. 
Acccess management serves these common enterprise goals:

- **Security**: Limit the blast radius of inadvertent or intentional errors, by restricting each user's actions to their needed scope within the application.
- **Accountability**: Ensure compliance, by restricting read and write access to sensitive data.
- **Operational efficiency**: Match enterprise workflows, by delegating access over subsets of objects/resources to appropriate users and teams.

## Where Can I Find Access Control Details? {#details}

See the following topics, according to your needs:

- [Authentication](authentication): Authenticating users via local basic auth or external options (SSO, Splunk, LDAP).
- [Members and Permissions](members): Fine-grained access control configurable at separate levels (Organization, product, Worker Group, and lower-level resources like Cribl Stream Projects and Cribl Search Datasets).
- [Teams](teams): Groups of Members who share the same Permissions.
- [Local Users](local-users): Cribl Stream's original Role-based model for creating users, and for managing their access across a Cribl deployment.
- [Roles](roles): Cribl Stream's original RBAC model for managing Roles and Policies, and for assigning them to users.

## Which Access Method Should I Use? {#restrictions}

Cribl supports both the Members/Permissions and the legacy Users/Roles models, and these models are cross-compatible for many use cases. However, certain purposes require you to choose a specific model:

- **Cribl.Cloud** relies only on Members/Permissions. See Cribl.Cloud Organization-level Permissions starting at [Inviting Members](cloud-managing#inviting-members), and product- and lower-level Permissions starting at [Product‑Level Permissions](members#permissions-product).

  > Cribl.Cloud's [Organization-level](cloud-managing#member-roles) Permissions include an Owner superuser. This option has no counterpart at the [on-prem](members#permissions-org) (customer-managed) Organization level.
  {.box .cloud}

- **Stream Projects** and Subscriptions rely only on Members/Permissions. See [Project‑Level Permissions](/stream/sharing-projects#permissions-list).

- **GitOps** [integration](/stream/gitops) authorization requires the legacy `gitops` [Role](roles#general-roles). This legacy Role has no counterpart Permission.

- **Collectors**: The `collect_all` [Role](roles#group-level-roles) specifically enables creating, configuring, and running [Collection jobs](/stream/collectors) on all Stream Worker Groups. This legacy Role has no counterpart Permission.

- **Notifications**: The `notification_admin` [Role](roles#general-roles) specifically enables creating and receiving all Notifications. This legacy Role has no counterpart Permission.

- **Search granular resources** (Datasets, Dataset Providers, and search results) can be shared via Members/Permissions. For details, see the Search [Sharing](/search/sharing) topic.
