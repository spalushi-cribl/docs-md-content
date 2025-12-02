# Git Remote Repos & Trusted CAs


If you are using an internal Git server, a self-signed certificate might prevent Cribl Stream from successfully pushing commits to the origin. You might see errors like these when pushing (or pulling) via the CLI:

```shell
SSL certificate problem: self signed certificate in certificate chain
SSL certificate problem: unable to get local issuer certificate
```

## Resolving the Errors

To ensure that Git trusts your self-signed certificate, follow these steps:

1.  [Obtain the certificate chain](#chain) (root, intermediates, and leaf) for the Git server.
    
2.  As the `cribl` user, run this command: 
  `git config http.sslCAInfo /path/to/certs.pem`
    
3.  Test with this command: 
  `git push origin`
  Verify that this throws no errors.

##  Obtain the Certificate Chain (TLS/SSL)  {#chain}

Use these steps to enable Worker-to-Leader mutual authentication:

### A. Validate the Client Certs

If you are using an internal certificate authority, obtain a copy of the CA public certificate, then add it to `/etc/systemd/system/cribl.service`:

```shell
...
[Service]
Environment="NODE_EXTRA_CA_CERTS=/opt/cribl/local/cribl/auth/certs/ca.pem"
...
```

For details, see [CA Certificates and Environment Variables](securing-tls-overview#env-vars).

### B. Simplify the Common-Name Regex

The common-name regex (if required) should omit the `CN=` at the beginning of the **Common Name** field. The example below will match all immediate subdomains of `se.lab.cribl.io`, like `madsci.se.lab.cribl.io`.

If you disable **Validate Client Certs**, Cribl Stream will match only on common names.

![Common Name example](tls-cn-example.48c78674ef.png)

### C. Extract SSL Certificate Info

As in this example:

```shell
openssl x509 -in certificate.pem -text -noout
```

### D. Dump the Certificate Chain from the Server

As in this example:

```shell
echo "" | openssl s_client -host www.google.com -port 443 -showcerts 2>&1 | sed -n '/BEGIN CERTIFICATE/,/END CERTIFICATE/p'
```
