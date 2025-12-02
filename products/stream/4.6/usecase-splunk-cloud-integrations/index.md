# Splunk Cloud and BYOL Integrations


Cribl Stream can send data to these flavors of Splunk Cloud:

- The free, single-instance trial version.
- A distributed Splunk Cloud instance with clustered indexers.
- A Bring Your Own License (BYOL) deployment, either in a non-Splunk cloud or on-prem.

You have a choice of two methods for sending the data: 

- [Splunk HEC](https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector) (HTTP Event Collector).
- The S2S (Splunk-to-Splunk) protocol.

Of all the possible combinations, three have proven most useful in the field:

- Using Splunk HEC with the trial version of Splunk.
- Using S2S with a distributed instance of Splunk.
- Using S2S with a BYOL deployment of Splunk.

## When to Use Splunk HEC

Splunk HEC is fast and easy to set up. Under the hood, it uses the HTTP/S protocol. This offers better compression than S2S, which is a binary protocol.

The Splunk HEC endpoints are virtual endpoints, front-ended with load balancers – ELB for AWS, or GLB for GCP. This provides good load-balancing.

Cribl generally recommends using Splunk HEC for integrating with Splunk Cloud, because (1) it requires fewer connections than S2S, and therefore consumes less memory; and (2) because its superior compression yields lower egress costs.

## When to Use S2S

S2S allows each Cribl Stream Worker Process to connect to multiple indexers concurrently, which distributes data very effectively. This helps significantly with Splunk search, by placing a smaller burden on a larger number of indexers. This support for concurrent connections is the main advantage of S2S. Consider S2S if you plan to route all your data through Cribl Stream first, and you prioritize search performance.



## Using Splunk HEC

#### Identify Your Splunk HEC Endpoint

