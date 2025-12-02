# FIPS Mode


Federal Information Processing Standards [FIPS](https://www.nist.gov/standardsgov/compliance-faqs-federal-information-processing-standards-fips) is a set of US government standards and guidelines for information security. You can deploy Cribl Stream in **FIPS mode**. This mainly restricts the cryptographic algorithms used within Cribl Stream, and also enforces stricter password requirements. 

In Cribl Stream version 4.4.4, FIPS mode is a beta feature that can be enabled in Customer Managed deployments.

## Requirements {#fips-requirements-overview}

To run Cribl Stream in FIPS mode your system and passwords must meet the requirements described in this section. 

### FIPS Mode System Requirements

To run Cribl Stream in FIPS mode, your environment must satisfy the following requirements:

- The operating system must support FIPS [140-2](https://csrc.nist.gov/pubs/fips/140-2/upd2/final); several Linux distributions meet this standard. See NIST's list of tested configurations [here](https://csrc.nist.gov/projects/cryptographic-module-validation-program/certificate/4282).
- In particular, the environment must have a FIPS validated version of OpenSSL installed. For Cribl Stream version 4.4.4, this must be [OpenSSL version 3.0.8](https://www.openssl.org/blog/blog/2023/05/29/fips-3-0-8/), with its associated certificate.

### FIPS Mode Restrictions on Cryptographic Algorithms

When run in FIPS mode, Cribl Stream uses only those cryptographic algorithms that satisfy the FIPS standards. This means that Cribl Stream does not run any part of its code that uses algorithms that do not support FIPS, specifically the MD5 (message-digest) and CRC-32 (Cyclic Redundancy Check 32) algorithms.

Therefore, in FIPS mode:

- [Cribl expressions](cribl-reference) that rely on MD5 or CRC-32 will fail silently.
This includes expressions that use the [`C.Mask.md5()`](expressions-mask#cmaskmd5) method
and can in turn affect the behavior of parent Functions and their existing parent Pipelines.
- The UI will hide certain options normally made available by the typeahead feature.  

### FIPS Mode Password Rules {#password-rules}

When Cribl Stream is in FIPS mode, all passwords must:

- Contain eight or more characters.
- Use characters from three or more of the following categories:

    - Lowercase letters.
    - Uppercase letters located after the first character in the password.
    - Digits located before the last character in the password.
    - Non-alphanumeric ASCII characters such as `#`, `!`, or `?`.
    - Non-ASCII characters such as `ñ`, `€`, or emoji.



> If you get locked out of your account, you need to [reset](authentication#manual-pwd) your password manually.
> 
{.box .info}

## Running Cribl Stream in FIPS Mode {#running-fips}

> Cribl Stream version 4.4.4 does not support migrating a previous version of Cribl Stream to version 4.4.4 with FIPS mode. To begin using Stream with FIPS enabled, follow the steps below.
{.box .info}

Before you start Cribl Stream **for the first time after installing**: 
- Set the `CRIBL_FIPS` environment variable to `1` (true), **or**,
- Edit `cribl.yml`, adding this top-level element:
  
    ```
    fips: true 
    ```

Start Cribl Stream.

Verify that you are in FIPS mode.
   
- On the Leader, search `cribl.log` for this message:
  
    ```
    "level":"info","message":"running with FIPS enabled"
    ```

- The presence of the above message confirms that Cribl Stream is in FIPS mode.
  
Log in as `admin` – you will be prompted to enter a FIPS compliant password.
  


