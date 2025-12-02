# Kerberos Authentication for Windows Event Forwarder


This document covers the Kerberos authentication setup steps you need to take before configuring a [Windows Event Forwarder Source](sources-wef).

> Kerberos authentication is only supported on x86_64 Linux systems.
>
{.box .info}

### Initial Assumptions

To ensure a smooth configuration process, review the following prerequisites:

- The clients that will be sending events are in a Windows domain. In this example, the domain is `contoso.com`, the NETBIOS name is `CONTOSO`, and the domain controller is `DC01.contoso.com`.

- Kerberos KDC is already configured and working on the domain, and WEC with Kerberos
authentication already works on the domain.

- You have an x86_64 Worker Node to which you have shell access. This guide assumes a recent Ubuntu distribution.

- The fully qualified domain name (FQDN) of this example Worker Node is `worker1.contoso.com`.

- You are using a Kerberos credential cache of the `DIR` or `FILE` type.

### 1. Initial Setup for the Worker Node {#kerberos-enviro-prep}

To set up the Worker Node:

1. Ensure that the Worker Node can resolve the domain controller's FQDN. You can test it with:  
`ping DC01.contoso.com`.

1. Ensure that the Worker Node's `hostname` is set to the FQDN. If it's not, you can set it
with `hostnamectl set-hostname worker1.contoso.com`.

1. Ensure the Worker Node has its clock synchronized with the domain controller.

1. Install the `krb5-user` package and configure Kerberos to correctly identify your
domain. Here's an example configuration (`/etc/krb5.conf`).

  ```
  [libdefaults]
  default_realm = CONTOSO.COM

  [realms]
    CONTOSO.COM = {
      kdc = DC01.contoso.com
      admin_server = DC01.contoso.com
    }

  [domain_realm]
    .contoso.com = CONTOSO.COM
    contoso.com = CONTOSO.COM
  ```

### 2. Setup for the Domain Controller {#domain-controller-setup}

Configure the domain controller to support Kerberos authentication for your Worker Node. This involves setting up DNS records and creating a domain user with the necessary permissions and encryption settings. Proper configuration ensures that the Worker Node can securely authenticate and communicate within your network.

To set up the domain controller:

1. Set up forward and reverse DNS for the Worker Node's FQDN (that is, both A and PTR records).

1. Both `nslookup worker1.contoso.com` and `nslookup <worker's IP>` should work.

1. Create a domain user for the Worker Node. This guide assumes the user is called
`svc-stream-worker1`.

1. Set the user's password to never expire.

1. Under the user's **Properties** > **Account** tab, set both `This account supports Kerberos AES 128 bit encryption` and `This account supports Kerberos AES 256 bit encryption`.

