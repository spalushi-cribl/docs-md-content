# Palo Alto Syslog to Cribl.Cloud


Panorama management appliances give you insights into what is happening behind your Palo Alto Networks firewalls, including details about applications, URLs, threats, data files, and patterns. By forwarding logging data directly from Palo Alto firewalls or Panorama management appliances to your Cribl.Cloud instance, you can shape, process, search, and route the data to the analytics platform of your choice.

This guide outlines how to configure your Palo Alto Networks firewalls or Panorama management appliances to connect securely to your Cribl.Cloud instance using Google Trust Services (GTS) as the Certificate Authority (CA).

## Obtain Cribl.Cloud CA Certificates 

Google Trust Services publishes the necessary certificates in a user-friendly repository. Follow these steps to download the required certificates:

1. Visit the [Google Trust Services Root Repository](https://pki.goog). 
1. Download the following certificates:
   - `GTS Root R1` (PEM format): This is the root certificate for Google Trust Services.
     - **Fingerprint**: `d9:47:43:2a:bd:e7:b7:fa:90:fc:2e:6b:59:10:1b:12:80:e0:e1:c7:e4:e4:0f:a3:c6:88:7f:ff:57:a7:f4:cf`
   - `WR1` (PEM format): This is the intermediate certificate issued by Google Trust Services.
     - **Fingerprint**:: `b1:0b:6f:00:e6:09:50:9e:87:00:f6:d3:46:87:a2:bf:ce:38:ea:05:a8:fd:f1:cd:c4:0c:3a:2a:0d:0d:0e:45`
  
> You only need to download these certificates once.
>
{.box .info}


### Import Trusted Roots into PAN

Next, you need to import the certificates into PAN. In the PAN OS:
1. Go to **Device** > **Certificate Management** > **Certificates**.
1. Select the **Import** button at the bottom. 
1. The **Import Certificate** configuration modal appears. Import both downloaded certificates (`GTS Root R1` and `WR1`).
1. Once you have imported both certificates, go to the **Device Certificates** table and select the blue link for each certificate you just imported. 
1. Select the **Trusted Root CA** check box for each certificate.

### Configure the Syslog Server Profile

Now that you've imported the trusted certificates, proceed with creating a new Syslog Server Profile:

1. After importing the certs, navigate to **Device** > **Server Profiles** > **Syslog**. 
1. Create a new Syslog Server Profile.

    > If you're using Syslog over TLS, make sure to set the profile to use SSL instead of TCP.
    > 
    > Even if a PAN rule does not explicitly reference the TLS certificate that you added from Cribl, TLS will detect the certificate information and use it correctly.
    >
    {.box .info}

1. Configure the Syslog server field with your Cribl.Cloud Organization's ingest address. To find your ingest address, navigate to your Cribl.Cloud portal and select the gear ⚙️ on the top bar. 
1. Select **Network Settings** and choose the Worker Group you're sending data to. For example, sending to the default Group your ingest address would look like:
    `default.main.<OrganizationID>.cribl.cloud.`

### Configure Log Settings to Use the Syslog Profile

Next, configure your log settings to use the Syslog Profile. 

1. Go to **Device > Log Settings**.
2. Ensure the desired log types are configured to use the newly created Syslog Server Profile.



## Configure Firewall Rule Log Forwarding

To selectively forward specific log types, create a Log Forwarding Profile and define the rules for which traffic should be forwarded.

1. Use the **Log Forwarding Profile** to create a ruleset and configure the appropriate log types to be forwarded.
2. Go to **Objects > Log Forwarding** and configure each relevant section with the appropriate settings.


## Forward Admin Activity Logs

For additional context, you can forward your admin activity logs:

1. Go to **Device > Setup > Management**, and select the gear ⚙️ in the **Logging and Reporting** settings. 
2. Enable the **Debug and Operational Commands** and **UI Actions**, and choose the appropriate Syslog server.


## Customize OCSP Validation Settings for the `syslog-ng` Process

If the certificate validation fails, follow these steps to customize the validation settings for the `syslog-ng` process. This requires shell access to your device.


> These commands are for an individual PAN OS device and might differ for Panorama. Refer to the official PAN-OS documentation for specific instructions on your version.
>
{.box .info}


```shell
set syslogng ssl-conn-validation all-conns skip
set syslogng ssl-conn-validation explicit OCSP skip CRL skip EKU skip
```

## Validation 

To validate whether you have successfully set up the TLS-encrypted Syslog, check the logs on your PAN OS.



## Troubleshooting

If you run into issues not anticipated above, you can use some of the following commands to further diagnose them:

```shell
# restart the syslog-ng process
debug syslog-ng restart

# see syslog-ng process logs
tail follow yes mp-log syslog-ng.log

# get stats for processes
debug log-receiver statistics
debug syslog-ng stats

# tcpdump activity on the syslog port and view the results
# note: control-c to exit the tcpdump, it will not show anything on the cli when running
tcpdump filter "port 6514" snaplen 0
view-pcap mgmt-pcap mgmt.pcap
```