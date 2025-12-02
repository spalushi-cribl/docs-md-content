# Cribl Edge 4.0.2


## Corrections 

This release includes the following fix:

- CRIBL-14160 On the Google Cloud Pub/Sub Source and Destination, we've rolled back validation of **Topic ID** and **Subscription ID** field values. (For users who entered a full path rather than just an ID substring in these fields, this strict validation broke the integration and broke the UI.) 