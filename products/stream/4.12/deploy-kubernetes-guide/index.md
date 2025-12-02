# Considerations for Cribl Stream on Kubernetes



Kubernetes can be a powerful platform for deploying Cribl Stream, offering scalability and flexibility. However, there are specific use cases where Kubernetes might not be the optimal choice. This guide will explore the factors to consider when deciding whether Kubernetes is the right fit for your Cribl Stream deployment. We'll also discuss workarounds and best practices to ensure a smooth integration.

### Understanding the Challenges of Dynamic Workloads and HP Autoscaling

To maximize the benefits of Cribl Stream's integration with Kubernetes, it's essential to understand its optimal use cases and potential limitations.

#### Optimal Use Cases

Cribl Stream, when deployed on Kubernetes, excels at handling  [pull-based Sources](https://docs.cribl.io/stream/sources/#pull-sources), where data is readily available for collection. This is because Kubernetes's containerized environment and dynamic scaling capabilities align well with data Sources where collection is on demand or scheduled. 

By actively pulling data from Sources like [Collector Sources](https://docs.cribl.io/stream/sources/#collector-sources), Cribl Stream running on Kubernetes ensures timely data ingestion, and minimizes the risk of data loss. This proactive approach also enables Cribl Stream to handle a large number of Sources efficiently, making it ideal for environments with many log files or syslog servers.

#### Less-Ideal Use Cases

Cribl Stream Workers running on Kubernetes can effectively handle data streams from [push-based Sources](https://docs.cribl.io/stream/sources/#push), provided they are provisioned appropriately with fixed resources and sufficient overhead to accommodate average and peak data volumes. However, challenges can arise when dealing with senders that experience frequent, significant fluctuations in data volume, especially when using auto-scaling with scale-in policies.

Kubernetes environments, with their dynamic scaling capabilities, can be powerful tools to accommodate these fluctuations. However, the ephemeral nature of Kubernetes pods, particularly when managed by Horizontal Pod Autoscaler (HPA), can introduce complexities. HPA automatically scales the number of pods based on defined metrics, such as CPU utilization. While this is beneficial for resource optimization, it can lead to unexpected behavior in certain scenarios.

When a sudden spike in data volume occurs, HPA may scale up by creating new pods to handle the increased load. However, if the traffic spike is short-lived, these newly created pods may be terminated prematurely to conserve resources. This can disrupt ongoing data processing tasks, leading to data loss or processing delays. Additionally, the constant creation and destruction of pods can introduce latency and overhead, impacting overall system performance.

Consider the following when deploying Cribl Stream within Kubernetes environments.

### General Kubernetes Best Practices

When managing large Cribl deployments within Kubernetes, it's essential to be aware of potential limitations inherent to large Kubernetes clusters. To ensure optimal performance and stability, consider the best practices outlined in the [Kubernetes documentation](https://kubernetes.io/docs/setup/best-practices/). These guidelines address critical aspects like resource management, network configuration, and monitoring, which become increasingly important as your Kubernetes environment scales.

### Tuning Cribl for Kubernetes

Cribl Stream leverages Worker Processes to efficiently handle data ingestion and processing tasks. By configuring the maximum number of Worker Processes per pod, you can optimize resource allocation and prevent overloading.

Kubernetes containers use resource limits that map to shares of the CPU and Memory. Although a Kubernetes container has visibility of the entire host CPU, it can only use the resources allocated to the pod based on the configured limits. For details, see [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).

To ensure optimal performance and avoid overloading your Kubernetes pods, consider the following:

- **Set the Maximum Worker Processes:**  
   Use the `--set env.CRIBL_MAX_WORKERS=NUMBER_WORKER_PROCESSES` configuration setting to specify the maximum number of Worker Processes per pod. While Cribl cannot directly determine the number of CPUs within a container, this setting allows you to increase the maximum number of Worker Processes, helping to optimize resource allocation and avoid excessive load. This setting helps balance resource utilization and prevent excessive load on individual pods.  
     
- **Determine the Optimal Worker Process Count:**

   * **Self-Hosted Environments:** Calculate the available CPU cores on your host nodes and set the Worker Process count accordingly.  To avoid overloading your nodes, it's recommended to deploy a limited number of Cribl Stream pods per node. A 1:1 mapping, where each node hosts a single Cribl Stream pod, is often a good starting point. For details, see [Sizing and Scaling](https://docs.cribl.io/stream/scaling/).  
   * **Cloud Environments:** Refer to your cloud provider's instance type specifications or leverage auto-scaling mechanisms like HPA to dynamically adjust the number of pods based on demand.


### Security Context Considerations for OpenShift and Non-Root Environments {#nonroot}

To make sure your deployments run smoothly on OpenShift or non-root environments, you’ll need to configure the security context through the `values.yaml` file. The `values.yaml` file allows you to override default settings and customize your deployment without altering the core Helm chart templates.

To add or update the `values.yaml` file:  

1. Locate the [`values.yaml`](https://github.com/criblio/helm-charts/blob/master/helm-chart-sources/logstream-workergroup/values.yaml) file.
2. Modify the `securityContext` section. 
3. Customize the capabilities to match your environment's needs:

```yaml
securityContext:
  capabilities:
    add:
      - CAP_NET_BIND_SERVICE
      - CAP_SYS_PTRACE
      - CAP_DAC_READ_SEARCH    
      - CAP_NET_ADMIN
```

4. Install your updated values to Helm, using this command: `helm install -f /bar/values.yaml`.
For a description of the capabilities, see [Set Capabilities for Cribl Stream](deploy-runtime-user#cap).


### Persistent Queue Considerations for Kubernetes Deployments

Persistent queuing in Cribl Stream on Kubernetes requires careful consideration of autoscaling and storage performance.

#### Autoscaling

To ensure data durability and prevent unexpected data loss, it's crucial to carefully manage autoscaling when using persistent queues in Cribl Stream on Kubernetes.

* **Avoid Automatic Scale-In:** Disable automatic scale-in or limit it to manual intervention. This prevents unexpected pod terminations, which can lead to data loss.  
* **`StatefulSet` Deployment:** Use the `StatefulSet` deployment mode of the Worker Group chart. `StatefulSet` ensures that pods are assigned to specific volumes, maintaining data consistency.

#### Storage Performance

To ensure optimal performance and data integrity, use high-performance storage for persistent queues in Cribl Stream on Kubernetes.

**High-Performance Storage:**  Use Elastic Block Store (EBS block) storage for the `CRIBL_VOLUME_DIR`. EBS storage is resilient and has minimal risk of data loss or corruption. EBS volumes can be moved to a new Kubernetes host (in the same Availability Zone (AZ)) in the event of a host failure or termination.

**Avoid Shared Storage:** While shared storage solutions like Elastic File System (EFS) can be flexible, they may not provide the necessary performance for persistent queuing. EBS volumes, while less flexible, offer better performance and reliability.

#### Additional Considerations

* **Volume Mount Configuration:**  Ensure `CRIBL_VOLUME_DIR` is set, and set to the mount point of the PVC defined in the volumeClaimTemplates configuration. Enabling `CRIBL_VOLUME_DIR` ensures that the worker GUID is consistent on each startup.  
* **Monitoring:** Continuously monitor PQ usage, including storage utilization and performance metrics, to identify potential bottlenecks.  
* **`ExtraVolumeMounts`:** While the Kubernetes `extraVolumeMounts` option allows for persistent volume usage,  Cribl doesn't recommend it due to potential issues arising from variations in persistent storage implementations. Additionally, `extraVolumeMounts` is a requirement for non-root Kubernetes environments like Anthos and OpenShift, which can make the setup more complex.

