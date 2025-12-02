# Local Users


This page covers how to create and manage Cribl Edge users, including their credentials and (where enabled) their access roles. These options apply if you're using the **Local** Authentication type, which is detailed [here](authentication).

## Creating and Managing Local Users

On the Leader Node, you manage users by selecting **Settings** > **Global Settings** > **Access Management** > **Local Users**.  In single-instance deployments ([Stream](/stream/deploy-single-instance), [Edge](/edge/deploy-single-instance)), you select **Settings** > **Access Management** > **Local Users**. 

The resulting **Local Users** page will initially show only the default `admin` user. You are operating as this user.

![Managing users](users-manage.6e262487dc.png)

To create a new Cribl Edge user, click **New User**. To edit an existing user, click anywhere on its row. With either selection, you will see the modal shown below.

The first few fields are self-explanatory: they establish the user's credentials. Usernames and passwords are case-sensitive.

If you choose to establish or maintain a user's credentials on Cribl Edge, but prevent them from currently logging in, you can toggle the **Enabled** slider to `No`.

![Entering and saving a user's credentials](users-add-edit.2dc687c8c7.png)

Logged-in users can change their own Cribl Edge password from the **Local Users** page, by clicking on their own row to open a **Local Users** modal that manages their identity.

![Self-serve password changes](user-settings-pwd-change.18e6e14818.png)
{scale="50%"}

## Adding Roles

If you've enabled role-based access control you can use the modal's bottom **Roles** section to assign access Roles to this new or existing user.

> For details, see [Roles](roles#roles). Role-based access control can be enabled only on distributed deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with an Enterprise license. With other license types and/or single-instance deployments ([Edge](/edge/deploy-single-instance), [Stream](/stream/deploy-single-instance)), all users will have full administrative privileges.
>
{.box .info}

Click **Add Role** to assign each desired role to this user. The options on the **Roles** drop-down reflect the Roles you've configured at **Settings** > **Global Settings** > **Access Management** > **Roles**.

Note that when you assign multiple Roles to a user, the Roles' permissions are additive: This user is granted a superset of the highest permissions contained in all the assigned Roles.

When you've configured (or reconfigured) this user as desired, click **Save**.

By default, Cribl Edge will log out a user upon a change in their assigned Roles. You can defeat this behavior at **Settings** > **Global Settings** > **General Settings > API Server Settings > Advanced > Logout on roles change.**
