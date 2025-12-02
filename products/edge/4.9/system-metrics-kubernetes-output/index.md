# Kubernetes Metrics Details


This document provides an overview of the Kubernetes metrics collected by Cribl Edge's [Kubernetes Metrics](sources-kubernetes-metrics) Source. These metrics are designed to align with the schema defined by the [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics/tree/main/docs) project. 

Cribl Edge translates the raw metrics into a structured format while maintaining the same metric names, units, and dimensions as defined in the `kube-state-metrics` schema. This alignment helps to integrate with existing monitoring tools and workflows that rely on the standard.

The metrics are further organized into logical sections based on the Kubernetes resource types they represent:
- [Auth](#auth)
- [Cluster](#cluster)
- [Policy](#policy)
- [Service](#service)
- [Storage](#storage)
- [Workload](#workload)

## Auth {#auth}

Metrics related to user authentication and access control within the cluster. The metrics are further broken down into the following Metric sub-types. 

### `CertificateSigningRequest Metrics`

| Metric name | Metric type | Description |
|---|---|---|
| `kube_certificatesigningrequest_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [[`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) ](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_certificatesigningrequest_created` | Gauge | Unix creation timestamp of the CertificateSigningRequest. |
| `kube_certificatesigningrequest_condition` | Gauge | The number of each CertificateSigningRequest condition. |
| `kube_certificatesigningrequest_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_certificatesigningrequest_cert_length` | Gauge | Length of the issued certificate. | 


### `Role` Metrics


| Metric Name        | Metric Type | Description                                                                                                  |
|-------------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `kube_role_annotations` | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [[`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) (https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md).
|` kube_role_labels`    | Gauge        | Kubernetes labels converted to Prometheus labels controlled via `--metric-labels-allowlist`           |
| `kube_role_info`      | Gauge        | Information about a Kubernetes role.                                                                              |
| `kube_role_created`   | Gauge        | Unix timestamp when the role was created.                                                                           |
| `kube_role_metadata_resource_version` | Gauge | Resource version of the role object.                                                                               |

### `RoleBinding` Metrics

| Metric name                                      | Metric type | Description                                                                                                  |
|---------------------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------|
|`kube_rolebinding_annotations`                         | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)   |
| `kube_rolebinding_labels`                            | Gauge        | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)            |
| `kube_rolebinding_info `                             | Gauge        | Information about a Kubernetes rolebinding.                                                                          |
| `kube_rolebinding_created`                           | Gauge        | Unix creation timestamp of the RoleBinding.                                                                           |
| `kube_rolebinding_metadata_resource_version`         | Gauge        | Resource version of the RoleBinding object.                                                                               |

### `ServiceAccount` Metrics


| Metric name                                      | Metric type | Description                                                                                                  |
|---------------------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `kube_serviceaccount_info `                         | Gauge        | Information about a service account.                                                                                |
|` kube_serviceaccount_created `                      | Gauge        | Unix creation timestamp of the service account.                                                                     |
| `kube_serviceaccount_deleted`                       | Gauge        | Unix deletion timestamp of the service account (if applicable).                                                    |
| `kube_serviceaccount_secret `                         | Gauge        | Secret being referenced by a service account.                                                                      |
| `kube_serviceaccount_image_pull_secret`               | Gauge        | Secret being referenced by a service account for the purpose of pulling images.                                    |
| `kube_serviceaccount_annotations`                     | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_serviceaccount_labels`                          | Gauge        | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)            |


## Cluster {#cluster}

Metrics related to the overall health and status of the Kubernetes cluster itself, including nodes and namespaces.

### `ClusterRole` Metrics 

| Metric name                                      | Metric type | Description                                                                                                  |
|---------------------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `kube_clusterrole_annotations`                         | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_clusterrole_labels`                            | Gauge        | Kubernetes labels converted to Prometheus labels controlled via  [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .           |
| `kube_clusterrole_info`                              | Gauge        | Information about a Kubernetes cluster role.                                                                          |
| `kube_clusterrole_created`                           | Gauge        | Unix creation timestamp of the ClusterRole.                                                                           |
| `kube_clusterrole_metadata_resource_version`         | Gauge        | Resource version of the ClusterRole object.                                                                               |


### `ClusterRoleBinding` Metrics

| Metric name                                                 | Metric type | Description                                                                                                  |
|--------------------------------------------------------------|--------------|-----------------------------------------------------------------------------------------------------------------|
| `kube_clusterrolebinding_annotations`                         | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_clusterrolebinding_labels`                            | Gauge        | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .           |
| `kube_clusterrolebinding_info `                             | Gauge        | Information about a Kubernetes cluster role binding.                                                                |
| `kube_clusterrolebinding_created`                           | Gauge        | Unix creation timestamp of the ClusterRoleBinding.                                                                   |
| `kube_clusterrolebinding_metadata_resource_version`         | Gauge        | Resource version of the ClusterRoleBinding object.                                                                               |


### `Lease` Metrics 

| Metric name        | Metric type | Description |
|-------------------|--------------|-------------|
| `kube_lease_owner`   | Gauge        | Information about the lease owner. |
| `kube_lease_renew_time` | Gauge        | Time when the lease will be renewed. |

### `Namespace` Metrics 

| Metric name                 | Metric type | Description                                  |
|------------------------------|--------------|----------------------------------------------|
| kube_namespace_annotations   | Gauge        | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| kube_namespace_created      | Gauge        | Unix creation timestamp of the namespace.     |
| kube_namespace_labels        | Gauge        | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .          |
| kube_namespace_status_phase  | Gauge        | The phase of the namespace (Active or Terminating). |

### `Node` Metrics 
| Metric name                 | Metric type | Description                                  | 
|------------------------------|--------------|----------------------------------------------|
| `kube_node_created`             | Gauge        | Unix creation timestamp of the node.         | 
| `kube_node_status_allocatable`  | Gauge        | The amount of resources allocatable for pods (after reserving some for system daemons). | 
| `kube_node_status_capacity`     | Gauge        | The total amount of resources available for a node. | 
| `kube_node_status_condition `  | Gauge        | The condition of a cluster node.             | 
| `kube_node_spec_schedulable`    | Gauge        | Whether a node can schedule new pods.        | 
| `kube_node_spec_taint`          | Gauge        | The taint of a cluster node.                  | 
| `kube_node_status_addresses `  | Gauge        | The addresses of a node.                      | 
| `kube_node_labels`             | Gauge        | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . | 
| `kube_node_status_phase`        | Gauge        | The phase of the node (Unknown, Running, Terminating). | 
| `kube_node_info`               | Gauge        | Information about a cluster node.             |  (but see kube_node_*. metrics for details)


## Policy {#policy}

Metrics related to resource quotas, limits, and network policies that govern resource usage within the cluster.

### `LimitRange` Metrics

| Metric name        | Metric type | Description |
|-------------------|--------------|-------------|
| `kube_limitrange`     | Gauge        | Information about resource limits and requests. | 
| `kube_limitrange_created` | Gauge        | Unix creation timestamp of the LimitRange. |

### `Network Policy` Metrics 

| Metric name                           | Metric type | Description                                                                                                               |
|------------------------------------|--------------|------------------------------------------------------------------------------------------------------------------------|
| `kube_networkpolicy_annotations`        | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via `metric-annotations-allowlist` |
| `kube_networkpolicy_created`            | Gauge       | Unix creation timestamp of the NetworkPolicy.                                                                   |
| `kube_networkpolicy_labels`             | Gauge       | Kubernetes labels converted to Prometheus labels controlled via `metric-labels-allowlist`)           |
| `kube_networkpolicy_spec_egress_rules`  | Gauge       | The number of egress rules defined in the NetworkPolicy.                                                             |
| `kube_networkpolicy_spec_ingress_rules`| Gauge       | The number of ingress rules defined in the NetworkPolicy.                                                              |

### `PodDisruptionBudget` Metrics

| Metric name                                             | Metric type | Description                                                                                                               |
|------------------------------------------------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `kube_poddisruptionbudget_annotations`                    | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_poddisruptionbudget_labels`                         | Gauge       | Kubernetes labels converted to Prometheus labels controlled via `--metric-labels-allowlist]`         |
| `kube_poddisruptionbudget_created`                        | Gauge       | Unix creation timestamp of the PodDisruptionBudget.                                                                   |
| `kube_poddisruptionbudget_status_current_healthy `        | Gauge       | The number of currently healthy pods relative to the PodDisruptionBudget's desired healthy pods.                             |
| `kube_poddisruptionbudget_status_desired_healthy`         | Gauge       | The minimum number of healthy pods that the PodDisruptionBudget requires.                                                       |
| `kube_poddisruptionbudget_status_pod_disruptions_allowed` | Gauge       | The number of pod disruptions that are currently allowed for the PodDisruptionBudget.                                           |
| `kube_poddisruptionbudget_status_expected_pods`           | Gauge       | The total number of pods that the PodDisruptionBudget expects to be running at any given time.                               |
| `kube_poddisruptionbudget_status_observed_generation`     | Gauge       | The generation observed by the PodDisruptionBudget controller.                                                                  |


### `ResourceQuota` Metrics

| Metric name                                             | Metric type | Description                                                                                                               |
|------------------------------------------------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `kube_resourcequota`                                       | Gauge       | Provides information about the resource quotas defined for namespaces.                                                                                                                          |
|` kube_resourcequota_created`                              | Gauge       | Unix creation timestamp of the ResourceQuota.                                                                           |
| `kube_resourcequota_annotations `                         | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_resourcequota_labels`                               | Gauge       | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`]((https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) )         |

## Service {#service}

These metrics provide information about the health and connectivity of services within the cluster.

### `Endpoint `Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_endpoint_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) | 
| `kube_endpoint_info `| Gauge | Information about a Kubernetes endpoint. | 
| `kube_endpoint_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`]((https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) | 
| `kube_endpoint_created` | Gauge | Time a Kubernetes endpoint was created. | 
| `kube_endpoint_ports` | Gauge | Information about ports of a Kubernetes endpoint. | 
| `kube_endpoint_address` | Gauge | IP addresses and ports of a Kubernetes endpoint. | 

### `Endpointslice` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| kube_endpointslice_annotations | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_endpointslice_info` | Gauge | Information about a Kubernetes endpointslice. |
| `kube_endpointslice_ports` | Gauge | Information about ports of a Kubernetes endpointslice. |
| `kube_endpointslice_endpoints` | Gauge | Information about endpoints in a Kubernetes endpointslice. |
| `kube_endpointslice_endpoints_hints` | Gauge | Hints applied to an endpointslice. |
| `kube_endpointslice_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_endpointslice_created `| Gauge | Time a Kubernetes endpointslice was created. | 


### `Ingress` Metrics 

| Metric name | Metric type | Description |
|---|---|---|
| `kube_ingress_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_ingress_info` | Gauge | Information about a Kubernetes ingress. |
| `kube_ingress_labels`| Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
|`kube_ingress_created` | Gauge | Time a Kubernetes ingress was created. |
| `kube_ingress_metadata_resource_version` | Gauge | Resource version of the ingress object. |
| `kube_ingress_path` | Gauge | Information about a path served by an ingress. |
| `kube_ingress_tls` | Gauge | Information about TLS configuration of an ingress. |

### `IngressClass` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_ingressclass_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)   |
| `kube_ingressclass_info` | Gauge | Information about a Kubernetes ingressclass. |
| `kube_ingressclass_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)   |
| `kube_ingressclass_created` | Gauge | Time a Kubernetes ingressclass was created. |

### `Service` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_service_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  .|
| `kube_service_info` | Gauge | Information about a service. |
| `kube_service_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_service_created` | Gauge | Unix creation timestamp of the service. |
| `kube_service_spec_type` | Gauge | Type of the service. |
| `kube_service_spec_external_ip` | Gauge | Service external IPs. One series for each IP. |
| `kube_service_status_load_balancer_ingress` | Gauge | Service load balancer ingress status. |

## Storage {#storage}

Metrics related to the usage and performance of storage resources within your cluster.

### `ConfigMap` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_configmap_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_configmap_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-labels-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_configmap_info` | Gauge | Information about a ConfigMap. |
| `kube_configmap_created` | Gauge | Time a ConfigMap was created. |
| `kube_configmap_metadata_resource_version` | Gauge | Resource version of the ConfigMap object. |


### `PersistentVolume` Metrics

| Metric Name | Metric Type | Description |
|---|---|---|
| `kube_persistentvolume_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [--metric-annotations-allowlist](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_persistentvolume_capacity_bytes` | Gauge |  |
|`kube_persistentvolume_status_phase` | Gauge |  |
| `kube_persistentvolume_claim_ref` | Gauge |  |
| `kube_persistentvolume_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [--metric-labels-allowlist](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_persistentvolume_info` | Gauge | Information about Persistent Volumes |
| `kube_persistentvolume_created` | Gauge | Unix creation timestamp |
| `kube_persistentvolume_deletion_timestamp` | Gauge | Unix deletion timestamp |
| `kube_persistentvolume_csi_attributes` | Gauge | CSI attributes of the Persistent Volume, disabled by default, manage with [--metric-opt-in-list](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) |
| `kube_persistentvolume_volume_mode` | Gauge | Volume Mode information for the PersistentVolume. |


### `PersistentVolumeClaim` Metrics

| Metric Name | Metric Type | Description |
|---|---|---|
| `kube_persistentvolumeclaim_annotations `| Gauge | Kubernetes annotations converted to Prometheus labels controlled via [--metric-annotations-allowlist](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_persistentvolumeclaim_access_mode` | Gauge | Access modes allowed for this claim. |
| `kube_persistentvolumeclaim_info` | Gauge | Information about PersistentVolumeClaims | 
| `kube_persistentvolumeclaim_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [--metric-labels-allowlist](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) |
| `kube_persistentvolumeclaim_resource_requests_storage_bytes` | Gauge | Requested storage size in bytes |
| `kube_persistentvolumeclaim_status_condition` | Gauge | Condition of the claim |
| `kube_persistentvolumeclaim_status_phase` | Gauge | Phase of the claim |
| `kube_persistentvolumeclaim_created` | Gauge | Unix creation timestamp |
| `kube_persistentvolumeclaim_deletion_timestamp` | Gauge | Unix deletion timestamp |


### `Secret` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| k`ube_secret_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md)  |
| `kube_secret_info` | Gauge | Information about Secrets (such as, type). |
| `kube_secret_type` | Gauge | Type of the Secret. |
| `kube_secret_created` | Gauge | Unix creation timestamp of the Secret. |
| `kube_secret_metadata_resource_version` | Gauge | Resource version of the Secret object. |
| `kube_secret_owner`| Gauge | Owner information of the Secret. |

 
### `StorageClass` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_storageclass_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_storageclass_info` | Gauge | Information about StorageClasses (such as, provisioner, reclaim policy, volume binding mode). |
| `kube_storageclass_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) |
| `kube_storageclass_created` | Gauge | Unix creation timestamp of the StorageClass. |

### `VolumeAttachment` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_volumeattachment_info` | Gauge | Information about VolumeAttachments (such as, attacher, node). |
| `kube_volumeattachment_created` | Gauge | Unix creation timestamp of the VolumeAttachment. |
| `kube_volumeattachment_spec_source_persistentvolume` | Gauge | PersistentVolume name used in the VolumeAttachment spec source. |
| `kube_volumeattachment_status_attached` | Gauge | Whether the VolumeAttachment is attached. |
| `kube_volumeattachment_status_attachment_metadata` | Gauge | Metadata of the attached volume. |

## Workload {#workload}

Metrics related to overall resource consumption and performance of your applications. They track key aspects like CPU usage, memory utilization, and network traffic of your deployments, pods, and other workloads.

### `CronJob` Metrics

| Metric name                                    | Metric type | Description                                                                                                                                                      |
| ---------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `kube_cronjob_annotations`                       | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_cronjob_info`                              | Gauge       | Information about CronJobs (such as, schedule, concurrency policy, timezone). |
| `kube_cronjob_labels `                           | Gauge       | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_cronjob_created`                           | Gauge       | Unix creation timestamp of the CronJob. |
| `kube_cronjob_next_schedule_time`                | Gauge       | Time of the next scheduled execution of the CronJob. |
| `kube_cronjob_status_active`                     | Gauge       | Whether the CronJob is actively running Jobs. |
| `kube_cronjob_status_last_schedule_time`         | Gauge       | Time of the last attempted execution of the CronJob. |
| `kube_cronjob_status_last_successful_time`       | Gauge       | Time of the last successful execution of the CronJob. |
| `kube_cronjob_spec_suspend `                     | Gauge       | Whether the CronJob is suspended. |
| `kube_cronjob_spec_starting_deadline_seconds`    | Gauge       | Deadline in seconds for the CronJob to complete the execution of its Jobs. |
| `kube_cronjob_metadata_resource_version`         | Gauge       | Resource version of the CronJob object. |
| `kube_cronjob_spec_successful_job_history_limit` | Gauge       | Maximum number of successful Jobs to retain. |
| `kube_cronjob_spec_failed_job_history_limit`     | Gauge       | Maximum number of failed Jobs to retain. |

### `DaemonSet` Metrics

| Metric name                                    | Metric type | Description                                                                                                                                                      |
| ---------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `kube_daemonset_annotations`                     | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_daemonset_created`                         | Gauge       | Unix creation timestamp of the DaemonSet. |
| `kube_daemonset_status_current_number_scheduled` | Gauge       | Current number of scheduled pods by the DaemonSet. |
| `kube_daemonset_status_desired_number_scheduled` | Gauge       | Desired number of scheduled pods by the DaemonSet. |
| `kube_daemonset_status_number_available`         | Gauge       | Number of available pods (ready and running) managed by the DaemonSet. |
| `kube_daemonset_status_number_misscheduled`      | Gauge       | Number of pods that are running but are not scheduled to be running on the current node. |
| `kube_daemonset_status_number_ready`             | Gauge       | Number of pods that are ready and running. |
| `kube_daemonset_status_number_unavailable`       | Gauge       | Number of unavailable pods managed by the DaemonSet. |
| `kube_daemonset_status_observed_generation`      | Gauge       | Generation of the DaemonSet that was last observed by the controller. |
| `kube_daemonset_status_updated_number_scheduled` | Gauge       | The number of pods that have been scheduled by the DaemonSet controller. |
| `kube_daemonset_metadata_generation`           | Gauge       | Generation of the resource as seen by the server. |
| `kube_daemonset_labels`                          | Gauge       | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |

### `Deployment` Metrics

| Metric name                                                | Metric type | Description                                                                                                                                                                 |
| ----------------------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `kube_deployment_annotations`                               | Gauge       | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_deployment_status_replicas`                             | Gauge       | Current number of replicas of the deployment. |
| `kube_deployment_status_replicas_available`                 | Gauge       | Number of available pods (ready and running) managed by the deployment. |
| `kube_deployment_status_replicas_unavailable`               | Gauge       | Number of unavailable pods managed by the deployment. |
| `kube_deployment_status_replicas_updated`                     | Gauge       | The number of pods that have been updated by the deployment controller. |
| `kube_deployment_status_observed_generation`                | Gauge       | Generation of the deployment that was last observed by the controller. |
|`kube_deployment_spec_replicas`                              | Gauge       | Desired number of replicas for the deployment. |
| `kube_deployment_spec_paused`                               | Gauge       | Whether the deployment is paused. |
| `kube_deployment_spec_strategy_rollingupdate_max_unavailable` | Gauge       | Maximum unavailable replicas during a rolling update. |
| `kube_deployment_spec_strategy_rollingupdate_max_surge`     | Gauge       | Maximum surge replicas during a rolling update. |
| `kube_deployment_metadata_generation`                     | Gauge       | Generation of the resource as seen by the server. |
| `kube_deployment_created`                                   | Gauge       | Unix creation timestamp of the deployment. |

### `Horizontal Pod Autoscaler` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_horizontalpodautoscaler_info` | Gauge | Information about the Horizontal Pod Autoscaler (HPA). |
| `kube_horizontalpodautoscaler_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_horizontalpodautoscaler_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_horizontalpodautoscaler_metadata_generation` | Gauge | Generation of the HPA resource as seen by the server. |
| `kube_horizontalpodautoscaler_spec_max_replicas` | Gauge | Maximum number of replicas allowed for the target resource. |
| `kube_horizontalpodautoscaler_spec_min_replicas` | Gauge | Minimum number of replicas allowed for the target resource. |
| `kube_horizontalpodautoscaler_spec_target_metric` | Gauge | Target metric for the HPA. |
| `kube_horizontalpodautoscaler_status_target_metric` | Gauge | Current value of the target metric. |
| `kube_horizontalpodautoscaler_status_condition` | Gauge | Condition of the HPA (such as, True, False, Unknown). |
| `kube_horizontalpodautoscaler_status_current_replicas` | Gauge | Current number of replicas of the target resource. |
| `kube_horizontalpodautoscaler_status_desired_replicas` | Gauge | Desired number of replicas for the target resource as determined by the HPA. |

### `Job` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_job_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_job_info` | Gauge | Information about the Job. |
| `kube_job_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via `--metric-annotations-allowlist`. |
| `kube_job_owner` | Gauge | Owner of the Job. |
| `kube_job_spec_parallelism` | Gauge | Parallelism of the Job. |
| `kube_job_spec_completions` | Gauge | Completions of the Job. |
| `kube_job_spec_active_deadline_seconds` | Gauge | Active deadline seconds of the Job. |
| `kube_job_status_active` | Gauge | Whether the Job is active. |
| `kube_job_status_succeeded` | Gauge | Whether the Job succeeded. |
| `kube_job_status_failed` | Gauge | Whether the Job failed. |
| `kube_job_status_start_time` | Gauge | Start time of the Job. |
| `kube_job_status_completion_time` | Gauge | Completion time of the Job. |
| `kube_job_complete` | Gauge | Job completion condition. |
| `kube_job_failed` | Gauge | Job failed condition. |
| `kube_job_created` | Gauge | Job creation time. |
| `kube_job_status_suspended`| Gauge | Whether the Job is suspended. |

### `Pod` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_pod_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via  [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_pod_info` | Gauge | Information about pod |
| `kube_pod_ips` | Gauge | Pod IP addresses |
| `kube_pod_start_time` | Gauge | Start time in unix timestamp for a pod |
| `kube_pod_completion_time` | Gauge | Completion time in unix timestamp for a pod |
| `kube_pod_owner` | Gauge | Information about the Pod's owner |
| `kube_pod_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via  [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_pod_nodeselectors` | Gauge | Describes the Pod nodeSelectors |
| `kube_pod_status_phase` | Gauge | The pods current phase |
| `kube_pod_status_qos_class` | Gauge | The pods current qosClass |
| `kube_pod_status_ready` | Gauge | Describes whether the pod is ready to serve requests |
| `kube_pod_status_scheduled` | Gauge | Describes the status of the scheduling process for the pod |
| `kube_pod_container_info` | Gauge | Information about a container in a pod |
| `kube_pod_container_status_waiting` | Gauge | Describes whether the container is currently in waiting state |
| `kube_pod_container_status_waiting_reason` | Gauge | Describes the reason the container is currently in waiting state |
| `kube_pod_container_status_running` | Gauge | Describes whether the container is currently in running state |
| `kube_pod_container_state_started` | Gauge | Start time in unix timestamp for a pod container |
| `kube_pod_container_status_terminated` | Gauge | Describes whether the container is currently in terminated state |
| `kube_pod_container_status_terminated_reason` | Gauge | Describes the reason the container is currently in terminated state |
| `kube_pod_container_status_last_terminated_reason` | Gauge | Describes the last reason the container was in terminated state |
| `kube_pod_container_status_last_terminated_exitcode` | Gauge | Describes the exit code for the last container in terminated state. |
| `kube_pod_container_status_last_terminated_timestamp `| Gauge | Last terminated time for a pod container in unix timestamp. |
| `kube_pod_container_status_ready` | Gauge | Describes whether the containers readiness check succeeded |
| `kube_pod_status_initialized_time` | Gauge | Time when the pod is initialized. |
| `kube_pod_status_ready_time` | Gauge | Time when pod passed readiness probes. |
| `kube_pod_status_container_ready_time` | Gauge | Time when the container of the pod entered Ready state. |
| `kube_pod_container_status_restarts_total` | Counter | The number of container restarts per container |
| `kube_pod_container_resource_requests` | Gauge | The number of requested request resource by a container. It is recommended to use the `kube_pod_resource_requests` metric exposed by kube-scheduler instead, as it is more precise. |
| `kube_pod_container_resource_limits` | Gauge | The number of requested limit resource by a container. It is recommended to use the `kube_pod_resource_limits` metric exposed by kube-scheduler instead, as it is more precise. |
| `kube_pod_overhead_cpu_cores` | Gauge | The pod overhead in regards to cpu cores associated with running a pod |
| `kube_pod_overhead_memory_bytes` | Gauge | The pod overhead in regards to memory associated with running a pod |
| `kube_pod_runtimeclass_name_info` | Gauge | The runtimeclass associated with the pod |
| `kube_pod_created` | Gauge | Unix creation timestamp |
| `kube_pod_deletion_timestamp` | Gauge | Unix deletion timestamp |
| `kube_pod_restart_policy` | Gauge | Describes the restart policy in use by this pod |
| `kube_pod_init_container_info` | Gauge | Information about an init container in a pod |
| `kube_pod_init_container_status_waiting` | Gauge | Describes whether the init container is currently in waiting state |
| `kube_pod_init_container_status_waiting_reason` | Gauge | Describes the reason the init container is currently in waiting state |
| `kube_pod_init_container_status_running` | Gauge | Describes whether the init container is currently in running state |
| `kube_pod_init_container_status_terminated` | Gauge | Describes whether the init container is currently in terminated state |
| `kube_pod_init_container_status_terminated_reason` | Gauge | Describes the reason the init container is currently in terminated state |
| `kube_pod_init_container_status_last_terminated_reason` | Gauge | Describes the last reason the init container was in terminated state |
| `kube_pod_init_container_status_ready` | Gauge | Describes whether the init containers readiness check succeeded |
| `kube_pod_init_container_status_restarts_total` | Counter | The number of restarts for the init container |
| `kube_pod_init_container_resource_limits` | Gauge | The number of CPU cores requested limit by an init container |
| `kube_pod_init_container_resource_requests` | Gauge | The number of CPU cores requested by an init container |
| `kube_pod_spec_volumes_persistentvolumeclaims_info` | Gauge | Information about persistentvolumeclaim volumes in a pod |
| `kube_pod_spec_volumes_persistentvolumeclaims_readonly` | Gauge | Describes whether a persistentvolumeclaim is mounted read only |
| `kube_pod_status_reason` | Gauge | The pod status reasons |
| `kube_pod_status_scheduled_time` | Gauge | Unix timestamp when pod moved into scheduled status |
| `kube_pod_status_unschedulable`| Gauge | Describes the unschedulable status for the pod |
| `kube_pod_tolerations` | Gauge | Information about the pod tolerations |
| `kube_pod_service_account`| Gauge | The service account for a pod |
| `kube_pod_scheduler` | Gauge | The scheduler for a pod |

### `ReplicaSet` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_replicaset_annotations` | Gauge | Kubernetes annotations converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) .|
| `kube_replicaset_status_replicas` | Gauge | Number of replicas in the current ReplicaSet. |
| `kube_replicaset_status_fully_labeled_replicas` | Gauge | Number of fully labeled replicas in the current ReplicaSet. |
| `kube_replicaset_status_ready_replicas` | Gauge | Number of ready replicas in the current ReplicaSet. |
| `kube_replicaset_status_observed_generation` | Gauge | Observed generation of the ReplicaSet. |
| `kube_replicaset_spec_replicas` | Gauge | Replicas is the number of desired replicas. |
| `kube_replicaset_metadata_generation` | Gauge | Generation of the ReplicaSet as seen by the server. |
| `kube_replicaset_labels` | Gauge | Kubernetes labels converted to Prometheus labels controlled via [`--metric-annotations-allowlist`](https://github.com/kubernetes/kube-state-metrics/blob/main/docs/developer/cli-arguments.md) . |
| `kube_replicaset_created` | Gauge | Unix creation timestamp. |
| `kube_replicaset_owner` | Gauge | Owner of the ReplicaSet. |

### `ReplicationController` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_replicationcontroller_status_replicas` | Gauge | Number of replicas in the current ReplicationController. |
|`kube_replicationcontroller_status_fully_labeled_replicas` | Gauge | Number of fully labeled replicas in the current ReplicationController. |
| `kube_replicationcontroller_status_ready_replicas` | Gauge | Number of ready replicas in the current ReplicationController. |
| `kube_replicationcontroller_status_available_replicas` | Gauge | Number of available replicas in the current ReplicationController. |
| `kube_replicationcontroller_status_observed_generation` | Gauge | Observed generation of the ReplicationController. |
| `kube_replicationcontroller_spec_replicas` | Gauge | Replicas is the number of desired replicas. |
| `kube_replicationcontroller_metadata_generation` | Gauge | Generation of the ReplicationController as seen by the server. |
| `kube_replicationcontroller_created` | Gauge | Unix creation timestamp. |
| `kube_replicationcontroller_owner` | Gauge | Owner of the ReplicationController. |

### `Stateful Set` Metrics

| Metric name | Metric type | Description |
|---|---|---|
| `kube_statefulset_status_replicas` | Gauge | Number of replicas in the current StatefulSet. |
| `kube_statefulset_status_replicas_current` | Gauge | Current number of replicas in the StatefulSet. |
| `kube_statefulset_status_replicas_ready` | Gauge | Number of ready replicas in the StatefulSet. |
| `kube_statefulset_status_replicas_available` | Gauge | Number of available replicas in the StatefulSet. |
| `kube_statefulset_status_replicas_updated` | Gauge | Number of updated replicas in the StatefulSet. |
| `kube_statefulset_status_observed_generation` | Gauge | Observed generation of the StatefulSet. |
| `kube_statefulset_replicas` | Gauge | Desired number of replicas in the StatefulSet. |
| `kube_statefulset_ordinals_start` | Gauge | Ordinals starting point for StatefulSet. |
| `kube_statefulset_metadata_generation` | Gauge | Generation of the StatefulSet as seen by the server. |
| `kube_statefulset_persistentvolumeclaim_retention_policy` | Gauge | StatefulSet PVC retention policy. |
| `kube_statefulset_created` | Gauge | Unix creation timestamp of the StatefulSet. |
| `kube_statefulset_status_current_revision` | Gauge | Current revision of the StatefulSet. |
| `kube_statefulset_status_update_revision` | Gauge | Update revision of the StatefulSet. |