In Splunk Cloud, identify your HEC endpoint, as described in the Splunk [documentation]( https://docs.splunk.com/Documentation/Splunk/8.2.0/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Cloud). Here are some example URL patterns for HEC endpoints:

- Free version: 
`https://inputs.<cloud_stack_name>:8088/<endpoint>`

- Paid Version in AWS: 
`https://http-inputs-<cloud_stack_name>:443/<endpoint>`

- Paid version in GCP: 
`https://http-inputs.<cloud_stack_name>:443/<endpoint>`

A HEC endpoint for a paid version of Splunk Cloud on AWS with an endpoint for JSON-formatted events, for a company called "Acme Group," might look like this:

`https://http-inputs-acmegroup.splunkcloud.com:443/services/collector/event`

Copy the endpoint URL for use when configuring Cribl Stream in the next section.

#### Create HEC Tokens

You need to create at least one HEC token. For deployments where you set up routing to individual indexes, or you use HEC tokens for RBAC on Splunk, you will create multiple HEC tokens.

1. In the Splunk UI, open the **Settings** menu and click **Data Inputs**.

![Settings > Data inputs](image1.d3bbc8afce.png)

2. In the resulting modal's **HTTP Event Collector** section, click **+ Add new**.

![Adding a new HTTP Event Collector](image2.48e771bf31.png)
{border="true"}

3. Name the new token and click **Next**.

![Adding a new token](image3.a9994dc5d4.png)
{border="true"}

4. Do not add any indexes. This way HEC can write to any index. If you prefer a default index other than `main`, choose it from the **Default index** drop-down.

![Indexes for the new token](image4.4a59dc251a.png)
{border="true"}

5. Once the token has been created, copy it for use when configuring Cribl Stream in the next section. 

![Copying the token value](image5.b785c54c53.png)
{border="true"}

#### Add a Splunk HEC Destination in Cribl Stream

From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Destinations**, then select **Splunk** > **HEC** from the **Manage Destinations** page's tiles or left nav. Click **Add New** to open the **HEC** > **New Destination** modal.

In the **General Settings** tab:

 - Grab the values that you copied in the previous section, and paste them into the **Splunk HEC Endpoint** and **HEC Auth Token** fields, respectively. Be sure to specify HTTPS, because the endpoint will default to HTTP.

- Click **Save**.

- Click **Commit & Deploy**.

![Populating General Settings with values from Splunk](image6.994cbb30ba.png)
{border="true"}

#### Verify that Data is Flowing from Cribl Stream to Splunk Cloud

> Be sure you have committed and deployed the newly created configuration. Otherwise, data will not flow to Splunk Cloud, and verification will fail.
>
{.box .danger}

1. In Cribl Stream, open the Splunk HEC Destination that you created in the previous section.

    - In the configuration modal's **Test** tab, click **Run Test**.
    - You should see a **Success** message.

2. In Splunk, search on `index=main cribl_pipe=*`. Events that you sent from the Cribl Stream **Test** tab should appear in the search results.

![Events flowing to Splunk](image8.fda7110390.png)
{border="true"}

##  Using S2S  {#use-s-to-s}

#### Prepare Splunk Cloud for Cribl Stream Integration

1. In Splunk Cloud, download the Splunk Cloud Universal Forwarder credentials app to your desktop. 

2. Change the file suffix from `.spl` to `.tar.gz`.

![Changing the credentials app file suffix](image9.8889d15a9d.png)

3. Untar/unzip the directory to expose the files.

![Credentials app files](image10.c1ed70a76f.png)

4. Locate the following files. You will need them when you configure Cribl Stream in the next section.

    - `./default/<SplunkCloudInstanceName>_cacert.pem`
    - `./default/<SplunkCloudInstanceName>_server.pem`
    - `./default/outputs.conf`
    -  `./local/outputs.conf`

#### Configure Certificate Settings in Cribl Stream

1. In the top menu, go to **Setting** > **Global Settings**.

1. Next, in the left nav, select **Security** > **Certificates**.

1. Populate each field below with the specified content:

    - **Certificate**:  Drag and drop the `server.pem` file. 
    - **Private Key**: Copy and paste just the `private key` section of the `server.pem` file.
    - **Passphrase**: Copy and paste just the SSL password from the `../local/outputs.conf` file.
    - **CA certificate**: Drag and drop the `cacert.pem` file. 
 
#### Add a Splunk Destination in Cribl Stream

The type of Destination to add depends on what form of Splunk you're using:

- For a trial version of Splunk Cloud, select **Splunk Single Instance**.
- For a paid version of Splunk Cloud, select **Splunk Load Balanced**. This is required because any paid version of Splunk Cloud will have multiple indexer entries in the `../default/outputs.conf` file.

1. From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Destinations**, then select either **Splunk** > **Load Balanced** or **Splunk** > **Single Instance** from the **Manage Destinations** page's tiles or left nav. Then click **New Destination** to open the corresponding **New Destination** modal.

2. In the **General Settings** tab, populate the **Address** and **Port** fields.

    - From the `./default/outputs.conf` file you copied in the previous section, divide the value of the `server` line between the two fields shown in the screenshot below.

3. In the **TLS Settings (Client Side)** tab:

    - From the **Certificate name** drop-down, select the certificate that you created.
    - From the `./local/outputs.conf` file, paste the `sslPassword` value into the **Passphrase** field.
    - Click **Save**.
    - Click **Commit** and **Deploy**.


#### Verify that Data is Flowing from Cribl Stream to Splunk Cloud

> Be sure you have committed and deployed the newly created configuration. Otherwise, data will not flow to Splunk Cloud, and verification will fail.
>
{.box .danger}

1. In Cribl Stream, open the Destination that you created in the previous section. 

    - In the configuration modal's **Test** tab, click **Run Test**.
    - You should see a **Success** message.

2. In Splunk, search on `index=main cribl_pipe=*`. Events that you sent from the Cribl Stream Test tab should appear in the search results.

![Events flowing to Splunk](image17.587ad69063.png)
{border="true"}

## Using S2S with Splunk BYOL

Before you begin configuring this option, you should already have Splunk Universal Forwarders configured to send data securely to your Splunk environment. This enables you to:

- Re-use content from the `.pem` and `outputs.conf` files already in use on those Forwarders. 

- Follow the procedures in the in the [previous section](#use-s-to-s) to add the certificate to Cribl.Cloud. 

- Then, reference the certificate in the Splunk Destination configuration.

If you need to secure your Splunk indexers, see the Splunk [documentation](https://docs.splunk.com/Documentation/Splunk/latest/Security/ConfigureSplunkforwardingtousesignedcertificates).

## When Your Data Source is a Splunk Forwarder

If a Splunk [Universal or Heavy Forwarder](http://docs.splunk.com/Documentation/Splunk/latest/Forwarding/Typesofforwarders) is the source of the data you want to send to Splunk Cloud:

- In Cribl Stream, create a [Splunk TCP Source](sources-splunk) to receive data from the Splunk Forwarder.

- This process includes configuring the Splunk Forwarder to point to the new Source in Cribl Stream, and (optionally) securing the communication with TLS.
