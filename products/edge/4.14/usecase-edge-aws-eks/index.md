# Amazon Elastic Kubernetes Services (EKS) Add-On for Edge


Use Amazon Elastic Kubernetes Services Add-On to track cost of Kubernetes application in the AWS Cloud.

---

Amazon Elastic Kubernetes Services [Amazon EKS](https://aws.amazon.com/eks/) is a managed container service
to run and scale Kubernetes applications in the AWS Cloud.

Cribl Edge provides an optimized bundle for Amazon EKS cluster cost visibility.
This means you can accurately track costs by namespace, cluster, Pod, or by
organizational concepts such as team or application. Kubernetes platform
administrators and finance leaders can use Cribl Edge to visualize a breakdown
of their Amazon EKS cluster charges. This can help them to allocate costs and
chargeback organizational units such as application teams.

You can use your existing AWS support agreement to obtain support.

## In This Article

In this article, you will learn more about how the Amazon EKS architecture
interacts with Cribl Edge. You will also learn how to deploy Cribl Edge on EKS using
one of two different methods: 

1. Deploy Cribl Edge on an Amazon EKS cluster using the Amazon EKS add-on. 
1. Deploy Cribl Edge on an Amazon EKS cluster via AWS CLI.

## Architecture Overview

![Amazon EKS architecture on Edge](edge-aws-eks-arch.cc6e3b040e.png)
{scale="70%" border="false"}

## Deploy Cribl Edge on an Amazon EKS Cluster

This section covers how to deploy Edge on an AWS EKS cluster.

### Prerequisites

To deploy Edge on an Amazon EKS cluster using the Amazon EKS add-on, make sure you
have:

- A Cribl.Cloud deployment or a self-hosted Cribl Leader.
- The ability to subscribe to Cribl Edge on AWS Marketplace.
- `kubectl`, [AWS CLI](https://aws.amazon.com/cli/), and (optional) `eksctl` installed on your machine.
- Access to an [Amazon EKS Cluster](https://aws.amazon.com/eks/).

### Set up the Cribl Edge API Server 

1. Log into your [Cribl Deployment](https://docs.cribl.io/stream/deploy-planning/)
   in a self-hosted deployment, or log into Cribl.Cloud, and select **Manage Edge**.
1. In Edge, select the Fleet for which you want to access deployment
   details.

> If you have Fleets running Edge Nodes with version 4.8.2 and older,
> you can encounter failing health checks that require specific API server configuration.
> See [Common Challenges on the Edge](edge-common-challenges#failing-health-checks) for more information.
{.box .info}

#### Access the Cribl URI and Token

To enable your Cribl Edge add-on for EKS, you'll need the Cribl URI and token.
You can collect it from the Kubernetes script in Edge:

1. In Edge, select a Fleet to configure. 
1. Select **Add/Update Edge Node** and select **Kubernetes**.
1. In the **Script** field, copy the `cribl.leader` URI, starting at `tls:`. This contains the auth
   token and the organization name for your deployment.

   Example URI:
  
   `tls://<auth_token>@<cribl_org_id>.cribl.cloud?group=<default_fleet>`

1. Save this URI somewhere you can easily access it in next steps.

### Enable the Cribl Edge Add-On from EKS Console

To get started, access your Amazon EKS console and open your EKS clusters.

1. In the **Add-ons** tab, select **Get more add-ons**.
1. Type `Cribl Edge` into the search bar.
1. Follow the onscreen instructions to enable the Cribl Edge add-on for your
   Amazon EKS cluster. Learn more about direct deployment to Amazon EKS clusters
   from this [AWS blog post](https://aws.amazon.com/blogs/aws/new-aws-marketplace-for-containers-now-supports-direct-deployment-to-amazon-eks-clusters/). 

#### Configure AWS EKS Settings

After you enable the Cribl Edge add-on, configure its settings:

1. Under **Version**, select the latest version.
1. Under **Select IAM role**, select **Inherit from node**.
1. Twirl open **Optional configuration settings** and navigate to **Configuration values**.
1. Insert the following JSON configuration:

    ```json
    {
     "cribl": {
         "leader": "tls://<token>@<cribl_cloud_leader>.cribl.cloud?group=<default_fleet>"
     }
    }
   ```
1. Replace the `leader` value with the URI for your Edge Leader gathered in 
  [Access the Cribl URI and Token](#access-the-cribl-uri-and-token).

   - For self-hosted Cribl deployments, enter the publicly available URI and token
     for your Leader Node.
1. In **Conflict resolution method**, select **None**.

## Enable Cribl Edge Add-on Using AWS CLI

You can also enable the Cribl Edge add-on via CLI.

Enter this on the command line:

`aws eks create-addon --addon-name cribl_cribledge --cluster-name $YOUR_CLUSTER_NAME --region $AWS_REGION --configuration-values file://path_to_cribl_leader.json`

Example output:

```JSON
{
    "addon": {
        "addonName": "cribl_cribledge",
        "clusterName": "$YOUR_CLUSTER_NAME",
        "status": "CREATING",
        "addonVersion": " v4.3.1-eksbuild.1",
        "health": {
            "issues": []
        },
        "addonArn": "arn:aws:eks:$AWS_REGION:xxxxxxxxxxxx:addon/$YOUR_CLUSTER_NAME/cribl_cribledge/90c23198-cdd3-b295-c410-xxxxxxxxxxxx",
        "createdAt": "2022-12-01T12:18:26.497000-08:00",
        "modifiedAt": "2022-12-01T12:50:52.222000-08:00",
        "tags": {}
    }
}
```

To monitor the installation status, you can run the following command:

`aws eks describe-addon --addon-name cribl_cribledge --cluster-name $YOUR_CLUSTER_NAME --region $AWS_REGION`

Example output:

```JSON
{
    "addon": {
        "addonName": "cribl_cribledge",
        "clusterName": "cribl",
        "status": "ACTIVE",
        "addonVersion": "v4.3.1-eksbuild.1",
        "health": {
            "issues": []
        },
        "addonArn": "arn:aws:eks:$AWS_REGION:xxxxxxxxxxxx:addon/cribl/cribl_cribledge/3ec5da22-97dd-e492-22d8-xxxxxxxxxxxx",
        "createdAt": "2023-11-09T08:28:35.212000-05:00",
        "modifiedAt": "2023-11-09T08:29:44.401000-05:00",
        "tags": {},
        "configurationValues": "{\r\n\t\"cribl\": {\r\n\t     \"leader\": \"tls://<cribl_token>@<cribl_leader>?group=<cribl_group>\"\r\n\t}\r\n}"
    }
}
```

### Disable the Cribl Edge add-on

To disable the Cribl Edge add-on, you can run the following command:

`aws eks delete-addon --addon-name cribl_cribledge --cluster-name $YOUR_CLUSTER_NAME –region $AWS_REGION`

## More Information

If you need more help, see [Amazon EKS Add-Ons](https://docs.aws.amazon.com/eks/latest/userguide/eks-add-ons.html) documentation.

