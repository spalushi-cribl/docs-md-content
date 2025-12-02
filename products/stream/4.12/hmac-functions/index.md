# HMAC Functions


HMAC (Hash-based Message Authentication Code) Functions are a crucial security feature used to ensure the integrity and authenticity of messages. In Cribl, HMAC Functions are used with REST Collectors, where they secure data collection by generating a unique signature for each request. This signature is then verified by the server to ensure that the request has not been tampered with and is from a trusted source.

## Why Use HMAC Functions?

- **Security**: HMAC Functions provide a way to verify the integrity and authenticity of messages, ensuring that data has not been altered in transit.
- **Authentication**: They help authenticate the source of the message, ensuring that it comes from a trusted entity.
- **Compliance**: Using HMAC Functions can help meet security compliance requirements by adding an additional layer of security to your data collection processes.

## Where to Use HMAC Functions

HMAC Functions are used in [REST Collectors](collectors-rest). They are configured to generate a signature for each request, which is then included in the request headers. The server receiving the request can then verify the signature to ensure the request's integrity and authenticity.

For a practical example of how to use HMAC functions, refer to the [Cribl REST Collector Configuration for the Duo API](https://github.com/criblio/collector-templates/tree/main/collectors/rest/duo) in Cribl’s Collector Templates repository.

## Creating an HMAC Function 

To create an HMAC Function: 

1. In the sidebar, select **Worker Groups** and choose a Worker Group.
1. On the **Worker Groups** submenu, select **Processing**, then **Knowledge**, then **HMAC Functions**.
1. Select **Add HMAC Function**. 
1. Enter a unique **ID**.
1. Optionally, enter a **Description** and assign **Tags** that you can use to filter and group HMAC Functions. 
1. Define a list of **HMAC signature strings** to evaluate and concatenate to form the HMAC signature string. This specifies the components of the request to include in the HMAC signature. For example, you might include the HTTP method, URL, and certain headers to ensure that the signature is unique and secure. Expressions returned as undefined or null are ignored and not added to the final signature string. Each string can access the following variables:
   - `method`: Uppercase HTTP verb used for the request: GET, PUT, POST, DELETE, and so on.
   - `urlObj`: A URL object containing details of the URL and query string passed with the request.
   - `headers`: An alphabetically ordered list of headers (by name) and values to send with the request.
   - `body`: The contents of the HTTP body (if present) passed with the request.
1. Specify the **Signature string delimiter** character to use when joining the HMAC signature strings for evaluation. For example, `':'`, `'-'`, and `'\n'`. The value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Leave blank to join without spaces.
1. Specify the **Authorization header name** that will contain the HMAC signature. Defaults to `signature`. This ensures the server knows where to find the signature in the request headers.
1. Define an **Authorization header expression** that forms the HMAC signature. The expression should incorporate the following:
   - `signatureString`: This contains the concatenated result of the HMAC signature strings.
   - HMAC generation: Use the [`C.Crypto.createHmac`](expressions-crypto#ccryptocreatehmac) function to calculate the HMAC signature based on the `signatureString`, a specified secret (either as a [text secret](securing-secrets) or inline), the SHA256 algorithm, and hexadecimal encoding. 
      
   For example: ``${C.Crypto.createHmac(`${signatureString}`, C.Secret('myTextSecret', 'text').value, 'sha256', 'hex')}``
1. Select **Save**, then **Commit** and **Deploy** your changes.

