# Cribl Stream 4.8


## New Features

This release provides the following improvements:

### Azure-Based Worker Groups

You now have more choice when creating Cribl-managed Worker Groups: besides the earlier option of Cloud groups based in AWS, you can now create Worker Groups in Azure. You can choose either of the providers whenever you create a new Cloud Worker Group, and in the case of Azure, you can assign the new group to a selected region.

## Experience Improvements 

Now when you opt in to the new Cribl.Cloud user interface, you’ll get an improved Global Settings experience, in addition to the streamlined global navigation experience and access to Workspaces, delivered in previous releases.

The Redis function has a new setting, **Scale Reads**, that allows you to specify which nodes in a Redis cluster should handle read commands. You can choose to direct reads to primary nodes, replica nodes, or both. This setting helps balance load and optimize read performance while providing flexibility between strong consistency and improved scalability.

We’ve added a new Knowledge Library, HMAC (Hash-based Message Authentication Code) Functions, that provide an additional authentication option for REST Collectors. By using HMAC Functions, you can generate unique signatures for each HTTP request, ensuring the integrity and authenticity of your data collection.

To improve disk usage accuracy, the Storage card now displays total storage rounded to two decimal places on the Landing and Monitoring pages. The result is a more precise view of your disk space, which allows you to plan your capacity and use it more effectively.

Projects have undergone several enhancements to improve security and functionality. Data captures and sample data files are now isolated within their respective projects. Additionally, datagen configurations are hidden from Project User roles. We eliminated potential infinite loops within Pipelines when using a Chain Function. And Post-processing Pipelines now function correctly when used in conjunction with Projects.

To run the Cribl image as a non-root user, use Docker's `--user` option or the Docker Compose `user` setting. For correct operation, set the `CRIBL_VOLUME_DIR` environment variable to a writable location. Using a persistent volume mounted to `CRIBL_VOLUME_DIR` is highly recommended for performance and data integrity.

In the Data Routes table you can now adjust the column width by dragging and dropping column dividers.

A new setting can be enabled in the Group's General Settings to automatically capture memory snapshots when Worker Processes approach capacity for hybrid or on-prem deployments. This feature can help identify and troubleshoot memory issues. This setting should only be enabled if recommended by support or if you are experiencing out-of-memory issues. Be aware that enabling this setting may impact system performance and storage usage.

To prevent excessive disk usage, Cribl Stream implemented log rotation for `cribl_stderr.log`, limiting files to 5 MB each with a maximum of five rotated copies. This addresses an issue where the log file could rapidly expand due to logged errors, impacting system performance.

You can now assign a **Mapping ID** to Teams in Cribl.Cloud, allowing you to map IDP groups to Teams.

## Sources and Destinations

We’ve added the ServiceNow Cloud Observability Destination allowing you to seamlessly route logs, metrics, and traces to ServiceNow's Cloud Observability platform using the OpenTelemetry Protocol (OTLP). You can configure the Destination with either HTTP or gRPC protocols, and securely manage your access tokens using Cribl's text secret storage.

The Azure Sentinel Destination has been renamed to Microsoft Sentinel and we’ve added a new setting, **Scope**, that allows you to configure the integration across various Azure environments, including Azure Public Cloud, Azure US Government Cloud, and Microsoft Azure operated by 21Vianet.

We’ve introduced enhanced metadata handling for **Auth tokens** in both HTTP Raw and HTTP (Bulk API) Sources. This provides greater flexibility and control over event processing, enabling you to add context to events based on the token used, and enhancing security by avoiding direct use of authorization values in Routes and Pipelines.

The Cribl TCP and TCP JSON Sources now include an **Enable load balancing** setting. This distributes the data from incoming TCP connections across multiple Worker Processes, enhancing performance compared to the default of having all data from an incoming connection processed on a single Worker Process. This helps mitigate issues related to TCP connection pinning, ensuring better utilization of available Worker Processes and improving data throughput.

We've added new retry options to improve connection reliability. The **Retry Connection Timeout** setting automatically tries to reconnect after temporary network disruptions. The **Retry Connection Reset** setting automatically reconnects when a connection is unexpectedly closed. These settings are available for the Splunk Search Collector, Splunk Search Source, and Office 365 Activity Source.

The OpenTelemetry (OTel) Source and Destination now support OTLP Metrics sum, summary, and histogram metric types. This ensures that all relevant metrics are accurately represented and transmitted, including support within the OTLP Metrics Function.

The Script Collector now supports passing `earliest` and `latest` parameters to the discover script.

We've strengthened security by enabling default certificate verification for HTTPS connections. Several newly created Sources and Destinations now automatically reject expired, self-signed, or mismatched certificates. This change aligns with industry best practices for TLS verification and provides a more secure and reliable data processing experience.

The Kafka and Confluent Cloud Source and Destination now support basic authentication for the schema registry.

The Amazon CloudWatch Logs Destination now exclusively supports Signature Version 4 authentication.

## Corrections

### Security Fixes

| ID                       | Description                   |
| -----------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------|
|<div style="width: 100px">CRIBL-25761</div>| @azure/msal-node has been updated to version 2.9.2 to include the latest security updates. |
| CRIBL-25764 | mssql2 has been updated to version 10.0.3 to include the latest security updates. |
| CRIBL-25758 | @google-cloud/pubsub has been updated to version 4.5.0 to include the latest security updates. |
| CRIBL-25756 | @grpc/grpc-js has been updated to version 1.9.15 to include the latest security updates. |

### Source and Destination Fixes

| ID                       | Description                   |
| -----------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------|
|<div style="width: 100px">CRIBL-26485</div>| Canceling a REST Collector job from the Job Inspector before it finished prevented state updates for events collected prior to cancellation. As a result, no state was saved for uncompleted tasks within the collection run. |
| CRIBL-26107 | Splunk TCP Sources previously defaulted to the older S2S protocol version 3. This behavior affected new instances and Worker Groups/Fleets. Newly created Splunk TCP Sources within existing Worker Groups/Fleets, however, correctly defaulted to version 4. This issue has been resolved, and all newly created Splunk TCP Sources now default to the correct S2S protocol version 4. |
| CRIBL-25842 | Splunk Destinations experienced issues when sending data via Splunk-to-Splunk (S2S) protocol to Splunk. The connection was terminated before any data was sent, only when acknowledgments (ACKs) were enabled. |
| CRIBL-25972 | The SNMP Trap Source allowed MD5 authentication in FIPS mode. As MD5 doesn't meet FIPS security requirements, it caused data flow problems and error logs. | 
| CRIBL-26377 | When capturing live data, the `_raw` field now expands fully to reveal its entire nested JSON structure when tapped, as expected. |

### Other Functional Fixes

| ID                        | Description                                                |
| ------------------------- | ---------------------------------------------------------- |
|<div style="width: 100px">CRIBL-26719</div>| The C.Time.s3TimePartition() and C.Time.timePartition() Functions no longer cause Worker Processes to crash when encountering events with timestamps prior to the Unix epoch. |
| CRIBL-26478 | The system no longer fails to properly release files from disk when you enable Source-side PQ. |
| CRIBL‑26359, CRIBL‑24336 | Collection jobs no longer get stuck in Pending state when you commit and deploy a configuration while the job is running. |
| CRIBL-26311 | Cribl.Cloud users can now view or download CPU profiles directly through the UI. |
