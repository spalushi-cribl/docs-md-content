# Cribl Edge as a Sidecar Container in AWS ECS


You can use Cribl Edge to collect logs from a service or task running in ECS. This is an alternative to forwarding logs to CloudWatch Logs.

> Currently, you can't configure Windows hosts as sidecar containers in AWS.
>
{.box .warning}

## Container Definition

Add a secondary container to the task definition. To configure the container, you must first set the following environment variables:

Name | Value
--- | ---
`CRIBL_DIST_LEADER_URL` | `tcp://<token>@<leader>:4200`
`CRIBL_DIST_MODE` | `managed-edge`
`CRIBL_EDGE` | `1`

![Add Environment Variables to Container Details](ed_aws_usecase1.1987c52e70.png)
{border="true"}

## Configure Storage 

To pass logs between containers, you can use a Bind Mount, which is a special folder mounted between containers. For details, see [Using Data Volumes in Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_data_volumes.html).

Here's an example configuration from the ECS task definition:

![ECS Task Definition Example](ed_aws_usecase2.52e243ab3a.png)
{border="true"}

## Sample JSON Container Definitions

You can adapt the following sample JSON definitions for the container. Note, this is not a full ECS task definition.

```json
{
    "containerDefinitions": [
        {
            "name": "whatever",
            "image": "foo/bar:latest",
            ...
            "mountPoints": [
                {
                    "sourceVolume": "logs4edge",
                    "containerPath": "/var/log",
                    "readOnly": false
                }
            ],
            "volumesFrom": [],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-create-group": "true",
                    "awslogs-group": "/ecs/cribl-leader-plus-edge",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "ecs"
                }
            }
        },
        {
            "name": "cribl-edge",
            "image": "cribl/cribl:latest",
            "cpu": 0,
            "portMappings": [],
            "essential": true,
            "environment": [
                {
                    "name": "CRIBL_EDGE",
                    "value": "1"
                },
                {
                    "name": "CRIBL_DIST_MODE",
                    "value": "managed-edge"
                },
                {
                    "name": "CRIBL_DIST_LEADER_URL",
                    "value": "tcp://cribledge@localhost:4200"
                }
            ],
            "mountPoints": [
                {
                    "sourceVolume": "logs4edge",
                    "containerPath": "/opt/logs4edge",
                    "readOnly": true
                }
            ],
            "volumesFrom": [],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-create-group": "true",
                    "awslogs-group": "/ecs/cribl-leader-plus-edge",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "ecs"
                }
            }
        }
    ],
    "volumes": [
        {
            "name": "logs4edge
            
            ",
            "host": {}
        }
    ],
   ... truncated ...
}
```