# Gigamon to Cribl Stream


Gigamon and Cribl have integrated the [Gigamon GigaVUE Cloud Suite](https://www.gigamon.com/products/access-traffic/cloud-suite.html) with Cribl Stream to optimize telemetry intelligence delivery across hybrid cloud infrastructures. This integration enables seamless data formatting and routing, enhancing the value of existing tools by providing comprehensive observability and security.

Gigamon's [Deep Observability Pipeline](https://www.gigamon.com/products/deep-observability-pipeline.html), with GigaVUE Cloud Suite, delivers deep visibility into hybrid cloud traffic, improving security and performance by addressing East-West application traffic blind spots. Cribl's data management solution enhances threat detection and incident response through seamless access, enrichment, and routing of telemetry data from multiple sources.

In this integration, data is sent from the [Gigamomn Application Metadata Exporter](https://docs.gigamon.com/doclib61/Content/GV-Cloud-V-Series-Applications/Observability-Gareway_Application.html) to a Cribl Stream HTTP Raw Source.

## Prerequisites

- Your Gigamon GigaVUE Cloud Suite has the AMI module enabled.
- You've enabled the appropriate [Application Metadata Intelligence](https://www.gigamon.com/products/optimize-traffic/application-intelligence/application-metadata.html) attributes.
- You know your Cribl Stream Worker Group ingress address, shown on the **Overview** page under **Group Information**.

## Configuring Cribl Stream

1. In your Cribl Stream account, select the Worker Group that will receive the data.
1. Select **Processing**, then **Packs**. Add the **Gigamon** [Pack](packs) from the Dispensary to your Worker Group.
1. Select **Routing**, then **QuickConnect**. Select **Add Source** and add a new **Raw HTTP** Source.
1. Configure the new [Raw HTTP Source](sources-raw-http) as follows:
   - **General Settings**:
     - **Input ID**: Enter a unique name to identify this Raw HTTP Source definition. Such as `Raw HTTP`.
     - **Address**: Enter the address to bind on. Use `0.0.0.0` to bind on all addresses.
     - **Port**: Enter `20001`. This port number is used in the Cribl Stream Worker Group ingress address you'll provide to Gigamon.
   - **Authentication**:
     - Select **Add token** and **Generate** an **Auth token**. You'll provide this token to Gigamon when configuring your **Monitoring Session**.
1. Click **Save**.
1. Use [QuickConnect](quickconnect) to connect your new Raw HTTP Source to [Cribl Destinations](destinations). Be sure to use the Gigamon Pack for the connection configuration.
1. **Commit & Deploy** your changes.

## Configuring Gigamon to Export Data to Cribl Stream:

1. In your Gigamon instance, navigate to the **Traffic** section from the FM instance.
1. Select the location of your deployment, like `AWS`.
1. Choose and edit your **Monitoring Instance**.
1. Select the **Details** for your AMX instance.
1. Create a new Cloud Tool Export as follows:
   - **Alias**: `Cribl`
   - **Cloud Tool**: `Other`
   - **Endpoint**: Provide your Cribl Stream Worker Group ingress address with port `20001`. For example, `https://workergroup.main.orgname.cribl.cloud:20001`.
   - **Headers**:
     - **Authorization**: Provide the **Auth token** you generated when configuring the Cribl Stream **Raw HTTP Source**.
     - **Content-Type**: `application/json`
   - **Enable Export**: Check the box to enable.
   - **Format**: `JSON`
1. Click **Save** and **Deploy** the changes.

## Verifying Data Flow

There are many ways to monitor the status of your Source. Navigate to the Source and use the available tabs.

- Use the [**Live Data**](sources#capturing-source-data) tab to capture metadata received by Cribl Stream.
- The [**Charts**](sources#charts-tab) tab displays throughput details.
- The [**Status**](sources#status-tab) tab provides details about the Workers in the group and their status.

You can also monitor Source throughput on the [Monitoring](monitoring#data-monitoring) page.