# Common Errors and Warnings


This page lists common error and warning messages that you might find in Cribl Stream's internal logs and/or UI. It includes recommendations for resolving the errors/warnings. Messages are grouped by component (Sources, Destinations, etc.). Note that:

- We've excerpted only the salient part of the error message from each event. We haven't listed full events.

- Examples don't preserve case sensitivity from the original events. 

- NodeJS serves as the Cribl Stream backend and has a large collection of well documented errors of its own. Some of system-level are listed below with the appropriate action to take. The remainder are documented at https://nodejs.org/api/errors.html, specifically in the `Common system errors` section of that page.

- Where events are written to log files, they might reside in different logs variously devoted to data processing, cluster communication, or the REST API. (For details, see [Types of Logs](monitoring#types-of-logs).) We note the log type where necessary.


## Web UI
**Where:** In Cribl Stream's UI.

### Error: Duplicate GUID

When you deploy and first run Cribl Stream software, it generates a GUID (globally unique identifier) and stores it in a `.dat` file located in `CRIBL_HOME/local/cribl/auth/`. For example: 

```shell
# cat CRIBL_HOME/local/cribl/auth/676f6174733432.dat

{"it":1570724418,"phf":0,"guid":"48f7b21a-0c03-45e0-a699-01e0b7a1e061"}
```

When deploying Cribl Stream as part of a host image or VM, be sure to remove this file, so that you don't end up with duplicate GUIDs. The file will regenerate on the next run.

### Error: "Failed to fetch"

**Cause:** This occurs only when accessing **Data** > **Collectors**. It can occur with ad blockers, and occurs with the Brave browser (because the newly loaded page's URL includes the string `collector`). 

**Recommendation:** If you have an ad blocker, allowlist the `https://<hostname>:9000/jobs/collectors` (single-instance) URL or the `https://<masternode>:9000/m/<group_name>/jobs/collectors` (Leader) URL.

### Error: "No workers registered"

**Cause:** Occurs during a live data capture, when there are no Workers available
to fulfill the capture request. This error can be triggered if you attempt to capture
events too quickly following a commit & deploy. 

**Recommendation:** There are two things you can try:

- If you recently committed and deployed changes, wait 30 seconds and retry the live capture.

- Navigate to **Manage > Workers** and confirm there are Workers currently registered with the
  Leader. Note that the error appears on a per-Group basis, which means that even if you have Workers
  registered, they may not be in the Group you're capturing live data from.

### Error: "The Config Helper service is not available because a configuration file doesn't exist or the settings are invalid. Please fix it and restart Cribl Stream." {#config-helper}

**Cause:** Occurs only on Leader Nodes. Each Worker Group relies on a config helper process that runs on its behalf on a Leader Node, and that process is not currently running. This error is usually triggered by restarting Cribl Stream on the Leader Node, and then trying to access one of these UI features before the Cribl Stream server is fully up:
- **Groups** or **Configure** link (3.x).
- **Worker Groups** menu (2.x).
- **Manage** > **Groups** or **Manage** > **Workers** tabs (4.x).

**Recommendation:** Try again in a few seconds. If you've made changes to the backend or filesystem permissions, stop Cribl Stream, and make sure the `cribl` user owns all relevant assets, before restarting: `sudo chown -R cribl.cribl /opt/cribl`

### Warning: "KMS saved, but secret exists in destination location. No data migrated."

**Cause:** This will appear when you attempt to save KMS Settings with a **Secret Path** entry that references a secret that already exists in HashiCorp Vault. It can also occur with OpenID Connect remote authentication. Cribl Stream has aborted the remote write to avoid overwriting an external shared secret.

**Recommendation:** Edit the Leader Node's `kms.yml` file  to use `provider: local`. Then restart Cribl Stream to correct the path conflict.

### Logs: "The number of Worker Processes has been limited by the license."

**Cause:** Workers use their local license until they connect to the Leader and download their current license data, which is why the message appears in Logs.

**Recommendation:** Ignore the message. Once the Worker checks in with the Leader and receives the production license, the rest of the Workers will start.

## Sources (General)
**Where:** These events will be in the Worker process logs.

### Error: "bind EACCES 0.0.0.0:514"

**Cause:** Cribl Stream doesn't have proper privileges to bind to the specified IP and port. Usually, this simply indicates that Cribl Stream is running as a non-root user, but a Source was accidentally configured to use a privileged port below 1024.

**Recommendation:** Check your Cribl Stream Sources' configuration. The error's `channel` field will indicate which input is affected. If you choose to enable access to the port range below 1024, see our configuration instructions for [systemd](deploy-single-instance#persisting-overrides) and [initd](deploy-single-instance#persisting-overrides-initd).

![EACCES error](Screen_Shot_2021-04-14_at_11.36.07_PM.58f58d3695.png)
{scale="40%"}

### Error: “bind EADDRINUSE 0.0.0.0:9514”

**Cause:** The interface and port combination is already in use by another process, which might or might not be Cribl Stream.

**Recommendation:** Check your Cribl Stream Sources' configuration. The error's `channel` field will indicate which Source is affected, but this error means that one or more other inputs are also using the same IP/port combination.

![ADDRINUSE error](Screen_Shot_2021-04-14_at_11.27.51_PM.0117523d8f.png)
{scale="45%"}

## HTTP-based Source
Where: These events will be in the Worker Process logs.

### Message: "Dropping request because token invalid","authToken": "Bas...Njc="" (v2.4.5 and later)

**Cause:** The specified token is invalid. Note the above message is logged only at debug level.

## Kafka-based Source
Where: These events will be in the Worker Process logs.

### Error: "KafkaJSProtocolError: Not authorized to access topics: [Topic authorization failed]"

**Cause:** The username does not have read permissions for the specified topic.

## Splunk TCP Source
Where: These events will be in the Worker Process logs.

### Error: "connection rejected" with a `reason` of "Too many connections"

**Cause:** The maximum number of active Splunk TCP connections has been exceeded per worker process. The default is 1000.

**Recommendation:** In the Splunk TCP input's Advanced Settings configuration, increase the Max Active Connections value, set it to 0 for unlimited, and/or increase the # of worker processes the Worker Node(s) are using..

## Splunk HEC Source
Where: These events will be in the Worker Process logs.

### Error: "Malformed HEC event"

**Cause:** The event is missing both `event` and `fields` fields.

### Error: "Invalid token"

**Cause:** Auth token(s) are defined, but the token specified in the HEC request doesn't match any defined tokens, therefore it's invalid.

### Error: "Invalid index"

**Cause:** The Splunk HEC Source's **Allowed Indexes** field is configured with specific indexes, but the client's HTTP request didn’t specify any of them.

### Error: "{"text":"Server is busy","code":9,"invalid-event-number":0}"

**Note**: This is not logged in Cribl Stream, but would be found in the response payload sent to your HEC client.

**Cause:** "Server is busy" is the equivalent of a 503 HTTP response code. The most likely cause is the **Max active requests** setting in the HEC input's Advanced Settings is insufficient to service the number of simultaneous HEC requests. Increase the value and monitor your clients to see if the 503 response is eliminated.

## TLS Errors
**Where:** These events can be in Worker Process or API logs on the Workers or Leader, depending on whether the issue is associated with a Source or Destination, etc.

### Error: “certificate has expired”

**Cause:** **Validate server certs** or **Validate client certs** is enabled, and the peer's certificate has expired.

**Recommendation:** Disable **Validate server certs** or **Validate client certs** (depending on whether Cribl Stream is serving as the client or server), so that encryption can still occur without authentication. Or renew the expired certificate.

### Error: “Client network socket disconnected before secure TLS connection was established”

**Cause:** Typically caused by a TLS protocol version mismatch between the client and server.

**Recommendation:** Verify that client's and server's TLS settings use the same minimum/maximum TLS version.

### Error: “Unable to get local issuer certificate”

**Cause:** Client can’t validate the server certificate’s CA (i.e., the issuer) because it doesn’t trust the CA cert. Or vice versa (server can't validate client's certificate CA) if mutual auth is enabled.

**Recommendation:** The CA certificates used by the server's leaf certificate must be trusted by the client. See [Configuring CA certs](securing-and-monitoring#env-vars).

### Error: “self signed certificate”

**Cause:** The client or server was presented with a self-signed cert from the peer, but that cert is not trusted.

**Recommendation:** The self-signed certificate must be trusted by the peer to which it's presented. See [Configuring CA certs](securing-and-monitoring#env-vars).

### Error: “Hostname/IP does not match certificate’s altnames"

**Variations:**
“Hostname/IP does not match certificate’s altnames: host: &lt;server hostname&gt; is not in the cert’s list” 
or:
“Hostname/IP does not match certificate’s altnames: IP: &lt;server IP&gt; is not in the cert’s list”

**Cause:** The server’s hostname/FQDN used on the client is not found in the CN or SAN attribute of the server’s certificate.

**Recommendation:** Examine the CN and/or SAN attribute, to see which names are listed that can be used as the server hostname/FQDN on the client. **CN values with spaces are not supported as hostnames/FQDNS.** If there isn’t a SAN attribute, then a new cert will need to be issued.

### Error: "140244944713600:error:141E70BF:SSL routines:tls_construct_client_hello:no protocols available:"

**Cause:** The highest TLS protocol available by the client is still too low for the server to support.

**Recommendation:** Review the minimum/maximum TLS version settings on the client and server, to ensure that they overlap.

### Error: "140251374995328:error:1408F10B:SSL routines:ssl3_get_record:wrong version number"

**Cause:** The client is using TLS but the server is probably not configured for TLS.

**Recommendation:** Review the TLS settings on the server.


## REST API Collector
**Where:** These events will be in the Worker Process logs.

### Error: "reason.stack == `TypeError [ERR_INVALID_URL]: Invalid URL: https%3A%2F%2Ftype.fit%2Fapi%2Fquotes" at onParseError (internal/url.js:258:9)"

**Cause:** This is due to unnecessarily encoding the Discover/Collect URL.

**Recommendation:** Remove the encoding function for the URL. URLs will rarely need text be encoded and when they do it's only the parts that need it that should be encoded, otherwise if the entire URL is encoded unnecessarily then errors like this will occur.

![Invalid URL](Screen_Shot_2021-04-13_at_5.53.56_PM.e4e38e435c.png)
{scale="40%"}

### Error: "statusCode: 429...Too many requests"

**Cause:** This response is triggered by rapidly repeated authentication requests from the Collector's Discover and Collect phases. It's especially likely when different Workers run multiple Collect tasks.

**Recommendation:** Gradually increase the **Login rate limit** threshold until you no longer receive this error response. On the Leader or a single-instance deployment, access this option at **Global Settings** > **General Settings** > **API Server Settings** > **Advanced**. In a distributed deployment, select **Manage**, select the Worker Group, and then navigate to **Group Settings** > **General Settings** > **API Server Settings** > **Advanced**.

## AWS Sources/​Destinations & S3-Compatible Stores {#aws-sources-destinations}
**Where:** These events will be in the Worker Process logs.

### Error: "message":"Inaccessible host: 'sqs.us-east-1.amazonaws.com'. This service may not be available..."

**Full Error Text:** `"message":"Inaccessible host: 'sqs.us-east-1.amazonaws.com'. This service may not be available in the 'us-east-1' region.","stack":"UnknownEndpoint: Inaccessible host: 'sqs.us-east-1.amazonaws.com'. This service may not be available in the 'us-east-1' region.`

**Cause:** If this is persistent rather than intermittent then it could be caused by TLS negotiation failures. For example, AWS SQS currently does not support TLS 1.3. If intermittent then a network-related issue could be occurring such as DNS-related problems.

### Error: "Missing credentials in config" or "stack:Error: connect ETIMEDOUT 169.254.169.254:80"

**Cause 1:** Can occur when **Authentication** is set to **Auto**, but no IAM role is attached.

**Cause 2:** Can occur on LogStream 2.4.4 or earlier, when an IAM role is attached to the EC2 instance, but the instance is using instance metadata v2.

**Recommendations:** Change to Manual Authentication; attach an IAM role; or if using IMDv2, switch to IMDv1 (if possible) or upgrade to LogStream 2.4.5 or later.

![Missing credentials in config](Screen_Shot_2021-04-12_at_11.22.57_PM.6869e9ac15.png)
{scale="40%"}

### Error: "message":"Bucket does not exist - self signed certificate

**Example Error Text:** 
`message: Bucket does not exist - self signed certificate
stack: Error: self signed certificate
    at TLSSocket.onConnectSecure (node:_tls_wrap_1535:34)
    at TLSSocket.emit (node:events:513:28)
    at TLSSocket.emit (node:events:489:12)
    at TLSSocket._fini... Show more
tmpPath: /opt/cribl/s3_bucket/cribl/2023/03/28/CriblOut-XTNGh5.0.json.tmp`

**Cause 1:** Can occur when the Destination uses a self-signed cert that has not yet been trusted by Cribl Stream.

**Cause 2:** Can occur when the user does not have appropriate permissions to view the bucket.

**Cause 3:** Can occur when the authentication token generated by your computer has a timestamp that is out of sync with the server's time, resulting in an authentication failure.

**Recommendations:** Disable **Advanced Settings** > **Reject unauthorized certificates** or; get the sender's cert and add it to the `NODE_EXTRA_CA_CERTS` path for validation; or verify that the user has appropriate permissions.

## Destinations (General)
**Where:** These events will be in the Worker Process logs, unless otherwise noted.

### Warning: “sending is blocked”

**Cause:** The Destination is blocking. This can occur with any TCP-based Destination, and is logged only after 1 second of blocking. This can also occur between Worker and Leader when the Worker can't connect to the Leader Node to send metrics data. When triggered by cluster communication, the warning will be in the Worker's API log.

### Warning: "exerting backpressure" (v2.4.0-2.4.1)

**Cause:** The Destination is blocking. This message is logged immediately upon detecting backpressure, for Destinations using any protocol. Some backpressure is normal when measured over timescales under 1 second, therefore this message can appear quite frequently, and is not indicative of a problem (which is why it's a warning).

### Warning: "begin backpressure" and "end backpressure" (v2.4.2 and later)

**Cause:** The Destination is blocking. Like the "sending is blocked" message, "begin backpressure" is logged only after 1 second of blocking. Unlike the "exerting backpressure" message, it is logged only once while backpressure is occurring (at the start), and it will always be followed by the "end backpressure" message.

## MinIO Destination
**Where:** These events will be in the Worker Process logs.

### Error: "Parse Error: Expected HTTP/"

**Cause:** The Worker is trying to use HTTP, but the server is expecting HTTPS. 

![Parse Error: Expected HTTP](Screen_Shot_2021-04-12_at_10.20.34_PM.4eea073d13.png)
{scale="40%"}

## Pipelines/Functions
**Where:** These events will be in the Worker Process logs.

### Error: "failed to load function...Value undefined  out of range..."

**Cause:** A [Lookup Function](lookup-function) attempted to load a lookup table that exceeded Node.js' hard size limit of 16,777,216 (i.e., 2<sup>24</sup>) rows. 

**Recommendation:** Split the lookup table to smaller tables, or use the [Redis Function](redis-function).

![Oversized lookup table error](lookup-rows-error.9718595e17.png)

## Diag Command
**Where:** On stdout

### Warning: "You are running Cribl Stream CLI as user=root, while the binary is owned by the user=cribl."

**Full Text:** "WARNING: You are running Cribl Stream CLI as user=root, while the binary is owned by the user=cribl. This may change the ownership of some files under CRIBL_HOME=/opt/cribl. Please make sure all files under CRIBL_HOME=/opt/cribl are owned by the user=cribl."

**Cause:** This is caused by improper ownership on `$CRIBL_HOME`, and will cause some files to be missing from the diag. 

**Recommendation:** Execute the `chown` command on the entire `$CRIBL_HOME` directory, so that everything can be owned by the proper user. Afterward, run the `./cribl diag create` command again.

## Cluster
**Where:** These events will be in the Workers' API logs.

### Error: "access denied"

**Cause:** The Worker's authtoken (located in `$CRIBL_HOME/local/_system/instance.yml`) is missing or doesn’t match the Leader's.
