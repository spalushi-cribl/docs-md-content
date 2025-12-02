# Local Users


This page covers how to create and manage Cribl Edge users, including their credentials and (where enabled) their access roles. These options apply if you're using the **Local** Authentication type, which is detailed [here](authentication).

> Local Users are only available in on-prem deployments.
> In Cribl.Cloud, use [Members and Permissions](members) for access control.
{.box .cloud}

## Create Local User {#create-local-user}

To create a new Local User:



1. In the sidebar, select **Settings**, then **Global**.
1. Under **Access Management**, select **Local Users**.
   The **Local Users** page will initially show only the default `admin` user. You are operating as this user.

   ![Managing users](users-manage-4.9.png)
   {border="true"}

1. Select **Add User**.
1. Provide the new User with basic information and set their **Password**. Usernames and passwords are case-sensitive.
1. To prevent the user from currently logging in, toggle **Enabled** to **No**.

## Manage Local User Information

As a logged-in Local User, you can change your own information
by opening the **Account** menu in the top right corner and selecting **My profile**.
Here, you can modify your first and last name, and change the account password.

Only an Admin can change the email assigned to a Local User.

## Password Rules

All passwords must:

- Contain eight or more characters.
- Use characters from three or more of the following categories:

    - Lowercase letters.
    - Uppercase letters located after the first character in the password.
    - Digits located before the last character in the password.
    - Non-alphanumeric ASCII characters such as `#`, `!`, or `?`.
    - Non-ASCII characters such as `ñ`, `€`, or emoji.

As of Cribl Edge version 4.5.0, these rules apply for all passwords, whether or not Cribl Edge is running in FIPS mode, with one exception:
* For Cribl Edge not running in FIPS mode, local users whose passwords existed before Cribl Edge 4.4.4 was released, can continue to use their passwords, even if those passwords do not satisfy the rules. 
* When these users change to a new password, the new password must satisfy the rules. 
* New users must create passwords that satisfy the rules.

> If you get locked out of your account, you need to [reset](authentication#manual-pwd) your password manually.
> 
{.box .info}

## Adding Roles

If you've enabled role-based access control you can use the modal's bottom **Roles** section to assign access Roles to this new or existing user.

> For details, see [Roles](roles#roles). Role-based access control can be enabled only on distributed deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with [certain plan/license tiers](https://cribl.io/pricing/). With other license types and/or single-instance deployments ([Edge](/edge/deploy-single-instance), [Stream](/stream/deploy-single-instance)), all users will have full administrative privileges.
>
{.box .info}

Select **Add Role** to assign each desired role to this user. The options on the **Roles** drop-down reflect the Roles you've configured at **Settings** > **Global** > **Access Management** > **Roles**.

Note that when you assign multiple Roles to a user, the Roles' permissions are additive: This user is granted a superset of the highest permissions contained in all the assigned Roles.

By default, Cribl Edge will log out a user upon a change in their assigned Roles. You can defeat this behavior at **Settings** > **Global** > **General Settings > API Server Settings > Advanced > Logout on roles change.**
