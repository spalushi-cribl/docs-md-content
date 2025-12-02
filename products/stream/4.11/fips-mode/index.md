# FIPS Mode


Federal Information Processing Standards [FIPS](https://www.nist.gov/standardsgov/compliance-faqs-federal-information-processing-standards-fips) represent a set of US government standards and guidelines for information security. You can deploy Cribl Stream in **FIPS mode**. This mainly restricts the cryptographic algorithms used within Cribl Stream and also enforces FIPS compliant password requirements. 

> Do not start Cribl Stream without FIPS mode enabled, or else you will be unable to run it in FIPS mode later. You must enable FIPS mode as described in this section, after installing **but before starting** Cribl Stream. 
{.box .warning}

## Requirements {#fips-requirements-overview}

This section describes the system and password requirements for FIPS mode.

### FIPS Mode System Requirements

To run Cribl Stream in FIPS mode, your environment must satisfy the following requirements.

#### Operating Systems 

Must support FIPS [140-2](https://csrc.nist.gov/pubs/fips/140-2/upd2/final). Several Linux distributions meet this standard. See NIST's list of tested configurations [here](https://csrc.nist.gov/projects/cryptographic-module-validation-program/certificate/4282).

> While RHEL 9 can be configured to run in FIPS mode at the operating system level, this **does not automatically enable FIPS mode within Cribl Stream.** You must still follow the procedures outlined in this document to explicitly enable Cribl Stream's FIPS mode. Cribl Stream relies on its own configuration to ensure FIPS compliance, independent of the OS-level FIPS setting.
{.box .warning}

##### OpenSSL

The operating system environment must have a FIPS validated version of OpenSSL installed. 

- **For Cribl Stream versions 4.5.0 through 4.8.1**: The FIPS provider version must be 3.0.8 or 3.0.9, and the system-wide OpenSSL version must also be [3.0.8](https://www.openssl.org/blog/blog/2023/05/29/fips-3-0-8/) or [version 3.0.9](https://www.openssl.org/blog/blog/2024/01/23/fips-309/index.html), with its associated certificate.
- **For Cribl Stream starting with version 4.8.2**: The FIPS provider version requirement must be >= 3.0.5, and the system-wide OpenSSL version is no longer a factor. Cribl Stream now verifies the OpenSSL version bundled within the binary.

Enterprise Linux 9 is the only version of Enterprise Linux that includes OpenSSL version 3.0.9 out of the box. While it's possible to manually install compatible versions of OpenSSL on older Enterprise Linux versions, this process is complex and generally not recommended. Therefore, the minimum recommended Enterprise Linux version for running Cribl Stream in FIPS mode is 9.

Even when running Cribl Stream on a FIPS-enabled RHEL 9 system, you must still explicitly enable Cribl Stream's FIPS mode as described in the [Running Cribl Stream in FIPS Mode](#running-fips) section below. The OS being in FIPS mode is not sufficient for Cribl Stream to operate in a FIPS-140 compliant manner.

### FIPS Mode Restrictions on Cryptographic Algorithms

When running in FIPS mode, Cribl Stream uses only those cryptographic algorithms that satisfy the FIPS standards. This means that Cribl Stream operates exclusively with FIPS-compliant cryptographic algorithms, disallowing the use of non-compliant algorithms like MD5 (message-digest) or CRC-32 (Cyclic Redundancy Check 32) algorithms.

To ensure proper functionality in FIPS mode, the FIPS provider version must be at least `3.0.5`.

Therefore, in FIPS mode:

**[Cribl expressions](cribl-reference) that rely on MD5 or CRC-32 will fail silently.**
This includes expressions that use the [`C.Mask.md5()`](expressions-mask#cmaskmd5) method
and can in turn affect the behavior of parent Functions and their existing parent Pipelines.

**The UI will hide certain options** normally made available by the typeahead feature.

**No Source, Destination, or Email Notification target that uses the Amazon AWS SDK v2 will perform checksums.** This is because that SDK uses MD5 to verify checksums, and it applies to both incoming or outgoing payloads. Specifically:
- The [Amazon Kinesis Data Streams Source](sources-kinesis-streams) will not perform checksums and the **Verify KPL checksums** setting will not be available.
- The [Amazon SNS Notification target](aws-sns-notification-targets) will not perform checksums. 
- The following Destinations will not perform checksums:
  - [Amazon CloudWatch Logs](destinations-cloudwatch-logs)
  - [Amazon Kinesis Data Streams](destinations-kinesis-streams)
  - [Amazon SQS](destinations-sqs)
  - [Google Cloud Storage](destinations-google-cloud-storage)
  - [MinIO](destinations-minio)
  - [Prometheus](destinations-prometheus)


###  Role-Based Access Control (RBAC) Controls {#rbac}

Cribl Stream's FIPS mode requires [RBAC](roles#rbac-concepts) to be enabled for enhanced security. This ensures proper access management within FIPS-compliant environments. To enable RBAC, the Cribl Stream instance has to be running in Distributed mode.
FIPS mode is available on certain license tiers. For details, see [Pricing](https://cribl.io/pricing/).

If FIPS mode is already enabled and RBAC is subsequently disabled, Cribl Stream will automatically disable FIPS mode and log a message stating:

 `"Disabling FIPS due to Role-based Access Control (RBAC) being disabled."`

Check `<cribl_install_dir>/local/_system/instance.yml` to ensure Cribl is running in Distributed mode. The default installation directory is typically `/opt/cribl`. The `mode` setting in this file should be set to `distributed`. If it is set to `standalone`, you will need to switch to a Distributed deployment to use FIPS mode with RBAC enabled.


### FIPS Mode Password Rules {#password-rules}

When Cribl Stream is in FIPS mode, all passwords must:

- Contain eight or more characters.
- Use characters from three or more of the following categories:

    - Lowercase letters.
    - Uppercase letters located after the first character in the password.
    - Digits located before the last character in the password.
    - Non-alphanumeric ASCII characters such as `#`, `!`, or `?`.
    - Non-ASCII characters such as `ñ`, `€`, or emoji.

As of Cribl Stream version 4.5.0, these rules apply for all passwords, whether or not Cribl Stream is running in FIPS mode, with one exception:
* For Cribl Stream not running in FIPS mode, local users whose passwords existed before Cribl Stream 4.4.4 was released can continue to use their passwords, even if those passwords do not satisfy the rules. 
* When these users change to new passwords, the new passwords must satisfy the rules. 
* New users must create passwords that satisfy the rules.

> If you get locked out of your account, you need to [reset](authentication#manual-pwd) your password manually.
> 
{.box .info}

## Running Cribl Stream in FIPS Mode {#running-fips}

To begin using Stream with FIPS enabled, complete either the [non-systemd](#fips-non-systemd) or the [systemd](#fips-systemd) procedure below, **before you start Cribl Stream for the first time after installing**. 

### Enabling FIPS Mode on a Non-systemd Distribution {#fips-non-systemd}

If the Linux distribution on which you run Cribl Stream **does not** use systemd:

1. Complete **either** of the two following steps:

    - Set the `CRIBL_FIPS` environment variable to `1` (true), **or**,
    - Edit `cribl.yml`, adding this top-level element:
  
      ```
      fips: true 
      ```
      Note that `fips: true` is the entire top-level element. For this element, the following "stanza" syntax would be incorrect – **do not** use it:

      ```
      # stanza syntax is incorrect for the fips element 
      # even if other elements use it
      fips
        fips: true 
      ```       
2. If you have installed a non-default OpenSSL binary (otherwise, skip this step):

    * Run the following command, and in the output, note the values of `MODULESDIR` and `OPENSSLDIR`. 
   
      ```
      <binary_of_non-default_OpenSSL> version -a
      ```
   * Set `OPENSSL_MODULES` to the value of `MODULESDIR`.
   * Set `CRIBL_OPENSSL_DIR` to the value of `OPENSSLDIR`.

3. Ensure [RBAC](#rbac) is Configured in Cribl Stream. 
   
4. Start Cribl Stream.

Finally, verify that you are in FIPS mode as explained [below](#verify-fips).

### Enabling FIPS Mode on a systemd Distribution {#fips-systemd}

If the Linux distribution on which you run Cribl Stream **does** use systemd:

1. Run the following command, and in the output, note the values of `MODULESDIR` and `OPENSSLDIR`. (As shown, the command will run the default OpenSSL binary. If you have installed a different OpenSSL binary, you can modify the command to run that one instead.)
   
    ```
    openssl version -a
    ```

2. Run the following command, replacing the placeholders with the relevant paths from your environment. (This assumes that you do not already have a `nodejs.cnf` file. If you **do** have one, complete the [workaround](#nodejs-cnf-workaround) below instead of this step.)


    ```
    <path_to_Cribl_binary> generateFipsConf -d <path_to_OpenSSL_root_directory>
    ```
    If you encounter issues generating the FIPS configuration file, a discrepancy in the OpenSSL path might be the cause. For more detailed troubleshooting specific to RHEL 9, refer to the knowledge base article: [Installing Cribl Stream in FIPS Mode on RHEL 9](https://knowledge.cribl.io/stream-54/installing-cribl-stream-in-fips-mode-on-rhel-9-120).

    
    <br>An example of a successful command and its output:

    ```
    useralice@123456791234:/# /opt/cribl/bin/cribl generateFipsConf -d /lib/usr/ssl
    /opt/cribl/state/nodejs.cnf
    ```
    
    This command creates `nodejs.cnf` – the configuration file that NodeJS uses to run in FIPS mode as Cribl Stream starts. 

3. Run the following command to open an empty temporary file in an editor:

    ```
    systemctl edit cribl
    ```

4. Add the following content to the file, replacing the first placeholder with the value of `MODULESDIR` that you noted in Step 1, and the second with the path to your `nodejs.cnf` file:

    ```
    [Service]
    Environment="OPENSSL_MODULES=<value_of_MODULESDIR>"
    Environment="OPENSSL_CONF=<path_to_nodejs.cnf_file>"
    ```

5. Save and exit the editor. 

6. Run the following commands to reload the systemctl overrides and start Cribl:

    ```
    systemctl daemon-reload
    systemctl start cribl
    ```

7. Finally, verify that you are in FIPS mode as explained [below](#verify-fips).

#### When a `nodejs.cnf` File Already Exists {#nodejs-cnf-workaround} 

If you already have a `nodejs.cnf` file, replace Step 2 [above](#fips-systemd) with the following workaround:

1. Create a file with the following content, replacing `<placeholder>` with the value of `OPENSSLDIR`. Name the file something other than `nodejs.cnf`. 

    ```
    nodejs_conf = nodejs_init
    openssl_conf = nodejs_init

    .include /<placeholder>/fipsmodule.cnf

    [nodejs_init]
    providers = provider_sect

    [provider_sect]
    default = default_sect
    # The fips section name should match the section name inside the
    # included fipsmodule.cnf.
    fips = fips_sect
    [default_sect]
    activate = 1 
    ```

2. Resume the [main procedure above](#fips-systemd), starting with Step 3.

### Verifying that you are in FIPS mode {#verify-fips}
   
- On the Leader, search `cribl.log` for this message:
  
    ```
   "level":"info","message":"running with FIPS enabled", "OpenSSLVersion":"3.0.13+quic","OpenSSLFIPSProviderVersion":"3.0.8"
    ```

- The presence of the above message confirms that Cribl Stream is in FIPS mode.
  
Log in as `admin` – you will be prompted to enter a FIPS compliant password.
