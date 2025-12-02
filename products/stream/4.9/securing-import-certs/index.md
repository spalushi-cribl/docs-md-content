# Secure Leader Using an Existing TLS/SSL Certificate and Key


This page outlines how to protect Cribl Stream Leader to Worker Nodes communications, using an existing TLS/SSL certificate and key. 

Cribl Stream expects certificates and keys to be formatted in privacy-enhanced mail (`.pem`) format. To generate a self-signed certificate and corresponding key, see [Securing Cribl Stream](securing-ssl).

## Import Certificate and Key {#upload-certificate}

To use your certificate and key to prepare secure communications between Workers/Edge Nodes and the Leader:

1. Navigate to the Leader's **Settings** > **Globals** > **Security** > **Certificates** > **New Certificates** modal. 
2. Open your TLS/SSL certificate file. (The self-signed certificate example in [Configure TLS for API and UI Access](securing-tls) used the placeholder name `myCert.pem`.)
3. Copy the file's contents to your clipboard.
4. Paste the file's contents into the modal's **Certificate** field.

  > You can skip the preceding three steps: Just drag/drop your `.pem` file from your filesystem into the **Certificate** field, or upload it using the button at the field's upper right.
  {.box .success}

5. Open your private key file. (The self-signed certificate example in [Configure TLS for API and UI Access](securing-tls) used the placeholder name `myKey.pem`.)
6. Copy the file's contents to your clipboard.
7. Paste the clipboard contents into the same Leader modal's **Private key** field.

  > Here again, you can skip the preceding three steps by dragging/dropping or uploading the `.pem` file from your filesystem.
  {.box .success}

8. Fill in the **Name** and **Description** fields. 
9. If you've uploaded a self-signed certificate, just **Save** it now.
10. If your private key is encrypted, fill in the modal's **Passphrase** with the corresponding key.  
(You can paste the key's contents, or you can drag/drop or upload the key file.)
11. If you're uploading a certificate signed by an external certificate authority – For example, a downloaded Splunk Cloud certificate – import the chain into the **CA certificate** field before saving the certifcate. For details, see [Obtain the Certificate Chain (TLS/SSL)](/stream/git-certificate-errors#chain).


