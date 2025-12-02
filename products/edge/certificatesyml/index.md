# certificates.yml


`certificates.yml` maintains a list of configured certificates and their parameters.

```yaml {title="certificates.yml"}
# Name - Unique identifier for this certificate
# [string; required]
id:
# Description - Optional description for the certificate
# [string]
description:
# Certificate - Drag/drop or upload host certificate in PEM/Base64 format, or paste its contents
# here
# [string; required]
cert:
# Private key - Certificate private key in PEM format
# [string; required]
privKey:
# Passphrase - Optional passphrase for the private key
# [string]
passphrase:
# CA certificate - Optionally, drag/drop or upload all CA certificates in PEM/Base64 format. Or, paste
# certificate contents here. Certificates can be used for client and/or server authentication.
# [string]
ca:
# Referenced - List of configurations that reference this certificate
inUse:
```