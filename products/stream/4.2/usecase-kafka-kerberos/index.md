# Kafka Authentication with Kerberos


This page describes how to set up a [Kafka](https://kafka.apache.org/) Source and Destination configured to use Kerberos authentication. It also documents the verification steps to test your setup.

Kerberos is only available on Linux, so you'll have to use a Linux host or Docker container to work through this procedure.

For additional information on Kafka configuration, see also [Kafka Sources](sources-kafka) or [Kafka Destinations](destinations-kafka).

### Set up the Cribl Stream Host

Now that you have set up an instance of Kafka with Kerberos, perform the following steps on the same host that Stream is running on.

1. Install `krb5-user` (or the equivalent for non-Ubuntu systems) on your Worker Node.
1. Ensure you have a valid `/etc/krb5.conf` file. Consult your Kerberos admin if you don't have one. Kerberos authentication with Kafka specifically requires `rdns = false` in this config, but no other special settings. Cribl Stream supports the file and directory cache types.
1. Ensure the keytab path in your configuration points to a valid keytab on each Worker Node.

## Configure the Kafka Destination

Create a new Kafka Destination with the following settings:

### General Settings

- **Brokers**: `broker.kerberos-demo.local:9092`
- **Topic**: `Cribl`

### Authentication Settings

- **Enabled**: `Yes`
- **SASL mechanism**: `GSSAPI/Kerberos`
- **Keytab location**: `/<path-to-client-keytab>/kafka-client.key`. This path to the valid keytab file must be available on all Worker Nodes.
- **Principal**: `kafka_producer@TEST.CONFLUENT.IO`
- **Broker service class**: `kafka`

## Configure the Kafka Source

Create a new Kafka Source with the following settings:

### General Settings

- **Brokers**: `broker.kerberos-demo.local:9092`
- **Topic**: `Cribl`
- **Group ID**: `Cribl`

### Authentication Settings

- **Enabled**: `Yes`
- **SASL mechanism**: `GSSAPI/Kerberos`
- **Keytab location**: `/<path-to-client-keytab>/kafka-client.key`
- **Principal**: `kafka_producer@TEST.CONFLUENT.IO`
- **Broker service class**: `kafka`

## Verify the Setup {#verifying}

1. Open the **Live Data** tab and start a capture with **Capture Time (sec)** and **Capture Up to N Events** set to high values (for example, `1000`).

1. In a separate browser tab, navigate to the Destination modal's **Test** tab and select **Run Test** a few times.

1. Return to the Source's **Live Data** tab and verify that the Source is capturing events.