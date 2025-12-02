# Scaling Cribl in Amazon Fargate


Cribl, as of version 1.4, is a stateless processing engine. We have customers who have a need to scale Cribl into the 10s of Terabytes a day and the hundreds of thousands of events per second. Because we are stateless, scaling Cribl is a matter of throwing more cores at the problem. The simplest method we have found to manage scaling Cribl is to use Amazon Fargate, paired with Application and Network Load Balancers.

## Reference Architecture Overview

First, let's look at a high-level diagram of our deployment. For this deployment, we want to support data coming from Splunk's Heavyweight Forwarders, or from agents which normally send data to Elastic, like Beats, Logstash, or Fluentd. Splunk's protocol is implemented on top of just TCP, whereas Elastic uses its Bulk Ingestion API to move data and sends data over HTTP. To support both, we'll need two different load balancers: a Network Load Balancer for TCP, and an Application Load Balancer for HTTP, which will be backed by two separate ECS Services.

![](Cribl-ECS-Reference-Architecture-(7).8739b9c5c1.png)

Each of these ECS Services is set to auto scale. Since we're using Fargate, we do not need to maintain the underlying EC2 instances, so each service can easily scale independently based on load. Configurations for each service are identical, copied from a single instance of Cribl and stored in S3 for each worker to pick up.

## Cribl Configuration

