# Windows Event Forwarder


Cribl Stream supports receiving Windows events from the Windows Event Forwarding mechanism built into modern versions of Microsoft Windows (including Windows 10, Windows Server 2012, and more-recent releases).

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No** | Uses Key-Value Store
>
{.box .info}

This Source supports client certificates (mutual TLS) authentication and Kerberos authentication on Linux.

On Cribl Edge, this Source currently supports mutual TLS only with Windows Edge Nodes.

> If you are running this Source on a Cribl Stream version earlier than v.4.3.0, consider delaying any planned upgrades to v.4.3.x or v.4.4.0 until CRIBL-20444 is fixed.
>
{.box .warning}

## Upstream Prerequisites {#prerequisites}

This page assumes that you:
- Already have Windows Event Forwarding set up.
- Are pointed to one or more Windows Event Collectors (WECs).
- Are using either client certificate authentication over HTTPS or Kerberos authentication over HTTP. 

If you are using client certificate authentication, we also assume that you have – or will generate – a server certificate for this Source, issued by the same Certificate Authority as the client certs. 

> For details, see [Configuring Upstream Clients/Senders](#clients).
> 
> For a complete walk-through of generating certificates, setting permissions and policies, and ingesting Windows Event subscriptions and data through Cribl.Cloud, see our [Configuring WEF for Cribl Stream](/stream/usecase-wef-config) topic.
>
{.box .success}

### Monitoring Numerous EventIDs with WEF

A single XPath query within a Windows Event Forwarding (WEF) subscription is limited to approximately 22 `EventID`s. If your query exceeds this limit, WEF may fail to forward matching events to Cribl Stream.

To monitor a larger number of `EventID`s, the best practice is to use a single subscription containing multiple queries.

You can achieve this by editing the subscription's XML configuration to include several `<Query>` blocks within the `<QueryList>`. Each query can contain its own group of `EventID`s, allowing you to bypass the single-query limit efficiently.

Creating multiple subscriptions is not recommended, as it consumes more resources and goes against Microsoft's performance guidelines.

## Configuring Cribl Stream to Receive Windows Events

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Windows Event Forwarder**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Windows Event Forwarder**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Windows Event Forwarder Source definition. 

**Address**: Enter the hostname/IP on which to listen for Windows events data. (E.g., `localhost` or `0.0.0.0`.)

**Port**: Enter the port number. The default, 5986, is the port used by Windows Event Collector for HTTPS-based subscriptions.

**Authentication method**: Specify the method for accepting incoming client connections – either `Client certificate` or `Kerebros`.

### Client Certification Authentication Settings

**Certificate name**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.  

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`. If multiple certificates are present in a `.pem`, each must directly certify the one preceding it in that file. This should match the order of traversing the chain from the host to the root CA cert.

> The CA certificate that generated the Cribl Stream server certificate must be the same root CA that signed all client certificates that will be forwarding Windows events to this Source. If the issuing CA is an intermediate CA, this CA must also match on the server and all client certs.
>
{.box .info}

**Common name**: Regex matching subject common names in peer certificates allowed to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. E.g., to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

**Verify certificate via OCSP**: If toggled to `Yes`, Cribl Stream will use an OCSP (Online Certificate Status Protocol) service to check that client certificates presented in incoming requests have not been revoked. Exposes this additional toggle:

**Strict validation**: If enabled, Cribl Stream will fail checks on **any** OCSP error. Otherwise, Cribl Stream will fail checks only when a certificate is revoked, and will ignore other errors (such as `OCSP server is unavailable` errors).

### Kerberos Authentication Settings {#kerberos-auth-settings}

To implement Kerberos authentication, you must first [prepare your environment](#kerberos-enviro-prep).

**Service principal name**: Specify the full service principal name, in the form `HTTP/<fully qualified domain name>@REALM`. This is case-sensitive.

**Keytab location**: Specify the path to the keytab file containing the service principal credentials. Cribl Stream will use `/etc/krb5.keytab` if you do not specify a different keytab file.

> Kerberos authentication is disabled for Cribl-managed Cribl.Cloud Workers, but it is enabled for on-prem Worker Groups, whether the Leader is based in Cribl.Cloud or on-prem.
>
{.box .cloud}

### Optional Settings

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Subscriptions {#subscriptions}

Click **Add Subscription** to define a new subscription. At least one subscription is required, and has the following options:

**Name**: A friendly name for the subscription, which is only used to help you identify it.

**Version**: A read-only field which will be populated when the Source is saved, and will be assigned a new value any time new changes are made to the subscription that affect how a client interprets the subscription.

**Format**: You can choose `Raw` to receive only XML data about the subscribed events, or `RenderedText` to additionally include amplifying information generated by the client about the event's contents.

**Heartbeat**: The maximum allowable time, in seconds, before the client will check in with Cribl Stream even if it has no new events to send.

**Batch timeout**: The maximum time, in seconds, that the client should aggregate new events before sending them to Cribl Stream.

> Windows Event Collector defines two delivery modes of "Event Delivery Optimization":
> - The "Minimize Bandwidth" mode corresponds to a **Heartbeat** and **Batch timeout** of 6 hours (21,600 seconds).
> - The "Minimize Latency" mode corresponds to a **Heartbeat** of 1 hour (3,600 seconds) and a **Batch timeout** of 30 seconds. 
>
{.box .info}

**Read existing events**: Control whether historical events should be sent by the client when it first connects. If set to `No`, only events generated by the client after the subscription is received will be forwarded. If set to `Yes`, the behavior depends on the **Use bookmarks** setting:

- If **Use bookmarks** is set to `No`, then each time a client *forcibly* renews its subscription (e.g., after group policy update), all events matching the query will be sent. Restarting Cribl Stream or restarting the client will not cause all events to be re-sent.
- If **Use bookmarks** is set to `Yes`, then when the client receives a subscription for the first time, it will send all historical events matching the query, but on subsequent (even forced) resubscriptions, it will only send events later than the saved bookmark.
- In either case, if a subscription is changed and saved, the saved bookmarks are no longer valid (they are tied to a specific subscription version), and all events matching the updated subscription will be sent.

**Use bookmarks**: If toggled to `Yes`, Cribl Stream will keep track of which events have been received, resuming from that point even if the client forcibly re-subscribes. If set to `No`, a client (re-)subscribing will either send all historical events, or send only events subsequent to the subscription, depending on the **Read existing events** setting.

**Compression**: Sets whether Windows clients compress the events sent to Cribl Stream, using the Streaming Lossless Data Compression (SLDC) algorithm. Defaults to `Yes`.

**Queries**: See the explanation in [Configuring Queries](#queries) below.

**Query builder mode**: See the explanation in [Configuring Queries](#queries) below.

**Targets**: Set the DNS names of the clients that should receive this subscription. Wildcard-matching is supported.

#### Configuring Queries {#queries}

Queries determine which events to send from the clients. **At least one query is required.** The format is derived from the XPath implementation used by Windows Event Collector. You have two **Query builder mode** options: **Simple** or **Raw XML**. 

Select **Raw XML** if you have an existing XPath query. Paste your query into the **XML query** field.

Select **Simple** if you need to manually build queries. Click **Add Query** to define each new query. A query has the following properties:
- **Path**: Set this to the `Path` attribute of a `Select` XPath element. See the example below.
- **Query expression**: Set this to the value inside a `Select` XPath element. See the example below.

With either a **Simple** or **Raw XML** query, you can use the **Targets** field to enter the DNS names of the endpoints that should forward these events. Supports wildcards (e.g., `*.mydomain.com`). The default global wildcard (`*`) enables all endpoints.

> If you have both **Simple** and **Raw XML** queries defined within a given Subscription, the **Query builder mode** button that you select when saving this Source's configuration determines which query/queries Cribl Stream will send. To send both **Simple** and **Raw XML** queries, use the **Add Subscription** button to define multiple subscriptions.
>
{.box .success}

#### Example

When creating a subscription in a Windows Event Collector, you can view the generated XML/XPath query. Consider a subscription that returns all events from the Security log that are of severities Critical, Error, or Warning, and that occurred within the last 24 hours. This subscription would generate XML like this:

```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*[System[(Level=1  or Level=2 or Level=3) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]</Select>
  </Query>
</QueryList>
```

To use this subscription in a Cribl Stream Windows Event Forwarder Source, you would either use the **Raw XML** option and paste the above XML, or use **Simple** mode and set the following query properties:
- **Path**: `Security`
- **Query expression**: `*[System[(Level=1  or Level=2 or Level=3) and TimeCreated[timediff(@SystemTime) &lt;= 86400000]]]`

> You do not need to use the `<Query>` element's `Id` or `Path` attributes anywhere in the Cribl Stream subscription config.
>
{.box .success}

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Stream's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Stream's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Stream applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Stream will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Stream to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> As of Cribl Stream 4.1, Source-side PQ's default **Mode** changed from `Smart` to `Always on`. This option more reliably ensures events' delivery, and the change does not affect existing Sources' configurations. However:
> - If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
> - If you enable `Always on`, this can reduce data throughput. As a trade-off for data durability, you might need to either accept slower throughput, or provision more machines/faster disks.
> - You can optimize Workers' startup connections and CPU load at **Group Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .warning}

### Processing Settings

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Allow MachineID mismatch**: Set this to `Yes` if you do not want to verify that the events sent by a client match the client's certificate Common Name (Subject CN). If set to `No`, events where the `MachineID` (by default the client's machine name, like `CLIENT1.domainName.com`) do not match the CN will be rejected.

This setting applies only to client certificate authentication. If you use Kerberos authentication, this option will not appear and will be treated as a `No` value.

**Enable proxy protocol**: Toggle to `Yes` if the connection is proxied by a device that supports [Proxy Protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2. This setting affects how the Source handles the [`__srcIpPort` field](#src-ip-port).

**Capture request headers**: Toggle this to `Yes` to add request headers to events, in the `__headers` field.

**Max active requests**: Maximum number of active requests allowed for this Source, per Worker Process. Defaults to `256`. Enter `0` for unlimited.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Socket timeout (seconds)**: How long Cribl Stream should wait before assuming that an inactive socket has timed out. The default `0` value means wait forever.

**Keep-alive timeout (seconds)**: After the last response is sent, Cribl Stream how long to wait for additional data before closing the socket connection. Defaults to `5` seconds; minimum is `1` second; maximum is `600` seconds (10 minutes).

The longer the timeout, the more Cribl Stream will reuse connections. The shorter the timeout, the closer Cribl Stream gets to creating a new connection for every request. When request frequency is high, you can use longer timeouts to reduce the number of connections created, which mitigates the associated cost.

**CA fingerprint override**: Use this setting only if the first certificate in the configured CA chain does not match the SHA1 fingerprint that the WEF client expects. If that is the case, enter the expected SHA1 fingerprint.

This setting only applies to client certificate authentication. It is ignored if you use Kerberos authentication.

#### Timeout Considerations for Kerberos Authentication

If you use Kerberos authentication, there are some trade-offs you should consider when you select settings in **Subscriptions** > **Batch timeout** and **Advanced Settings** > **Keep-alive timeout (seconds)**.

Windows expects socket connections to be long-lived for Kerberos authentation so it can avoid the overhead of performing the two-step Kerberos authentication on each new batch delivery.

If the socket timeout has elapsed between event deliveries, Cribl Stream will still handle it gracefully, but a small additional overhead is incurred.

Setting **Keep-alive timeout** >= **Batch timeout** will prevent this additional re-authentication step, but it also means that Cribl Stream will maintain open socket connections for each client for longer, which could cause it to hit the limit set by **Advanced Settings** > **Max active requests**. As a result, Cribl Stream will no longer accept additional connections for a given worker process.

Keeping the **Keep-alive timeout** low (such as the default 5 seconds) will minimize Cribl Stream memory usage (due to long-lived open connections), but it will result in an additional network round-trip on each batch of events delivered.

Note that the **Subscriptions** > **Heartbeat** setting is unaffected by the above because Windows treats socket connections for heartbeat signals as one-time-use and immediately closes them.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__final`
* `__headers` – Added only when **Advanced Settings** > **Capture request headers** is set to `Yes`.
* `__inputId` 
* `__srcIpPort` – See details [below](#src-ip-port).
* `__subscriptionName`
* `__subscriptionVersion`
* `_raw`
* `_time`

### Overriding `__srcIpPort` with Client IP/Port {#src-ip-port}

The `__srcIpPort` field's value contains the IP address and (optionally) port of the WEF client sending data to this Source.

When any proxies (including load balancers) lie between the WEF client and the Source, the last proxy adds an `X‑Forwarded‑For` header whose value is the IP/port of the original client. With multiple proxies, this header's value will be an array, whose first item is the original client IP/port.

If `X‑Forwarded‑For` is present, and **Advanced Settings** > **Enable proxy protocol** is set to `No`, the original client IP/port in this header will override the value of `__srcIpPort`. 

If **Enable proxy protocol** is set to `Yes`, the `X‑Forwarded‑For` header's contents will **not** override the `__srcIpPort` value. (Here, the upstream proxy can convey the client IP/port without using this header.)

## Key-Value Store

The Windows Event Forwarder Source uses key-value store for bookmark information when bookmarks are enabled.
`machineId` is used as keys.

## Configuring Upstream Clients/Senders {#clients}

This section summarizes how to configure Windows endpoints/senders to forward events to this Cribl Stream Source. You can find basic instructions for setting up WEF (in a traditional Windows environment) in [this Microsoft guide](https://docs.microsoft.com/en-us/windows/win32/wec/setting-up-a-source-initiated-subscription#setting-up-a-source-initiated-subscription-where-the-event-sources-are-not-in-the-same-domain-as-the-event-collector-computer). You can generally follow that guide's "non-domain" section to correctly configure the endpoints/senders.

> For a complete walk-through of generating certificates, setting permissions and policies, and ingesting Windows Event subscriptions and data through Cribl.Cloud, see Cribl's [Configuring WEF for Cribl Stream](/stream/usecase-wef-config) topic.
>
{.box .success}

1. Set up Cribl Stream's Windows Event Forwarder Source as outlined above. The CA certificate you use should be the issuing authority both for the server certificate, and for all client certs you plan to have forwarding events to this Source.

2. Ensure that your existing Windows Event Collector is receiving events correctly from whatever endpoints and subscriptions you already have in place.

3. For client certificate authentication only: Find the fingerprint of the CA cert you are using for this Source, via a tool like `certutil` or `certlm` on Windows, or `openssl` on other operating systems. You'll need this in the next step.

4. Edit the group policy for endpoints you want to forward to this Source:

   - Under **Computer Configuration > Administrative Templates > Windows Components > Event Forwarding**, modify the **Configure target Subscription Manager** setting.
  
   - Add a Subscription Manager with a value like: `Server=https://<cribl-instance>:<wef-source-port>/wsman/SubscriptionManager/WEC,Refresh=<desired refresh interval>,IssuerCA=<CA cert fingerprint from above>`
       - For Kerberos authentication, use `http://`, not `https://`. Kerberos provides its own encryption and does not use TLS.
       - For Kerberos authentication, do not enter the `IssuerCA` portion.
  
   - Note that the path portion is the same as required for a WEC subscription, and is not configurable in Cribl Stream.
  
   - Ensure that the protocol is one of the following:

       - `https` for client certificate (mutual TLS) authentication.
       - `http` (for Kerberos authentication).
  
   - When complete, save the policy and apply it to affected endpoints.

> The path component within the **Subscription Target URL** set using Group Policy Objects (GPOs) is strictly case-sensitive. If the capitalization is incorrect, WEF will encounter errors and fail to forward events to Cribl Stream.
> 
> The following format exemplifies the proper capitalization for the Subscription Target URL path:
> `Server=<url>/SubscriptionManager/wsman/WEC,Refresh=<interval>`
> 
> Pay close attention to the capitalization of "SubscriptionManager" within the path. It must be exactly as shown in the correct format (with a capital "M") for WEF to function properly.
>
{.box .warning}

5. Check that events are flowing into Cribl Stream now according to the configured subscriptions. If they are not:

   - Certificate authentication only: Verify that the clients can reach the Cribl Stream instance through the network to port 5986 (or other configured port) via TLS/HTTP. If clients are connecting to Cribl Stream via a proxy, you may need to set **Show originating IP** to `Yes` (in the Advanced section of the Source configuration). Ensure that the correct outbound firewall port is opened on the client. 

   - Certificate authentication only: Verify all of the following: that the certificate chain is correct; that the endpoints have a valid CRL encompassing the CA cert; that the CA cert is a trusted root on the clients; and that the server and client certs are issued by the same CA. The `CAPI2` Windows event log might reveal any errors here.

   - Certificate authentication or Kerberos authentication: Check Cribl Stream for any errors, as well as the `EventForwarding-Plugin` and `Windows Remote Management` event logs on the clients.

## Preparing the Environment for Kerberos Authentication {#kerberos-enviro-prep}

This section documents the setup steps you need to take before configuring a Windows Event Forwarder Source for Kerberos authentication.

### Initial Conditions and Assumptions

We assume that:

- The clients that will be sending events are in a Windows domain. In this example, the domain is `contoso.com`, the NETBIOS name is `CONTOSO`, and the domain controller is `DC01.contoso.com`.

- Kerberos KDC is already configured and working on the domain, and WEC with Kerberos
authentication already works on the domain.

- You have an x86-64 Worker Node to which you have shell access. This guide assumes a recent Ubuntu distribution.

- The fully qualified domain name (FQDN) of this example Worker Node is `worker1.contoso.com`.


### Initial Setup for the Worker Node

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

### Setup for the Domain Controller {#domain-controller-setup}

To set up the domain controller:

1. Set up forward and reverse DNS for the Worker Node's FQDN (i.e., both A and PTR records).

1. Both `nslookup worker1.contoso.com` and `nslookup <worker's IP>` should work.

1. Create a domain user for the Worker Node. This guide assumes the user is called
`svc-stream-worker1`.

1. Set the user's password to never expire.

1. Under the user's **Properties** > **Account** tab, set both `This account supports Kerberos AES 128 bit encryption` and `This account supports Kerberos AES 256 bit encryption`.

#### Keytab Export {#keytab-export}

To export the keytab:

1. Export a keytab file for this user, which will also create the appropriate service principal name (SPN). The keytab will be exported to the root of the user profile running the command.

1. Open an elevated PowerShell prompt on the domain controller.

1. Run `ktpass /princ http/worker1.contoso.com@CONTOSO.COM /pass <password> /mapuser CONTOSO\svc-stream-worker1 /crypto AES256-SHA1 /ptype KRB5_NT_PRINCIPAL /out worker1.contoso.com.keytab`.

1. Note the SPN created here (in this example,
`http/worker1.contoso.com@CONTOSO.COM`) as you will need it later. This SPN is case-sensitive.

1. Copy the keytab file to the Cribl Stream Worker Node. Carefully note the file's location on the Worker Node, because you will need it later.

> If you rotate passwords on this user service account, you'll need to export a new keytab file and merge or replace the keytab on each Worker Node. Microsoft describes a [general method](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-ad-auth-rotate-keytabs?view=sql-server-ver16) for this task. The Microsoft method is for a different service, but it explains the general principles. You could also use a tool like [`msktutil`](https://manpages.ubuntu.com/manpages/bionic/man1/msktutil.1.html).
>
{.box .info}

### Setup for the Windows Event Forwarder Source

The general setup for creating subscriptions is the same as documented in [Subscriptions](#subscriptions).

When you create the Source, select the `Kerberos` authentication option. Specify the SPN and
keytab file location that you set above in [Keytab Export](#keytab-export). The SPN is case-sensitive, and must
be exactly in the form used to export the keytab.

### Setup for the Windows Event Forwarding Target

To set up the Windows event forwarding target, you'll need to create group policy objects (GPOs) for the [subscription target](#gpo-subscription-target), the [WinRM service](#gpo-WinRM-service), and the [NETWORK SERVICE](#gpo-network-service).

#### Create a GPO for the Subscription Target {#gpo-subscription-target}

1. Access the UI **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Event Forwarding** > **Configure target Subscription Manager**.

1. Set to `Enabled`. 

1. Go to **Subscription Managers** > **Show**.

1. Add `Server=http://worker1.contoso.com:5985/wsman/SubscriptionManager/WEC,Refresh=60` to the list.

#### Create a GPO for the WinRM Service {#gpo-WinRM-service}

1. If you haven't already done so, create a GPO to auto-start the WinRM service on the client
machines:

1. Access the UI **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **System Services** > **Windows Remote Management (WS‑Management)**.

1. Set **Startup type** to `Automatic`.

#### Create a GPO for the NETWORK SERVICE {#gpo-network-service}

1. If you haven't already done so, create a GPO so that the NETWORK SERVICE can read event logs (required if you want to subscribe to the Security event log):

1. Access the UI **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Restricted Groups**.

1. In the right pane, right-click and select **Add Group**.

1. Enter `Event Log Readers` as the group name.

1. Under `Members of this group`, click **Add**.

1. Enter `NETWORK SERVICE` as the member name.

> For the change to take effect, you'll have to restart computers after you apply this GPO.
>
{.box .info}

When you have finished up your environment, select the [appropriate settings for Kerberos authentication](#kerberos-auth-settings).

## Troubleshooting {#troubleshooting}

When using auto-enroll, the `Eventlog-ForwardingPlugin Operational` logs might display an error of this form:

`If Kerberos mechanism is used, verify that the client computer and the destination computer are joined to a domain.`

To resolve this error: In your auto-enroll template's **Client‑Server Authentication Properties**, make sure the **Subject Name format** is set to **Common name**.