# 


|Term|Definition|
|----|----------|
|**Protect**|The toggle on the Cribl Guard homepage that adds a Post-Processing Pipeline to the target Destination with the Cribl Guard Function. Credit consumption begins as soon as data flows through the Cribl Guard Function.|
|**Scanning Rulesets**|In the Cribl Guard Function, scanning rulesets are combinations of rules and anchor terms that identify sensitive data in your event streams.|
|**Pattern to match**|The regular expression pattern that identifies strictly on patterns in your event data. For example, a pattern to match on credit cards would look for strings of numbers between 0-9, including hyphens or periods, in a specific order.|
|**Anchor terms**|The optional terms or characters that you can add to Cribl Guard rules. Anchor terms help improve the accuracy of data matching and limit unnecessary data redaction. <ul><li>For example, adding the anchor terms `cc`, `creditcard`, and `visa` could help identify a 16-digit number string as a credit card instead of a similar, 16-digit UUID. Matches are highlighted in the sample events panel.</li><li>You can add up to a maximum of six anchor terms to a Cribl Guard rule.</li></ul>|