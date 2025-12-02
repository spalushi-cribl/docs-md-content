# Searching AWS API


Learn how to search your AWS data.

---

Cribl Search supports searching [AWS API](https://docs.aws.amazon.com/) endpoints. You need a Dataset Provider and a
Dataset, see the setup guide for [AWS API](set-up-aws) for details.

This document provides examples of searching endpoints from the AWS API. To start, navigate to **Search Home**, where [searches](build-a-search) are run.

## Account and Endpoint Identifiers {#account-endpoint}

When you search the AWS API, events in the results will contain `accountName` and `endpointName` fields, which reflect
the account and endpoint configured on the Dataset.

If your Dataset has multiple accounts and/or endpoints enabled, these fields distinguish each event's source. They also
serve as potential filters that you can express in predicates, as you’ll see in examples below.

## Examples {#examples}

The following examples reference a Dataset with the **ID** `aws_dataset1_a`.

### EC2 Security Groups API Search

This search interrogates the `ec1_security_groups` endpoint for number of Security Groups that allow inbound to port
`22` from `0.0.0.0/0`, grouped by `VPCID`?

```kusto
dataset="aws_dataset1_a" endpointName="ec2_security_groups" IpPermissions[0].ToPort=22 IpPermission[0].IpRanges[0].CidrIp=`0.0.0.0/0'
| summarize Groups= count(GroupName), GroupsIds=count(GroupId) by VpcId
```


![Sample AWS Search: EC2 Security Groups](search-aws-basic-search.png)
{scale="100%"}

### EC2 Instances API Search

This search hits the `ec2_instances` API endpoints to find out top 5 most commonly used Amazon Machine Images (AMIs) and
summarizes the results by `ImageId`.

```kusto
dataset="aws_dataset1_a" endpointName="ec2_instances"
| summarize count () by ImageId
| where count >= 5
| order by count_desc
```


![Sample AWS Search: EC2 Instances](search-aws-basic-search2.png)
{scale="100%"}

Cribl Search comes with rich [charting options](Charting) out of the box, allowing you to adjust Charts as needed.

