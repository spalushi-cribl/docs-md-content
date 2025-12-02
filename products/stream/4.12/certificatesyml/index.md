# certificates.yml


`certificates.yml` maintains a list of configured certificates and their parameters.

```yaml {title="$CRIBL_HOME/local/cribl/certificates.yml"}
certificate_id: # [object] 
  description: # [string] Description - Brief description of this certificate. Optional.
  cert: # [string] Certificate - Drag/drop or upload host certificate, in PEM/Base64 format. Or paste its contents here.
  privKey: # [string] Private key - Certificate private key.
  passphrase: # [string] Passphrase - Passphrase. Optional.
  ca: # [string] CA certificate - Optionally, drag/drop or upload all CA certificate(s) in PEM/Base64 format. Or paste certs' contents here. Certs can be used for client and/or server auth.
  inUse: # [array of strings] Referenced - List of configurations referencing this certificate.
```
