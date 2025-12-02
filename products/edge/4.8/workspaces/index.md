# Workspaces


Workspaces offer a multi-tenancy capability, enabling you to create multiple isolated instances in your Cribl.Cloud Organization.
This lets you strengthen your security, and fulfill compliance and isolation requirements.

Each Workspace offers a dedicated virtual private cloud (VPC) that acts as a separate environment within your Organization.
Workspaces do not share product configurations, resources, or data flows, and each has a separate Leader Node.
They do share a layer of access control ([Organization-level Permissions](members#permissions-org) and [SSO configuration](cloud-sso)),
as well as API credentials, licenses, and billing.

You can manage all your Workspaces in one interface that contains access controls
for granting [Members and Teams](members-and-teams) access to individual Workspaces.

> Workspaces require a Cribl.Cloud [Enterprise plan](cloud-enterprise).
{.box .cloud}

An example use case scenario for creating multiple Workspaces
is setting up separate environments for different business units in an enterprise.
Data management, security, development, and any other units can have their own federated Workspaces,
with completely separate members and permissions lists.
This way, they can work in isolation without risk of interference and with increased focus.

## Opt In to Workspaces

Access to configuring multiple Workspaces is available once you opt in to a new Cribl.Cloud UI:

1. On the top navigation bar, click **Try the New Cribl.Cloud**.
1. In the modal, confirm with **Get Started**.
   You will move to a new user interface for managing Cribl.Cloud, including Workspaces.

If you want to return to the old UI:

1. Open your user menu at the right side of the top navigation menu.
1. Disable **Make the new experience the default**.
1. Click **Go Back to Classic**.

> Only users with the **Organization Owner** Member permission
> can opt in to the new user interface.
{.box .info}

## Limitations

You can [create](workspaces-configuring#add-new-workspace) up to 5 Workspaces per Organization.

If you need more Workspaces in your Organization (up to 50),
contact Cribl Support to discuss extension options.

> Workspaces require a Cribl.Cloud [Enterprise plan](cloud-enterprise).
{.box .cloud}
