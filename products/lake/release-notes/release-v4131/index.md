# Cribl Lake 4.13.1


Cribl Lake 4.13.1 is a maintenance release, including the following change. 



## AWS SDK Update from Sunsetting v2 to v3

AWS will end support for their
[AWS SDK for JavaScript v2](https://aws.amazon.com/blogs/developer/announcing-end-of-support-for-aws-sdk-for-javascript-v2/)
on September 8, 2025. The [Cribl Stream](/streamsearch/release-notes/release-v4131/) Destination and Collector for Cribl Lake use this SDK to exchange data with Cribl Lake. To ensure uninterrupted operation and compatibility, we have updated our SDK to v3 in this release, and we will completely remove the v2 SDK in our September 2025 release. Based on AWS’ public communications and our testing, Cribl expects this update to have no impact on Cribl Lake users.