1. Create a keytab file for the Worker Node. This file will contain the necessary credentials for the Worker Node to authenticate with the domain controller. See the [Keytab Export](#keytab-export) section below for instructions on how to generate the keytab file.

#### Keytab Export {#keytab-export}

This section guides you through exporting a keytab file, which is essential for Kerberos authentication. The keytab file contains the necessary credentials for the Worker Node to authenticate with the domain controller. You will generate this file using the `ktpass` command and then transfer it to the Worker Node. This step is crucial for enabling secure, authenticated communication between your Worker Node and the domain controller.

To export the keytab:

1. Export a keytab file for this user, which will also create the appropriate service principal name (SPN). The keytab will be exported to the root of the user profile running the command.

1. Open an elevated PowerShell prompt on the domain controller.

1. Run the following ktpass command to generate the keytab file:
  `ktpass /princ http/worker1.contoso.com@CONTOSO.COM /pass <password> /mapuser CONTOSO\svc-stream-worker1 /crypto AES256-SHA1 /ptype KRB5_NT_PRINCIPAL /out worker1.contoso.com.keytab`
   * `ktpass`: This is the command-line utility used to create keytab files for Kerberos authentication.
   * `/princ http/worker1.contoso.com@CONTOSO.COM`: Specifies the principal name. In this case, it is `http/worker1.contoso.com@CONTOSO.COM`. The principal name is a unique identifier for the service.
   * `/pass <password>`: Specifies the password for the user account. Replace `<password>` with the actual password for the `svc-stream-worker1` user.
   * `/mapuser CONTOSO\svc-stream-worker1`: Maps the principal name to the specified domain user account. Here, `CONTOSO\svc-stream-worker1` is the domain user created for the Worker Node.
   * `/crypto AES256-SHA1`: Specifies the encryption type to be used. `AES256-SHA1` is a strong encryption algorithm.
   * `/ptype KRB5_NT_PRINCIPAL`: Specifies the principal type. `KRB5_NT_PRINCIPAL` indicates that the principal is a Kerberos principal.
   * `/out worker1.contoso.com.keytab`: Specifies the output file name for the keytab. The keytab file will be saved as `worker1.contoso.com.keytab`.

1. Note the SPN created here (in this example,
`http/worker1.contoso.com@CONTOSO.COM`) as you will need it later. This SPN is case-sensitive.

1. Copy the keytab file to the Cribl Stream Worker Node. Carefully note the file's location on the Worker Node, because you will need it later. The keytab command and output will resemble this example:

```
PS C:\Users\Administrator> ktpass /princ http/wefkerb.weftest.local@WEFTEST.LOCAL /pass MyPassword1! /mapuser weftest\cribl /crypto AES256-SHA1 /ptype KRB5_NT_PRINCIPAL /out wefkerb.weftest.local.keytab
Targeting domain controller: SUP-DC01.weftest.local
Using legacy password setting method

Successfully mapped http/wefkerb.weftest.local@WEFTEST.LOCAL to cribl.
Key created.

Output keytab to wefkerb.weftest.local.keytab:
Keytab version: 0x502
keysize 110 http/wefkerb.weftest.local@WEFTEST.LOCAL ptype 1 (KRB5_NT_PRINCIPAL) vno 4 etype 0x12 (AES256-SHA1) keylength 32 (0xa46eb723b85fbaff07c46938ea91406f32e1e4bf627ab7dce1c0ef49dffe7a9d)
```
> If you rotate passwords on this user service account, you'll need to export a new keytab file and merge or replace the keytab on each Worker Node. Microsoft describes a [general method](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-ad-auth-rotate-keytabs?view=sql-server-ver16) for this task. The Microsoft method is for a different service, but it explains the general principles. You could also use a tool like [`msktutil`](https://manpages.ubuntu.com/manpages/bionic/man1/msktutil.1.html).
>
{.box .info}

### 3. Setup for the Windows Event Forwarding Target

To set up the Windows event forwarding target, you'll need to create group policy objects (GPOs) for the [subscription target](#gpo-subscription-target), the [WinRM service](#gpo-WinRM-service), and the [NETWORK SERVICE](#gpo-network-service).

#### Create a GPO for the Subscription Target {#gpo-subscription-target}

Create a Group Policy Object (GPO) to configure the subscription target for Windows Event Forwarding (WEF). This GPO will define the target server and the refresh interval for event subscriptions.

1. Access the UI **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Event Forwarding** > **Configure target Subscription Manager**.

1. Set to `Enabled`. 

1. Go to **Subscription Managers** > **Show**.

1. Add `Server=http://worker1.contoso.com:5985/wsman/SubscriptionManager/WEC,Refresh=60` to the list.

#### Create a GPO for the WinRM Service {#gpo-WinRM-service}

Create a Group Policy Object (GPO) to ensure that the Windows Remote Management (WinRM) service is automatically started on client machines. This is a crucial step for enabling remote management and communication between the clients and the Worker Node. By setting the WinRM service to start automatically, you ensure that the necessary remote management capabilities are always available.

1. Access the UI **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **System Services** > **Windows Remote Management (WS‑Management)**.

1. Set **Startup type** to `Automatic`.

#### Create a GPO for the NETWORK SERVICE {#gpo-network-service}

Create a Group Policy Object (GPO) to grant the NETWORK SERVICE account the necessary permissions to read event logs. This is particularly important if you plan to subscribe to the Security event log. Properly configuring these permissions ensures that the `NETWORK SERVICE` account can access and forward the required event logs to the Worker Node.

1. Access the UI **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Restricted Groups**.

1. In the right pane, right-click and select **Add Group**.

1. Enter `Event Log Readers` as the group name.

1. Under `Members of this group`, click **Add**.

1. Enter `NETWORK SERVICE` as the member name.

> For the change to take effect, you'll have to restart computers after you apply this GPO.
>
{.box .info}

Now you can provide Kerberos authentication to a [Windows Event Forwarder Source](sources-wef).