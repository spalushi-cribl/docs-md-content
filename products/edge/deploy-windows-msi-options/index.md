# Windows User Permissions and Requirements


Learn what user accounts and administrator privileges you need to run Cribl Edge.

---

This guide explains user context and MSI installer options for installing Cribl Edge on Windows, addressing potential issues related to user privileges and service accounts.

When installing Cribl Edge on Windows, it’s important to distinguish between two user accounts:

*   **Installer User:** This is the user account under which you run the Cribl Edge installer (MSI). This user must have `administrator` privileges to install the software and create the Cribl Edge service.
*   **Service Account:** This is the user account under which the Cribl Edge service itself will run. This account needs appropriate permissions to access necessary APIs and perform its functions, including the ability to execute in-product upgrades. `LocalSystem` is the default account.

## Required Administrator Privileges

Ensure you launch the installer (MSI) from a command prompt that is running with full administrator privileges. Simply being logged in as an `administrator` might not be sufficient; you need to explicitly `Run as administrator`.

### Username Formats
When specifying a username during installation (For example, using the `USERNAME` parameter with the `msiexec` command), use the correct format. However, the default service account is `LOCAL_SYSTEM`, which does *not* require a `username` or `password` to be specified. Specifying a `username` and `password` is only necessary if you intend to run the service under a specific user account (which is generally discouraged). 

#### Find the Correct Username

If you must use a specific user account, you can find the exact name:
1. Open the **Services Management** console (`services.msc`).
1. Locate the Cribl service.
1. Go to the **Log On** tab of the service's properties.

Selecting **OK** in the installer’s user prompt will show the resolved name, making it easier to enter.

The following is an example `msiexec` command using the recommended `LOCAL_SYSTEM` account (note the absence of `USERNAME` and `PASSWORD` parameters):

```shell
msiexec /i cribl-<version>-<build>.msi /qn MODE=mode-managed-edge HOSTNAME=yourLeader FLEET=yourFleet AUTH=YourToken
```

The following example shows how to specify a user:

```shell
msiexec /i cribl-<version>-<build>.msi /qn MODE=mode-managed-edge HOSTNAME=yourLeader FLEET=yourFleet AUTH=YourToken USERNAME="domain\user" PASSWORD="your_user_password"
```

## Service Account Configuration Options

The following options are available for configuring the Cribl Edge service account.

*   **Local Administrator or Administrators Group (Not Recommended):**  You can configure Cribl Edge to run under the local `Administrator` account or a local user who is a member of the local `Administrators` group. In such cases, the user must also be granted the `Log on as a service` privilege.  While possible, running as a specific user is generally discouraged due to the management overhead and potential security implications.

> Storing passwords in configuration files or scripts poses a significant security risk. If these credentials are compromised, the system and potentially other resources accessible by that account could be vulnerable. 
>
{.box .warning}

### Passwordless Options 

Instead of configuring a local `Administrator` account, you can configure Cribl Edge to run as:  

*   **Local System:** Cribl Edge on Windows, by default, runs under the `LocalSystem` account (also referred to as `SYSTEM`). This is a predefined system account with extensive privileges on the local machine. The `LocalSystem` account has the required `Log on as a service` privilege by default. It is a passwordless account, meaning it cannot be logged into interactively, and no password is associated with it. 

*   **Managed Service Accounts (MSA/gMSA):** For enhanced security in enterprise environments, we strongly recommend using Managed Service Accounts (MSA) or Group Managed Service Accounts (gMSA) for the Cribl Edge service. These accounts provide automatic password management and improve security posture. Using a standard user account with a password for a service is discouraged. Refer to Microsoft’s documentation for details: [Group Managed Service Accounts Overview](https://www.google.com/search?q=https://learn.microsoft.com/en-us/windows-server/security/group-managed-service-accounts-overview).


### User Requirements

To successfully run the Edge service under a specific user account or the Managed Service Accounts (MSA) the following requirements must be met.

#### Group Memberships

The user must be a member of the following groups:

*   Event Log Readers
*   Network Configuration Operators
*   Performance Log Users
*   Performance Monitor Users
*   Users (default)

#### User Rights

The user must have the following rights, which can be assigned via Local Security Policy or Group Policy tools:

*   Log on as a service
*   Manage Auditing and Security Log

#### Folder Permissions

The user needs appropriate permissions for the Cribl Volume directory, `C:\ProgramData\Cribl`:

*   **If the folder does not exist:** The user must have Write and Modify permissions for `C:\ProgramData` to create the folder and subdirectories.
*   **If the folder already exists:** Grant the user Full Control permissions for `C:\ProgramData\Cribl`, ensuring permissions propagate to subfolders and files.

The command to Apply Permissions to existing folder (run as Administrator):

```shell
icacls "C:\ProgramData\Cribl" /inheritance:e /grant <User Name>:(OI)(CI)F /t
```

## Troubleshooting

If you encounter issues, check the Windows Event Viewer for error messages related to the installation or the Cribl Edge service. For detailed installer logs, use the `/l*v logfile.txt` option with the `msiexec`. Also, check the Cribl logs themselves for more application-specific information.