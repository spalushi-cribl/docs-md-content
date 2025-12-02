# Set Up Cribl Outpost

 

Create a Cribl Outpost to relay communication between Worker Nodes and the Leader.

---

> ##### Preview Feature {#preview}
>
> This Preview feature is still being developed. We do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance, and related documentation could be incomplete.
>
> Please continue to submit feedback through normal Cribl [support channels](/support/),
> but assistance might be limited while the feature remains in Preview.
> 
{.box .info}

## Prerequisites

Cribl Outpost requires an Enterprise plan or Enterprise license. For details, see [Pricing](https://cribl.io/pricing/).

You can install Cribl Outpost on Linux only.
Do not run an Outpost Node on the same host as a Stream Worker to avoid both Nodes competing for resources.

## Create an Outpost



To create a new Outpost:

1. Go to the Cribl [Download page](https://www.cribl.io/download)
   and set the **Software** drop-down to `Cribl Suite (Edge and Stream)` for your processor architecture.
1. Select **Download now** to get the `.tgz` archive.
1. Un-tar the install package in your directory of choice:

   ```shell
   cd /opt/ 
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

1. Set the installation mode to `mode-outpost` to connect the Outpost to the designated Leader
   and allow Stream Workers to connect to it in turn:

   ```shell
   /opt/cribl/bin/cribl mode-outpost /
      -H <leader-hostname-or-IP> /
      -p <port> /
      -u <token> /
      -S 1 /
      -O <outpost-listener-url>
   ```

   - `<token>` must match the Leader token, to ensure a connection with the Leader.
     
     In Cribl.Cloud, get the token for your Workspace from the Worker Node modal (**Add/Update Worker Node**) from the **Auth token** field.
     
     In an on-prem deployment, get the token from **Settings** > **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Auth token**.

   - `<outpost-listener-url>` is the URL on which Worker Node will connect to the Outpost.

     When you are using TLS, the URL must include paths to locally installed certificate and private key files
     in `tls.certPath` and `tls.privKeyPath` respectively, for example:
     `'tls://0.0.0.0:4200?tls.certPath=/opt/certs/outpost.crt&tls.privKeyPath=/opt/certs/outpost.key'`.
     This certificate is not provided by Cribl.

     To indicate a Certificate Authority (CA), append `&tls.caPath=/etc/certs/ca.crt` to the URL.
     See [TLS Defaults and System-wide Settings](securing-tls-overview) for more information about TLS configuration.

     > Using TLS is strongly recommended is all cases, and required when the Outpost Node connects to a Leader Node via TLS,
     > including in a Cribl.Cloud deployment.
     > If you don't want to use TLS, provide `<outpost-listener-url>` in the form of `tcp://<host>:<port>`
     > and set the `-S` argument to `0`.
     {.box .warning}

1. Configure the installation as a service and start it:

   ```shell
   sudo /opt/cribl/bin/cribl boot-start enable -m systemd -u cribl
   sudo systemctl restart cribl-outpost.service
   ```

At this point your Cribl Outpost is ready for use
and you can start configuring Worker Nodes to connect to it.

## Connect Nodes to an Outpost {#connect}

Worker Nodes see Outpost in the same way as a Leader Node.
This means that, to connect a Node to an Outpost, you only need to provide
the Outpost listener URL that you defined with the `-O` parameter, and the Leader auth token.

The Outpost doesn't have a separate auth token.
When configuring Node connection, use the auth token of the Leader.

Configure the connection either from Settings (**Global > Distributed Settings > Leader Settings**),
or by setting the `CRIBL_DIST_LEADER_URL` environment variable:

```
CRIBL_DIST_LEADER_URL=tcp://<leader-token>@<outpost-hostname>:<outpost-port>
```
