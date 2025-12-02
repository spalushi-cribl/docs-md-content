# FIPS Mode for Cribl Edge (Windows)


This guide will walk you through the process of setting up and running Cribl Edge in FIPS (Federal Information Processing Standards) mode on Windows systems.

FIPS [140-2](https://csrc.nist.gov/pubs/fips/140-2/upd2/final) or FIPS [140-3](https://csrc.nist.gov/pubs/fips/140-3/final) compliance is required for software deployed in U.S. Federal Government and regulated industries. Cribl Edge supports operation in FIPS mode to meet these requirements.

## Prerequisites

* Cribl Edge Windows installer (.msi).
* Administrative access to the Windows system.
* FIPS-validated OpenSSL provider (instructions below).

## What You Need to Know About Pre-Configuration

> Before configuring FIPS-enabled Edge Nodes, ensure your Leader environment is already configured and running in FIPS mode.
{.box .warning}

* When running in FIPS mode, Cribl Edge uses only FIPS-approved cryptographic algorithms, ensuring all data handling meets federal security standards.
* When connecting to a FIPS-configured Leader, Cribl Edge must also be in FIPS mode for full compliance.
 
## Set up Cribl Edge in FIPS Mode

To set up Cribl Edge in FIPS mode, you'll need to build your own OpenSSL FIPS provider. For details, see [OpenSSL: Notes for Windows platforms](https://github.com/openssl/openssl/blob/master/NOTES-WINDOWS.md#native-builds-using-visual-c).


## Build Your Own FIPS Provider

This section guides you through the process of building your own OpenSSL FIPS provider.

### Install Required Tools

1. Install [Strawberry Perl](https://strawberryperl.com/), which includes the Netwide Assembler — (NASM).
2. Microsoft C++ toolchain (Visual Studio).
3. If you have `git` installed, you may need to rename/move its `perl.exe` to avoid conflicts.

### Build the Provider

1. Find and open the Visual Studio (20XX) Native Tools Command Prompt for `x64` architecture. You can search for it, by typing `x64` in the **Start** menu. Right-click on it and select **Run as administrator**. (Do not use PowerShell or plain CMD). 
2. Download the latest FIPS-validated version of OpenSSL:

    ```
    curl -LO https://www.openssl.org/source/openssl-3.0.9.tar.gz
    tar -xf openssl-3.0.9.tar.gz
    cd openssl-3.0.9
    ```
   If you run into an error message: `ERROR: module machine type 'x86' conflicts with target machine type 'x64'`, it means you're not using the right version of the command prompt for what you're trying to do. Make sure you've selected the `x64 Native Tools Command Prompt`.

3. Run the following commands in the  Visual Studio Developer Command Prompt (with `x64` architecture selected):

   ```
    perl configure VC-WIN64A enable-fips
    nmake
    ```
   The build process might take considerable time to complete. Once the build is complete, you'll have a `providers` directory. The providers directory is created within the OpenSSL build directory (for example, `openssl-3.0.9/providers/`). <br> 

4. Once built, you'll need the following files from the build directory:
   * `providers\fipsmodule.cnf` - This specific file from your build is essential as it contains a unique hash that validates the DLL you built.
   * `providers\fips.dll`
   * `providers\legacy.dll`

### Set up the Provider

This step creates a dedicated directory for Cribl's FIPS-related files and copies the necessary DLL and configuration files into it, preparing the application for FIPS-compliant operation.

In the same Native Tools Command Prompt, type the following:
```
    mkdir "C:\Program Files\Cribl-FIPS"
    copy providers\fips.dll "C:\Program Files\Cribl-FIPS"
    copy providers\legacy.dll "C:\Program Files\Cribl-FIPS"
    copy providers\fipsmodule.cnf "C:\Program Files\Cribl-FIPS"
```

### Generate the `nodejs.cnf` Configuration File {#config}

This step creates a necessary configuration file that enables FIPS compliance for the `Node.js` environment used by Cribl. You have two options.

#### Option 1: Manual Configuration Creation

1. Open an Administrative Command Prompt
2. Run the following command to create and open the configuration file:

```
   notepad "C:\Program Files\Cribl-FIPS\nodejs.cnf"
```
3. Copy and paste the following configuration content into the file:

```
nodejs_conf = nodejs_init
openssl_conf = nodejs_init
.include "C:\\Program Files\\Cribl-FIPS\\fipsmodule.cnf"

[nodejs_init]
providers = provider_sect
alg_section = algorithm_sect

[provider_sect]
default = default_sect
fips = fips_sect

[default_sect]
activate = 1

[algorithm_sect]
default_properties = fips=yes

```
4. Save the file and close Notepad.

This configuration redirects `Node.js` to use FIPS-compliant cryptographic modules and algorithms. The file includes references to the FIPS module configuration and sets default properties to enforce FIPS compliance across all cryptographic operations.


#### Option 2: Using the Cribl CLI (Post-Installation)
Alternatively, after installing Cribl Edge, you can generate the configuration file using the Cribl CLI:

`cribl generateFipsConf -d <fully qualified path to fipsmodule.cnf>`

You can generate the `nodejs.cnf` configuration once on any machine and then distribute it to others, eliminating the need to repeat this step on each individual machine.

## Install Edge with FIPS Configuration {#install}

To install Cribl Edge with FIPS mode enabled:

1. Use the `SETENVVARS` parameter with the MSI installer:

```
msiexec /i CriblEdge-<version>.msi SETENVVARS="OPENSSL_MODULES=C:\Program Files\Cribl-FIPS|OPENSSL_CONF=C:\Program Files\Cribl-FIPS\nodejs.cnf"
```
2. This opens the Windows Installer Wizard. Choose `Managed Mode` and complete the configuration to connect to the FIPS-enabled Leader.

When executing the MSI installer command, pay attention to these important parameter requirements:

* The pipe character (`|`) separates multiple environment variables.
* Make sure to enclose the entire `SETENVVARS` string in quotes.
* `OPENSSL_MODULES` points to the directory containing the provider files.
* `OPENSSL_CONF` points to the specific configuration file.

Alternatively, you can set the environment variables through the Installation Wizard:

![Set Environment Variables](environment-variables.png)
{border="true"} 

For details, see [Installing Cribl Edge on Windows](deploy-windows).


## Confirm Cribl Edge is in FIPS Mode

After installation, validate that Edge is running in FIPS mode by checking for FIPS initialization messages:

1. On the Leader or Edge Node, search `cribl.log` for this message:

 ```
   "level":"info","message":"running with FIPS enabled", "OpenSSLVersion":"3.0.13+quic","OpenSSLFIPSProviderVersion":"3.0.8"
```
2. In the UI, navigate to: **Node Settings** > **Diagnostics** > **System Info**.

The presence of the above message confirms that Cribl Edge is in FIPS mode.


## Troubleshooting

If Edge fails to start after FIPS configuration:

- Verify that all required files exist in the specified locations.
- Check that the `nodejs.cnf` properly references the location of `fipsmodule.cnf`.
- Review Edge logs for specific error messages related to FIPS initialization.
