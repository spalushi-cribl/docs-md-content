# Use Cribl Products to Remove or Disguise Sensitive Data


When you need to handle sensitive data responsibly, you can use both Cribl
Stream and Cribl Edge to anonymize personally identifiable information (PII)
through redaction, obfuscation, and encryption.

* **Redaction** and **obfuscation** allow you to anonymize sensitive information without
  compromising the analytical value of the data. 
* **Encryption** keeps data readable only by authorized users or systems. 

Together, these tools can help your organization manage PII, maintain data
privacy, and ensure compliance with data protection regulations, such as GDPR or
HIPAA, during data transit and storage.

To remove or disguise sensitive data to comply with regulations and security
standards, look into the following Cribl capabilities.

## Selectively Redact Sensitive Data

You can use the **Mask** Function to selectively redact sensitive data in Cribl
Stream and Cribl Edge. You can specify patterns that match PII, such as social
security numbers or credit card information, and completely remove or replace
them with fixed characters. For examples, read our docs on the
[Mask](/edge/mask-function/#mask) Function.

## Replace Personal Information with Obfuscated Values

You can use the **Mask** and **Eval** Functions for data privacy. These
Functions allow you to replace PII with obfuscated values. See our example doc
on how to [mask sensitive data](/edge/mask-function/#example-2-mask-sensitive-data).

## Encrypt Specific Fields 

With Cribl Stream, you can encrypt your sensitive data in real time before it’s
forwarded to and stored at a destination. Using the out-of-the-box
[Mask](/stream/mask-function/) Function, you can define patterns to encrypt with
specific key IDs or key classes.

You can manage encryption keys within Cribl Stream or Cribl Edge, and decryption
can happen either in Cribl (if the data flows back through it) or in the end
systems. This allows for secure data processing without exposing sensitive
information.

For a more detailed walkthrough and configuration examples, refer to [Encrypting Sensitive Data](/stream/usecase-encrypting-data/).

To encrypt data using Cribl:

1. Define encryption keys in the key management interface and associate them
   with key classes for organization.
1. Apply the Mask Function to encrypt data based on patterns. You can define
   patterns to target specific fields for encryption using the
   `C.Crypto.encrypt()` expression. The **Mask** Function configuration allows
   you to specify:
    * **Match Regex:** A regular expression to identify the data pattern you
      want to encrypt within a field.
    * **Replace Expression:** A JavaScript expression or literal that defines
      how the matched content should be replaced. In this case, you'll use `C.
      Crypto.encrypt()` to encrypt the matched data using the defined key.
1. Optionally, decrypt data on the receiving end. 

You can also use selective data routing, local processing, and field processing
at the Source to maintain data integrity. Consider these options:

* **Selective data routing:** Configure routing rules within Cribl Stream or
  Cribl Edge to send data to different Destinations based on the data
  sensitivity. For example, you could route data containing PII to a Destination
  with higher security controls, while sending other, non-sensitive data to
  Destinations with standard controls.
* **Local processing:** By deploying Cribl Edge on or near the data sources
  (such as on the same network or device), you can pre-process and de-identify
  data before it's transferred to central systems. This includes stripping out
  PII, masking it, or applying other transformations to ensure that sensitive
  data is properly handled in accordance with regulatory requirements.
* **Field processing at the source:** Using Cribl Edge parsing and
  transformation capabilities, perform real-time processing on the source device
  to remove or transform PII before it ever leaves the Edge Node. This is
  particularly beneficial for devices that may be deployed in remote or
  distributed environments, helping to maintain data sovereignty and reduce
  compliance risks.
  
## Sample Workflow to Enhance Data Privacy

You can enhance data privacy and compliance across your IT ecosystem by
incorporating the security and data routing Functions of Cribl Stream with the
local processing and field processing at the source capabilities of Cribl Edge.

Here's a sample workflow:

### Process Local Data with Cribl Edge

1. Deploy Cribl Edge close to the data sources, such as on network devices or
   directly on servers that generate logs.
1. Configure Cribl Edge to identify and parse logs in real time. Leverage the
   parsing capabilities to detect and extract structured data from the raw logs.
1. Apply initial transformations and data protections at the Edge Node:
    * Mask PII such as social security numbers or credit cards using pattern
      matching to ensure you're anonymizing sensitive information before leaving the
      source.
    * Conduct field-level redaction to remove other sensitive elements entirely
      from the dataset if required for compliance.

### Securely Transmit Data to Cribl Stream

1. Transfer the pre-processed and de-identified data from Cribl Edge to Cribl
   Stream using secure communication protocols like Cribl TCP.
1. Establish the connection over encrypted channels to protect the data during
   transit, even though you've already masked PII at the source.

### Advanced Data Processing with Cribl Stream

1. In Cribl Stream, further refine and enrich the data as needed, supplementing
   with any necessary context that was not available at the edge.
1. Apply additional security measures on data fields that may contain residual sensitive information:
    * Use the Mask Function to replace any potential PII with hashed or
      pseudonymized values that maintain operational utility for analytics but
      prevent re-identification.
    * Encrypt specific fields that you can't obfuscate or hash, preparing
      them for secure storage or advanced processing downstream.

### Selective Data Routing and Distribution

1. Categorize the incoming data based on sensitivity and content within Cribl Stream.
1. Establish and configure routing rules to direct data accordingly:
    * Route highly sensitive data that requires encryption or additional
      security controls to secure destinations like a SIEM with high-compliance
      storage solutions.
    * Send less sensitive, already de-identified data to less restricted
      environments for further analysis or long-term storage, such as Cribl Lake
      or other analytics platforms.

This sample workflow illustrates how the Cribl suite treats PII and sensitive
data with the necessary care at the point of generation (in Cribl Edge), in
transit, and throughout the processing lifecycle (within Cribl Stream). The
refined and routed data not only meets privacy and compliance standards but is
also optimized for further analysis and storage in your environment.

> The Cribl Docs team created this content with assistance from AI.
> Using AI-generated text allows us to quickly create expanded content with more detailed scenarios, solutions, and examples. Because AI-generated content isn’t always completely accurate, we review, test, and augment it before publishing.
{.box .info}