# Client Certificate Authentication for Windows Event Forwarder


This document covers the client certificate authentication setup steps you need to take before configuring a [Windows Event Forwarder Source](sources-wef).

### 1. Set Up WEF

Follow [this Microsoft guide](https://docs.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription#setting-up-a-source-initiated-subscription-where-the-event-sources-are-not-in-the-same-domain-as-the-event-collector-computer) for setting up Windows Event Forwarding (WEF) in a traditional Windows environment. Generally, you can follow the guide’s “non-domain” section to correctly configure the endpoints/senders.

### 2. Configure Certificates {#clients}

After setting up WEF, you need to configure certificates to ensure secure communication. Server certificate configuration varies depending on your deployment type. However, you'll always need to configure a client certificate.

**On-prem deployments**: Add the server certificate and CA chain to the Worker Group, see [importing certificates](securing-import-certs) for details. Add the client certificate to Windows either manually or with [certificate auto-enrollment](https://learn.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/configure-server-certificate-autoenrollment) through Group Policy to automatically configure unique device certificates. For specific configuration options, consult with your Windows administrators.

**Cribl.Cloud deployment**: Upload the CA chain for your client certificate, see [importing certificates](securing-import-certs) for details. The actual certificate/key pair in the chain doesn't matter for this purpose. It's solely to satisfy the technical requirement of uploading a chain. Use Cribl's provided certificate and key:
* `/opt/criblcerts/criblcloud.crt`
* `/opt/criblcerts/criblcloud.key`

### 3. Set Client Certificate Permissions

Set the appropriate permissions for the `NETWORK SERVICE` user to use the private key of the certificate for authentication.

Open the [Certificate Manager tool](https://docs.microsoft.com/en-us/dotnet/framework/tools/certmgr-exe-certificate-manager-tool) (`certlm.msc`).

* Right-click your client certificate and select **All Tasks** > **Manage Private Keys…**.

* Add the `NETWORK SERVICE` user.

### 4. Configure the Subscription Manager and Log Forwarding Group Policies {#subtemplate}

1. On a domain controller, open the Group Policy Management Console (GPMC).

1. Create a new Group Policy Object (GPO) or edit an existing one that is linked to the organizational unit (OU) containing the computers you want to configure.

1. Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components**. Select **Event Forwarding** to open the Local Group Policy Editor.

1. Select **Configure target Subscription Manager** and use one of the following templates depending on your deployment type, substituting the appropriate values for the placeholders.

    **On-Prem**:
    ```shell
    Server=https://<cribl-worker-or-lb>:<wef-source-port>/wsman/SubscriptionManager/WEC,Refresh=<desired refresh interval>,IssuerCA=<CA cert fingerprint>
    ```

    **Cribl.Cloud**:
    ```shell
    https://<group-name>.main.<org-name>.cribl.cloud:<wef-source-port>/wsman/SubscriptionManager/WEC,Refresh=<desired refresh interval>,IssuerCA=<CA cert fingerprint>
    ```

    * The path portion `/wsman/SubscriptionManager/WEC` within the **Subscription Target URL** set using Group Policy Objects (GPOs) is the same as required for a WEC subscription and is strictly case-sensitive. If the capitalization is incorrect, WEF will encounter errors and fail to forward events to Cribl Stream.
    * The `IssuerCA` thumbprint is the SHA1 thumbprint of the issuing certificate, which could be an intermediate certificate. This must match the thumbprint of the first certificate in the certificate chain configured in the WEF Source's CA certificate path or the specified **CA fingerprint override**.

5. When complete, save the policy and apply it to affected endpoints. Link the GPO to the appropriate OU if it is not already linked.

6. To enable the Network Service to read the security logs, you'll need to configure access for the Event Log Service.

    > When configuring a Group Policy Object (GPO) to add NETWORK SERVICE permissions, carefully review existing permissions to prevent unintended removal. Preserve all necessary permissions to avoid application issues.
    >
    {.box .warning}

    1. Open the Local Group Policy Editor tool (`gpedit.msc`).

    2. Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Event Log Service**. Double-click **Security**, then in the **Settings** pane, select **Configure log access**.

    ![Configuring log access](st-wef-cloud05.82749efe08.png)

    3. In the resulting modal, under **Options** > **Log Access**, enter the following Log Access configuration:

    ```shell
    O:BAG:SYD:(A;;0xf0007;;;SY)(A;;0x7;;;BA)(A;;0x1;;;BO)(A;;0x1;;;SO)(A;;0x1;;;S-1-5-32-573)(A;;0x1;;;S-1-5-20)
    ```

    4. Then, reboot your Windows machine to apply this setting. (Source: Microsoft's [Security event log forwarding fails...](https://docs.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/security-event-log-forwarding-fails-error-0x138c-5004#resolution) topic.)

7. Check that events are flowing into Cribl Stream now according to the configured subscriptions. On client machines, run `gpupdate /force` to apply the new policy immediately. If events are not flowing, verify all of the following:
     * The clients can reach the Cribl Stream instance through the network to port `5986` (or other configured port) via TLS/HTTP. If clients are connecting to Cribl Stream via a proxy, you may need to toggle **Show originating IP** on (in the Advanced section of the Source configuration). Ensure that the correct outbound firewall port is opened on the client.
     * The certificate chain is correct
     * The endpoints have a valid CRL encompassing the CA cert
     * The CA cert is a trusted root on the clients 
     * The server and client certs are issued by the same CA. The `CAPI2` Windows event log might reveal any errors here.
     * Check Cribl Stream for any errors, as well as the `EventForwarding-Plugin` and `Windows Remote Management` event logs on the clients.

Now you can provide client certificate authentication to a [Windows Event Forwarder Source](sources-wef).