Cribl configurations are simply YAML files on the filesystem. For our reference architecture, containers will pull data from a location in S3 at startup. You should create a S3 bucket and upload your Cribl configurations to it. Building a working configuration for Cribl is out the scope of this article.  As a simple base configuration which takes data in on S2S and HTTP, [see this example repo](https://github.com/criblio/cribl-ecs-refarch/tree/master/configs). Upload this to a S3 bucket which you will grant Fargate access to read these configurations. Any S3 bucket should do, although we recommend creating one specifically for this POC (out of scope for this article).

## Naming Convention

For the people coming along after you, we recommend a naming convention for the items we're going to provision as part of this reference architecture. For all of our examples, we're using a prefix of `cribl-ecsra` which is short for ECS Reference Architecture.

## Load Balancers

Before we provision the ECS cluster, we need to provision the load balancers. We are going to create two Load Balancers, a Network Load Balancer and an Application Load Balancer. First, let's walk through creating the Network Load Balancer. 

1. In AWS, navigate to EC2
2. On the left select Load Balancers. 
3. From the 3 options of Load Balancer, choose Network Load Balancer. You should find yourself at a screen like this:

![](Screenshot-2019-02-14-20.54.40.c13beb912f.png)

4. For the name, we entered `cribl-ecsra-s2s`. S2S is the name of the protocol Splunk uses between instances of Splunk, like the Forwarders and the Indexers. 
5. For the Load Balancer Port, we'll use `9997`, which is the standard convention for Splunk. 

Selection of the VPC is important and situational. To be internet facing, the NLB and the ALB will require being placed in Subnets which have an Internet Gateway. This is not a requirement for internal load balancers. In our example, the Cribl Fargate containers will be running in Subnets which are behind a NAT gateway. For each Availability Zone, the NLB and ALB are placed in a different subnet from the actual containers running Cribl, but inside the VPC routing allows them to communicate with each other. 

6. For this example, we've selected two subnets, covering two different availability zones which are in the same VPC I will be provisioning the Fargate containers.
7. Click Next
8. You can skip the Configure Security Settings screen as there is nothing to configure there. Click Next again. 

![](Screenshot-2019-02-14-20.55.29.f20ff55ef7.png)

9. For "Target group", Select "New target group"
10. For "Name", we called it `cribl-ecsra-s2s-tg`. 
11. For "Target Type", *Important*, needs to be set to `IP` for Fargate Containers. 
12. For Port, enter `9997`
13. Click Next, 
14. In "Register Targets" we're not going to register any targets manually, those will be done automatically by ECS, so click "Next" 

Review should look similar to the below.

![](Screenshot-2019-02-14-20.57.12.0380c2509d.png)

15. Click Create 

Next, we will move on to creating the Application Load Balancer. 

1. In EC2, you should be back on the load balancer screen, and click "Create".
2. Choose Application Load Balancer, and we should find ourselves on a screen like below.

![](Screenshot-2019-02-14-20.58.34.3161fbf844.png)

3. For "Name", we entered `cribl-ecsra-http`. 
4. Selection of internet facing or internal should mirror your prior choice for the NLB
5. For "Port", set to `10080`. 
6. Select the same subnets as the Network Load Balancer
7. Click "Next"
8. We can skip past "Configure Security Settings". Click "Next"

![](Screenshot-2019-02-14-20.59.28.505f417ebe.png)

It's unclear why Network Load Balancers do not also have security group settings, but the Application Load Balancer allows you to configure a Security Group for the ALB. 

9. Select "Create a new security group"
10. For "Security group name", we entered `cribl-ecsra-http-sg` 
11. Select "Custom TCP" and enter `10080` in the port range, and select "From Anywhere" in Source
12. Click "Next"

![](Screenshot-2019-02-14-21.06.35.74577b1c9a.png)

This should look very similar to the NLB target group settings. 

13. For "Target group", select "New target group"
14. For "Name", we entered `cribl-ecsra-http-tg`
15. For "Target Type", *Important*, needs to be set to `IP` for Fargate Containers. 
16. For "Port", set to `10080` 
17. For "health check", select `HTTP`
18. For "Path", enter `/api/v1/health`, Cribl's health endpoint
19. Click "Next"
20. Skip past putting anything in the target group again, as ECS will put items in the target group for us. Click "Next"

![](Screenshot-2019-02-14-21.07.11.82bd3ed82b.png)

Your review screen should look similar to the above.

21. Click "Create"

## IAM Policy & Role

Next, we need to define the security policy for our Fargate tasks. Our tasks will need two sets of permissions. The first, which is standard for all ECS tasks, is the `ecsTaskExecutionRole` which allows the tasks to query ECS APIs and pull down docker images from ECR. The second, which is custom to our use case, is access to an S3 bucket to pull down Cribl configurations.

First, we're going to create a policy which contains the permissions our ECS tasks will need. 
1. In AWS, navigate to the IAM Service
2. Select Policies
3. Click "Create Policy"
4. Click the "JSON" tab
5. Modify the following JSON to replace YOURBUCKET with your S3 bucket

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutAnalyticsConfiguration",
                "s3:GetObjectVersionTagging",
                "s3:CreateBucket",
                "s3:ReplicateObject",
                "s3:GetObjectAcl",
                "s3:DeleteBucketWebsite",
                "s3:PutLifecycleConfiguration",
                "s3:GetObjectVersionAcl",
                "s3:PutObjectTagging",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:GetBucketPolicyStatus",
                "s3:GetBucketWebsite",
                "s3:PutReplicationConfiguration",
                "s3:DeleteObjectVersionTagging",
                "s3:GetBucketNotification",
                "s3:PutBucketCORS",
                "s3:GetReplicationConfiguration",
                "s3:ListMultipartUploadParts",
                "s3:PutObject",
                "s3:GetObject",
                "s3:PutBucketNotification",
                "s3:PutBucketLogging",
                "s3:GetAnalyticsConfiguration",
                "s3:GetObjectVersionForReplication",
                "s3:GetLifecycleConfiguration",
                "s3:ListBucketByTags",
                "s3:GetInventoryConfiguration",
                "s3:GetBucketTagging",
                "s3:PutAccelerateConfiguration",
                "s3:DeleteObjectVersion",
                "s3:GetBucketLogging",
                "s3:ListBucketVersions",
                "s3:ReplicateTags",
                "s3:RestoreObject",
                "s3:ListBucket",
                "s3:GetAccelerateConfiguration",
                "s3:GetBucketPolicy",
                "s3:PutEncryptionConfiguration",
                "s3:GetEncryptionConfiguration",
                "s3:GetObjectVersionTorrent",
                "s3:AbortMultipartUpload",
                "s3:PutBucketTagging",
                "s3:GetBucketRequestPayment",
                "s3:GetObjectTagging",
                "s3:GetMetricsConfiguration",
                "s3:DeleteBucket",
                "s3:PutBucketVersioning",
                "s3:GetBucketPublicAccessBlock",
                "s3:ListBucketMultipartUploads",
                "s3:PutMetricsConfiguration",
                "s3:PutObjectVersionTagging",
                "s3:GetBucketVersioning",
                "s3:GetBucketAcl",
                "s3:PutInventoryConfiguration",
                "s3:GetObjectTorrent",
                "s3:PutBucketWebsite",
                "s3:PutBucketRequestPayment",
                "s3:GetBucketCORS",
                "s3:GetBucketLocation",
                "s3:ReplicateDelete",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::YOURBUCKET",
                "arn:aws:s3:::YOURBUCKET*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetAccountPublicAccessBlock",
                "s3:ListAllMyBuckets",
                "s3:HeadBucket"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

6. Click "Review Policy"
7. We named ours `criblECSRATaskPolicy`
8. Click "Create Policy"

As with all things security related, please audit our IAM configuration. We built our IAM configuration with AWS's visual editor to allow full access to the S3 bucket and include the stock `ecsTaskExecutionRole` permissions all in the same policy.

Next, we need to create an IAM role which uses this policy.

1. Select "Roles" at the left
2. Click "Create Role"
3. Click "AWS Service"
4. Click "Elastic Container Service"
5. Click "Elastic Container Service Task"
6. Click "Next: Permissions"
7. Select the Policy you just created, ours was named `criblECSRATaskPolicy`
8. Click "Next: Tags"
9. We did not create any tags, but optionally you can. Click "Next: Review"
10. Enter a role name. We called ours `criblECSRATaskRole`
11. Click "Create Role"

## Fargate Cluster

Now, let's create our Fargate Cluster. Fargate Clusters are very simple when it comes to configuration, almost all of the configuration is done at the Service and Task Definition level. To create the cluster:

1. Navigate to the Elastic Container Service (ECS) in the AWS UI
2. "Create Cluster". Select "Networking only".
3. Click "Next Step".
4. Enter a cluster name. We entered `cribl-ecsra`.
5. Click Create

## Fargate Task Definition

In ECS, the Task Definition defines how to run a container. Services can contain multiple Tasks. Task definitions are versioned, and Services reference a specific version of the Task definition when running. The Task Definition will tell what container to run, set environment variables to configure its running parameters, and set limits on what resources we expect the Task to consume. 

1. On the left hand nav, click "Task Definitions".
2. Click "Create New Task Definition"
3. Choose "Fargate" under "Select launch type compability"
4. Click Next Step

![](Screenshot-2019-02-16-11.33.40.fb57ec8d4e.png)

5. For Task Definition Name, enter a meaningful name
6. For Task Role, select the IAM role you created earlier
7. For Task Execution Role, select the IAM role you created earlier

![](Screenshot-2019-02-16-11.34.04.01c6cd57d6.png)

8. For Task memory, select 2GB
9. For Task CPU, select 1 vCPU
10. Under Container Definitions, click "Add Container"

![](Screenshot-2019-02-16-11.36.34.df68f3d3ae.png)

11. For Container Name, input `cribl`
12. For Image, enter `cribl/cribl:latest`. This will pull our official image from Docker Hub.
13. Under Port Mappings, add 3: `9000`, `9997`, and `10080`.

![](Screenshot-2019-02-16-12.25.43.d5d89c7e26.png)

14. Under Environment Variables, add a key `CRIBL_CONFIG_LOCATION` and enter an S3 URL which is the location of the config directory you have uploaded to S3.
15. Click "Create"

## Fargate S2S Service

Now we're finally ready to configure and schedule the compute to run Cribl. We need to create a Fargate Service which will run our Cribl containers. 

1. Navigate to your cluster in ECS, under "Clusters" find your cluster and click on it
2. Under Services, click "Create"

![](Screenshot-2019-02-16-14.19.12.d0e02200fc.png)

3. For Launch Type, select "FARGATE"
4. For Task Definition, select the task definition you created earlier
5. For Cluster, select the cluster you created earlier
6. For Service Name, enter cribl-s2s
7. For Number of Tasks, input how many tasks you would like to schedule. We recommend 1 Task per 200 GB a day of ingestion volume.
8. Click Next Step

![](Screenshot-2019-02-16-14.23.46.8d7025ed31.png)

9. Under Cluster VPC, select the same VPC you selected for the load balancers
10. Under Subnets, select your designed subnets. For our example, we chose subnets which are behind a NAT gateway and do not allow public IPs.
11. Under auto-assign public IP, select your desired value. We chose "DISABLED"
12. Select Edit for Security Groups

![](Screenshot-2019-02-16-14.20.52.617061d24e.png)

13. Choose "Create new security group"
14. Input a security group name. We named it `cribl-s2s`
15. For inbound rules, select "Custom TCP" and input `9997` into "Port Range"
16. Click "Save"

![](Screenshot-2019-02-16-14.28.56.0b72524b61.png)

17. Under "Load Balancing", select "Network Load Balancer"
18. For "Load Balancer Name", select the network load balancer you created earlier.
19. For "Container name: port", select the container you created earlier and click "Add to load balancer"

![](Screenshot-2019-02-16-14.29.18.0368edd810.png)

20. For "Production listener port", click "9997".
21. For "Target group name", select the target group you created earlier.
22. Uncheck "Enable service discovery integration"
23. Click "Next Step"
24. Auto scaling has been tested and works, but it's out of scope for this article. Click "Next Step"
25. Review your settings and click "Create"

## Fargate HTTP Service

Next we need to create our second service for HTTP.

1. Navigate to your cluster in ECS, under "Clusters" find your cluster and click on it
2. Under Services, click "Create"

![](Screenshot-2019-02-16-14.41.22.f59c171974.png)

3. For Launch Type, select "FARGATE"
4. For Task Definition, select the task definition you created earlier
5. For Cluster, select the cluster you created earlier
6. For Service Name, enter cribl-http
7. For Number of Tasks, input how many tasks you would like to schedule. We recommend 1 Task per 200 GB a day of ingestion volume.
8. Click Next Step

![](Screenshot-2019-02-16-14.39.00.ba73d4b7eb.png)

9. Under Cluster VPC, select the same VPC you selected for the load balancers
10. Under Subnets, select your designed subnets. For our example, we chose subnets which are behind a NAT gateway and do not allow public IPs.
11. Under auto-assign public IP, select your desired value. We chose "DISABLED"
12. Select Edit for Security Groups

![](Screenshot-2019-02-16-14.38.41.4fd1670071.png)

13. Choose "Create new security group"
14. Input a security group name. We named it `cribl-http`
15. For inbound rules, select "Custom TCP" and input `10080` into "Port Range"
16. For inbound rules, select "Custom TCP" and input `9000` into "Port Range"
17. Click "Save"

![](Screenshot-2019-02-16-14.43.24.1b2dc03de5.png)

18. Under "Load Balancing", select "Application Load Balancer"
19. For "Load Balancer Name", select the application load balancer you created earlier.
20. For "Container name: port", select the container you created earlier and click "Add to load balancer"

![](Screenshot-2019-02-16-14.43.58.951177f584.png)

21. For "Production listener port", click "10080".
22. For "Target group name", select the target group you created earlier.
23. Uncheck "Enable service discovery integration"
24. Click "Next Step"
25. Auto scaling has been tested and works, but it's out of scope for this article. Click "Next Step"
26. Review your settings and click "Create"

## Update Health Check Port

The API runs on a different port from HTTP input, so we need to manually update the target group to get it to check against the proper port. 

1. Navigate to the EC2 Service
2. Select Load Balancers > Target Groups
3. In the Target Groups list, select the target group for your HTTP service
4. Under Health Checks, click "Edit Health Check"
5. For "Port", click "override"
6. Next to "override", input `9000`. 

Your window should look like the below.

![](Screenshot-2019-02-18-17.14.40.2ae5948863.png)



## Validating Your Deployment

Now that you've got Cribl up and running in ECS, we need to validate we can send some traffic to it. You will a machine inside the VPC where you're doing validation. We did this via VPN, but you could also create an instance with a public IP you could SSH into. Using SSH, it should be easy to create a tunnel to access the UI of any worker node. The UI makes it easy to validate whether traffic is flowing from the UI.

First, we're going to generate some traffic via HTTP. To generate traffic, we're going to use [Gogen](https://github.com/coccyx/gogen). We will also require `curl`, `jq` and the AWS CLI. To install `gogen`:

```shell
sudo curl -Lso /usr/local/bin/gogen https://api.gogen.io/linux/gogen
sudo chmod 755 /usr/local/bin/gogen
```

You can substitute `linux` with `osx` above if you're on a Mac. Now that we have gogen, let's generate some traffic to Cribl instances. Below, customize the `LB_NAME` and `TG_NAME` to match your Load Balancer name and Target Group name respectively. This will use the AWS CLI to get the DNS name of the load balancer and the IP of the first node in the target group. It will generate traffic through the HTTP load balancer and then validate through Cribl's API that traffic has made it to Cribl.

```shell
LB_NAME=cribl-ecsra-http
TG_NAME=${LB_NAME}-tg
TG_ARN=$(aws elbv2 describe-target-groups --name ${TG_NAME} --output text --query "TargetGroups[0].TargetGroupArn")
CRIBL_NODE1_ADDR=$(aws elbv2 describe-target-health --target-group-arn ${TG_ARN} --query "TargetHealthDescriptions[0].Target.Id" --output text)
LB_HTTP_ADDR=$(aws elbv2 describe-load-balancers --names ${LB_NAME} --query 'LoadBalancers[0].DNSName' --output text)
gogen -c coccyx/weblog -o http -ot json --url http://${LB_HTTP_ADDR}:10080/cribl/_bulk -v gen -ei 10 -c 100
CRIBL_TOKEN=$(curl -s -H "Content-Type: application/json" -X POST --data '{"username":"admin","password":"admin"}' http://${CRIBL_NODE1_ADDR}:9000/api/v1/auth/login | jq -r .token)
sleep 10
curl -s -H "Authentication: Bearer ${CRIBL_TOKEN}" http://${CRIBL_NODE1_ADDR}:9000/api/v1/input/splunk/metrics | jq .items[].values.total.inEvents | awk '{total += $0} END{print "sum="total}'
```

Now, let's also run the same test via S2S. Again, customize `LB_NAME` and `TG_NAME` to match your S2S names.

```shell
LB_NAME=cribl-ecsra-s2s
TG_NAME=${LB_NAME}-tg
TG_ARN=$(aws elbv2 describe-target-groups --name ${TG_NAME} --output text --query "TargetGroups[0].TargetGroupArn")
CRIBL_NODE1_ADDR=$(aws elbv2 describe-target-health --target-group-arn ${TG_ARN} --query "TargetHealthDescriptions[0].Target.Id" --output text)
LB_S2S_ADDR=$(aws elbv2 describe-load-balancers --names ${LB_NAME} --query 'LoadBalancers[0].DNSName' --output text)
gogen -c coccyx/weblog -o splunktcp -ot splunktcp --url ${LB_S2S_ADDR}:9997 -v gen -ei 10 -c 100
gogen -c coccyx/weblog -o splunktcp -ot splunktcp --url ${LB_S2S_ADDR}:9997 -v gen -ei 10 -c 100
gogen -c coccyx/weblog -o splunktcp -ot splunktcp --url ${LB_S2S_ADDR}:9997 -v gen -ei 10 -c 100
CRIBL_TOKEN=$(curl -s -H "Content-Type: application/json" -X POST --data '{"username":"admin","password":"admin"}' http://${CRIBL_NODE1_ADDR}:9000/api/v1/auth/login | jq -r .token)
sleep 10
curl -s -H "Authentication: Bearer ${CRIBL_TOKEN}" http://${CRIBL_NODE1_ADDR}:9000/api/v1/input/splunk/metrics | jq .items[].values.total.inEvents | awk '{total += $0} END{print "sum="total}'
```



## Out of Scope Things to Consider

There are several things most customers would want to implement that we are expressly leaving out:

* Autoscaling
* SSL Configuration for HTTP and S2S
* How to update running configs across the cluster and signal to the cluster to pull new configs
