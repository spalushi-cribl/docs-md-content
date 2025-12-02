# Cribl Expressions


Native Cribl Stream methods can be found under `C.*`, and can be invoked from any Function that allows for expression evaluations. For example, to create a field that is the SHA1 of a another field's value, you can use the Eval Function with this **Evaluate Fields** pair:

Name | Value Expression
--- | ---
myNewField | `C.Mask.sha1(myOtherField)`

> Where fields' names contain special characters, you can reference them using the `__e['<field‑name‑here>']` convention. For details, see [Build Custom Logic to Route and Process Your Data: Fields with Non-Alphanumeric Characters](filter-and-transform-data#special-chars).
>
{.box .success}

Cribl expressions offer methods and properties in the following classes:

- [C.Crypto](expressions-crypto) - data encryption and decryption
- [C.Encode](expressions-encode-decode#encode) and [C.Decode](expressions-encode-decode#decode) - encoding and decoding data
- [C.Lookup](expressions-lookup) - running lookups
- [C.Mask](expressions-mask) - masking sensitive data
- [C.Net](expressions-net) - network methods
- [C.Text](expressions-text) - text manipulation
- [C.Time](expressions-time) - time and date manipulation
- [C.Schema](expressions-other#schema) - schema validation
- [C.Secret](expressions-other#secret) - secret management
- [C.env](expressions-other#env) - environment properties
- [C.os](expressions-other#os) - system methods
- [C.vars](expressions-other#vars) - global variables
- [C.version](expressions-other#version) - Cribl Stream versions
- [C.Misc](expressions-other#misc) - various other methods
