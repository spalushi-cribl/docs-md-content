# Access Management


Cribl Edge provides a range of access-management features for users with different security requirements. 
Access management serves these common enterprise goals:

- **Security**: Limit the blast radius of inadvertent or intentional errors, by restricting each user's actions to their needed scope within the application.
- **Accountability**: Ensure compliance, by restricting read and write access to sensitive data.
- **Operational efficiency**: Match enterprise workflows, by delegating access over subsets of objects/resources to appropriate users and teams.

## Where Can I Find Access Control Details? {#details}

See the following topics, according to your needs:

- [Authentication](authentication): Authenticating users via local basic auth or external options (SSO, Splunk, LDAP).
- [Members](members): Cribl Suite users given controlled access to the product.
- [Teams](teams): Groups of Members who share the same Permissions.
- [Permissions](permissions): Fine-grained access control configurable at separate levels (Organization, Workspace, product, Fleet, and lower-level resources like Cribl Stream Projects and Cribl Search Datasets).
- [Local Users](local-users): Cribl Edge's original Role-based model for creating users, and for managing their access across a Cribl deployment.
- [Roles](roles): Cribl Edge's original RBAC model for managing Roles and Policies, and for assigning them to users.

## Which Access Method Should I Use? {#restrictions}

Cribl supports both the Members/Permissions and the legacy Users/Roles models, and these models are cross-compatible for many use cases. However, certain purposes require you to choose a specific model:

- **Cribl.Cloud** relies only on Members/Permissions. See Cribl.Cloud Organization-level Permissions starting at [Inviting Members](members#invite-members), and product- and lower-level Permissions starting at [Product‑Level Permissions](permissions#product).

  > Cribl.Cloud's [Organization-level](permissions#organization) Permissions include an Owner superuser.
  > This option has no exact counterpart at the on-prem (customer-managed) Organization level,
  > but you can manage overall access to an on-prem deployment by using [Workspace-level](permissions#workspace) Permissions.
  {.box .cloud}

- **Stream Projects** and Subscriptions rely only on Members/Permissions. See [Project‑Level Permissions](/stream/sharing-projects#permissions-list).

- **GitOps** [integration](/stream/gitops) authorization requires the legacy `gitops` [Role](roles#general-roles). This legacy Role has no counterpart Permission.

- **Collectors**: The `collect_all` [Role](roles#group-level-roles) specifically enables creating, configuring, and running [Collection jobs](/stream/collectors) on all Stream Worker Groups. This legacy Role has no counterpart Permission.

- **Notifications**: The `notification_admin` [Role](roles#general-roles) specifically enables creating and receiving all Notifications. This legacy Role has no counterpart Permission.

- **Search granular resources** (Datasets, Dataset Providers, and search results) can be shared via Members/Permissions. For details, see the Cribl Search [Sharing](/search/sharing) page.
