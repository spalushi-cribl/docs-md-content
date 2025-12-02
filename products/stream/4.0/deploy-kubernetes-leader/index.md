# Kubernetes/Helm Deployment


Boot fully provisioned Leader and Worker Nodes via Helm

---

Cribl's leader and workergroup [Helm charts](https://github.com/criblio/helm-charts) provide a fast way to deploy a distributed Cribl Stream environment to a Kubernetes cluster.

For up-to-date details on [Prerequisites](https://github.com/criblio/helm-charts#prerequisites), [Deploying](https://github.com/criblio/helm-charts#deploying), [Upgrading](https://github.com/criblio/helm-charts#upgrading), and running distributed on a [Free License](https://github.com/criblio/helm-charts#running-distributed-on-a-free-license), see the Cribl Helm Charts repo's [Readme](https://github.com/criblio/helm-charts#readme). 

## K8s Leader Deployment

The rest of this page outlines how to deploy a Cribl Stream Leader Node (or [single instance](deploy-single-instance)) to AWS via Kubernetes, using a Cribl-provided [Leader Helm chart](https://github.com/criblio/helm-charts/tree/master/helm-chart-sources/logstream-leader).

> This chart is a work in progress, provided as-is. Cribl expects to further develop and refine it.
> 
> Cribl recommends deploying the Leader on stable, highly available infrastructure, because of its role in coordinating all Worker instances.
>
{.box .info}

## Deprecation Notice

This chart replaces the `logstream‑master` chart, which was deprecated as of v.2.9.9. See [Migration](#migration) below for instructions on migrating to this new chart to access features newly provided in this and future versions.

## New Capabilities

- Supports Cribl Stream v.4.0.4 (default version).


## Deployment

As built, Cribl's chart will deploy a Cribl Stream Leader server for Cribl Stream, consisting of a deployment, two services, and a number of persistent volumes.

![Deployment schematic](k8s-logstream-leader.8e3b98c4e0.png)

Note that this chart creates two load-balanced services:

- The main one (named after the Helm release), which is intended as the primary service interface for users.

- The "internal" one (named `<helm-release>‑internal`), which is intended for the worker-group-to-leader communication.



> By default, this chart installs only a Cribl Stream Leader Node. To also deploy Cribl Stream Worker Groups/Fleets via Helm, you can use the [Set Up Worker Groups/M​appings](#groups) override described below.
> 
> You can also use Cribl's separate [logstream-workergroup](https://github.com/criblio/helm-charts/tree/master/helm-chart-sources/logstream-workergroup) chart. For details, see  [Kubernetes Deployment: Worker Group/Fleet](deploy-kubernetes) in this documentation.
>
{.box .success}

## AWS and Kubernetes Prerequisites

This section covers both general and specific prerequisites, with a bias toward the [EKS](https://aws.amazon.com/eks/)‑oriented approach that Cribl uses for its own deployments.

#### Set Up AWS CLI

Install the AWS CLI, version 2, following [AWS' instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html). 

Next, create or modify your `~/.aws/config` file to include (at least) a `[profile]` section with the following SSO (single-sign-on) details:

``` {title="&#126;/.aws/config"}
[profile <your-profile-name>]
sso_start_url = https://<your-domain>/start#/
sso_region = <your-AWS-SSO-region>
sso_account_id = <your-AWS-SSO-account-ID>
sso_role_name = <your-AWS-role-name>
region = <your-AWS-deployment-region>
```

#### Set Up kubectl

You will, of course, need `kubectl` set up on your local machine or VM. Follow [Kubernetes' installation instructions](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

#### Add a Cluster to Your kubeconfig File

You must modify your `~/.kube/config` file to instruct kubectl what cluster (context) to work with.

1. Run a command of this form:
  `aws --profile <profile‑name> eks update-kubeconfig --name <cluster‑name>` <br/>
  This should return a response like this:
   `Added new context arn:aws:eks:us-west-2:424242424242:cluster/<cluster‑name> to /Users/<username>/.kube/config`

1. In the resulting `~/.kube/config` file's `args` section, as the new first child, insert the profile argument that you provided to the aws command. For example:

``` {title="&#126;/.kube/config"}
args:
- --profile=<profile‑name>
- --region
[...]
```

3. Also change the `command: aws` pair to include the full path to the `aws` executable. 
  This is usually in `/usr/local/bin`, in which case you'd insert: `command: /usr/local/bin/aws`.

This section of `~/.kube/config` should now look something like this:

``` {title="&#126;/.kube/config"}
      args:
      - --profile=<profile‑name>
      - --region
      - us-west-2
      - eks
      - get-token
      - --cluster-name
      - lab
      command: /usr/local/bin/aws
      env:
      - name: AWS_PROFILE
        value: <profile-name>
```

With these AWS and Kubernetes prerequisites completed, you're now set up to run `kubectl` commands against your cluster, as long as you have an active aws SSO login session.

Next, do the Helm setup.

## Install Helm and Cribl Repo

1. You'll need Helm (preferably v.3.x) installed. Follow the instructions [here](https://helm.sh/docs/intro/install/).

1. Add Cribl's repo to Helm, using this command:
  `helm repo add cribl https://criblio.github.io/helm-charts/`

## Persistent Storage

The chart requires persistent storage. It will use your default StorageClass, or (if you prefer) you can override `config.scName` with the name of a specific StorageClass to use. 

Cribl has tested this chart primarily using AWS EBS storage, via the CSI EBS driver. The volumes are created as `ReadWriteOnce` claims. For details about storage classes, see Kubernetes' [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/) documentation.

## AWS-Specific Notes

If you're running on EKS, Cribl highly recommends that you use Availability Zone–specific node groups. For details, see eksctl.io's [Autoscaling](https://eksctl.io/usage/autoscaling/) documentation. 

> Do not allow a single node group to spans AZs. This can lead to trouble in mounting volumes, because EBS volumes are AZ-specific.
>
{.box .warning}

See other [EKS-Specific Issues](https://github.com/criblio/helm-charts/blob/master/common_docs/EKS_SPECIFICS.md#known-eks-problems) on our GitHub repo.

## Configure the Chart's Values

You'll want to override some of the chart's default values. The easiest way is to copy this chart's default `values.yaml` file from our repo. save it locally, modify it, and install it in Helm:

1. Copy the **raw** contents of:
  https://github.com/criblio/helm-charts/blob/master/helm-chart-sources/logstream-leader/values.yaml

1. Save this as a local file, e.g.: ` /bar/values.yaml`

1. Modify values as necessary (see [Values to Override](https://github.com/criblio/helm-charts/tree/master/helm-chart-sources/logstream-leader#values-to-override) in the Helm Charts docs).

1. Install your updated values to Helm, using this command:
  `helm install -f /bar/values.yaml`


#### Match Versions

Cribl recommends that you use the same Cribl Stream version on Worker Nodes/Edge Nodes versus the Leader Node. So if, for any reason, you're not yet upgrading your Workers to the version in the Leader's default `values.yaml` > `criblImage.tag`, be sure to override that `criblImage.tag` value to match the version you're running on all Workers.

#### EKS-Specific Values

If you're deploying to EKS, many annotations are available for the load balancer. Set these as values for the `service.annotations` key. Internally, we typically use the annotations for logging to S3, like this:

``` {title="values.yaml [excerpt]"}
    service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "true"
    service.beta.kubernetes.io/aws-load-balancer-access-log-emit-interval: "5"
    service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-name: "<bucket name>"
    service.beta.kubernetes.io/aws-load-balancer-access-log-s3-bucket-prefix: "ELB"
```

For an exhaustive list of annotations you can use with AWS's Elastic Load Balancers, see the [Kubernetes Service](https://kubernetes.io/docs/concepts/services-networking/service/) documentation.


## Basic Chart Installation {#install}

With the above prerequisites and configuration completed, you're ready to install our chart to deploy a Cribl Stream Leader Node. Here are some example commands:

- To  install the chart with the release name `logstream-leader`: <br/>
  `helm install logstream-leader cribl/logstream-leader`

- To install the chart using the storage class `ebs-sc`: <br/>
  `helm install logstream-leader cribl/logstream-leader --set config.scName='ebs-sc'`

####  Post-Install/Post-Upgrade  {#post}

Cribl Stream will not automatically deploy changes to the Worker Nodes/Edge Nodes. You'll need to [commit and deploy changes](deploy-distributed#deploy) to all of your Worker Groups/Fleets. 

## Change the Configuration {#change-config}

If you don't override its default values, this Helm chart effectively creates a [single‑instance deployment](deploy-single-instance) of Cribl Stream, using the standard container image. You can later configure [distributed mode](deploy-distributed), licensing, user passwords, etc., all from the Cribl Stream UI. However, you also have the option to change these configuration details upfront, by installing with value overrides. Here are some common examples.

#### Apply a License 

If you have a Standard or Enterprise [license](licensing), you can use the `config.license` parameter to add it as an override to your install:

  `helm install logstream-leader cribl/logstream-leader --set config.license="<long encoded license string redacted>"`

#### Run Distributed on a Free License

If you do not specify a license with `config.license`, and you want to run [distributed](deploy-distributed), you'll need to go to Cribl Stream's **Settings** > **Global Settings** > **Licensing** UI page and accept the Free license. (The Free license allows one Worker Group/Fleet.) 

If your Helm configuration includes the `config.groups` option, the Cribl Stream Leader Node will be configured as a distributed Leader. If you omit that option, it will be configured as a single instance. (You can later use Cribl Stream's **Settings** > **Global Settings** > **Distributed** page to select **Mode:** `Leader`.)

#### Set the Admin Password

Normally, when you first install Cribl Stream and log into the UI, it prompts you to change the default admin password. You can skip the password-change challenge by setting your admin password via the `config.adminPassword` parameter:

  `helm install logstream-leader cribl/logstream-leader --set config.adminPassword="<new password>"`

#### Set Up Worker Groups/Mappings {#groups}

As mentioned above, the chart's default is to install a vanilla deployment of Cribl Stream. If you are deploying as a Leader, you can use the `config.groups` parameter to define the Worker Groups/Fleets you want created and mapped. Each group in the list you provide will be created as a Worker Group/Fleet, with a Mapping Rule to seek a tag with that Worker Group's name in it:

  `helm install logstream-leader cribl/logstream-leader --set config.groups={group1,group2,group3}`

The example above will create three Worker Groups/Fleets/Fleets – `group1`, `group2`, and `group3` – and a Mapping Rule for each.

## Migrating from the logstream-master Chart  {#migration}

Here is how to migrate from the deprecated `logstream‑master` chart to `logstream‑leader`.

#### Exporting your Configuration

You'll need to "export" your data from the existing `logstream-master` pod. And first, you'll need to get the current pod's name, as well as its namespace. The easiest way to do this is to run `kubectl get pods -A` and then look pods that start with the release name you used when you ran `helm install`. For example, if you installed with the following command:

`helm install ls-master cribl/logstream-master`

...you'd look for a pod name that started with `ls-master`.

Once you've identified your pod and namespace, you can export your configuration using a combination of `kubectl` and `tar`:

```shell
kubectl exec <pod name> -n <namespace> -- bash -c "cd /opt/cribl/config-volume; tar cf - ." > cribl_backup.tar
```

This command executes the tar based back up of the config-volume, and outputs it to a local tar file (`cribl_backup.tar`).

#### "Re-Hydrating" the Backup on the logstream-leader Chart

Exploding the tarball onto the new persistent volume is a one-time event. Once the config volume is restored, you'll make changes to the config via the Cribl Stream UI or API. Either approach will change the config on disk, which you wouldn't want to overwrite the next time the pod restarts. You can manually re-hydrate the backup by installing the logstream‑leader chart, and then running the following command:

```shell
cat cribl_backup.tar| kubectl -n <namespace> exec --stdin <pod name> -- bash -c "cd /opt/cribl/config‑volume/; tar xf -"
```

This will restore the data into the config volume (which is mounted as `/opt/cribl/config-volume`). If you want to double-check that, run:

```shell
kubectl -n <namespace> exec <pod name> -- bash -c "ls -alR /opt/cribl/config-volume"
```

After this, you want to **delete** the active pod, allowing the new one to come up with the restored configuration. To do this, you'd run the following `kubectl` command:

```shell
kubectl -n <namespace> delete <pod name>
```

This will cause the pod to exit, but the deployment will replace it with a new pod which will use the same config persistent volume.

#### Reconfiguring the Worker Groups/Fleets

Now that you've got a new working leader chart, you need to tell the workers to connect to the new leader instead of to the old `logstream-master` instance. This is a simple `helm upgrade` operation. You'll need to use the same command string that you used to install, changing the word `install` to `upgrade`. But change the value of `config.host` to the new service that was created for the logstream‑leader install.  (You can change `config.host` either via the `--set` option or in the `values.yml` file.) For example, if you ran the `logstream-leader` install with the release name `ls-lead`, like this:

`helm install ls-lead -f <values file> cribl/logstream-leader`

...you'd run `kubectl get service -n <namespace> | grep ls-lead` to get the two services that it created, and you'll want the name of the one that ends in `-internal`. In this case, that name would be `ls‑lead‑leader‑internal`.

Assume that for your workergroup install, you used a release name of `ls-wg1`, and a values file named `my‑values.yml` with the following contents:

```yaml
config:
  host: logstream-master-internal
  group: kubernetes
  token: criblmaster
  rejectSelfSignedCerts: 0
```

...then you'd replace the `host` value in this file with `ls‑lead‑leader‑internal`, and then run:

`helm upgrade ls-wg1 -f my-values.yml -n <namespace>`

The upgrade **should** replace all the existing workergroup pods with newly reconfigured ones. However, if you notice any workergroup pods with an AGE value indicating that it was started before the upgrade command, simply kill those pods, and they will re-spawn with the new configuration. 

## Preloading Configuration

The `extraConfigmapMounts` and `extraSecretMounts` options enable you to preload configuration files into the leader chart, via ConfigMaps and Secrets that you've created in your Kubernetes environment. However, because ConfigMaps and Secret mounts are read-only, you can't simply mount them into the configuration tree. 

Therefore, you must mount them to a location outside of the `/opt/cribl` tree, and then copy the files into the tree at startup. This copying can be accomplished using environment variables, as we'll see [below](#env-vars). 

> Both ConfigMaps and Secret mounts can be made writable, but the K8s documentation recommends against this.
>
{.box .info}

#### Configuration Locations

The chart creates a single configuration volume claim, `config-storage`, which gets mounted as `/opt/cribl/config-volume`. All Worker Group/Fleet configuration lives under the `groups/` subdirectory. If you have a Worker Group/Fleet named `datacenter_a`, its configuration will live in `/opt/cribl/config-volume/groups/datacenter_a`. See [Configuration Files](configuration-files) section for details on file locations.

####  Using Environment Variables to Copy Files  {#env-vars}

The cribl container's `entrypoint.sh` file looks for up to 30 environment variables assumed to be shell-script snippets to execute before Cribl Stream startup (`CRIBL_BEFORE_START_CMD_[1-30]`). It also looks for up to 30 environment variables to execute after Cribl Stream startup (`CRIBL_AFTER_START_CMD_[1-30]`). 

The variables in each set need to be in order, and cannot skip a number. (The `entrypoint.sh` script breaks the loop the first time it doesn't find an env var, so if you have `CRIBL_BEFORE_START_CMD_1` skipping to `CRIBL_BEFORE_START_CMD_3`, then `CRIBL_BEFORE_START_CMD_3` will not be executed.)

The chart uses this capability to inject the license and to set up groups. We'll use this same capability to copy our config files into place. So if you've provided the `config.license` and `config.groups` variables (occupying the first two slots), you'll need to start with `CRIBL_BEFORE_START_CMD_3`. In the examples below, we'll start with `CRIBL_BEFORE_START_CMD_3`, assuming that a `config.license` and `config.groups` have been set. 

##### Figuring Out Which Variable to Use

The easiest way to figure out which environment variable you need to use is to deploy the chart with all the options you plan to use (i.e., to use the `helm install` command with options that you plan to use for your deployment). Then check the pod definition for `CRIBL_*` environment variables. For example, if you used the following install command:

```shell
% helm install lsms -f ../leader-values.yaml -n logstream-ht cribl/logstream-leader
```

You can now get the pod's name: 

```shell
% kubectl get pods -n logstream-ht
NAME                                           READY   STATUS    RESTARTS   AGE
lsms-leader-659bfccdd6-xsz67                   1/1     Running   0          52m
```

And then you can use `kubectl describe` to get the relevant environment variables:

```shell
% kubectl describe  pod/lsms-leader-659bfccdd6-xsz67 -n logstream-ht  | egrep "CRIBL_.*START"
CRIBL_BEFORE_START_CMD_1:      if [ ! -e $CRIBL_VOLUME_DIR/local/cribl/licenses.yml ]; then mkdir -p $CRIBL_VOLUME_DIR/local/cribl ; cp /var/tmp/config_bits/licenses.yml $CRIBL_VOLUME_DIR/local/cribl/licenses.yml; fi
CRIBL_BEFORE_START_CMD_2:      if [ ! -e $CRIBL_VOLUME_DIR/local/cribl/mappings.yml ]; then mkdir -p $CRIBL_VOLUME_DIR/local/cribl;  cp /var/tmp/config_bits/groups.yml $CRIBL_VOLUME_DIR/local/cribl/groups.yml; cp /var/tmp/config_bits/mappings.yml $CRIBL_VOLUME_DIR/local/cribl/mappings.yml; fi
CRIBL_AFTER_START_CMD_1:       [ ! -f $CRIBL_VOLUME_DIR/users_imported ] && sleep 20 && cp /var/tmp/config_bits/users.json $CRIBL_VOLUME_DIR/local/cribl/auth/users.json && touch $CRIBL_VOLUME_DIR/users_imported
```

From that, you can tell that we already have a `CRIBL_BEFORE_START_CMD_1` and `CRIBL_BEFORE_START_CMD_2`, so our next logical variable should be `CRIBL_BEFORE_START_CMD_3`. 

#### Preloading Scenario

Here's a preload scenario that includes a sample ConfigMap, `extraConfigmapMounts`, copy command, and copy-once flag.

#### The ConfigMap
Let's say we want to preconfigure a collector job in the `group1` Worker Group. The job will be called `InfrastructureLogs`, and it will read ELB logs from an S3 bucket. First, we'll need a `jobs.yml` file, like this:

```yaml
InfrastructureLogs:
  type: collection
  ttl: 4h
  removeFields: []
  resumeOnBoot: false
  schedule: {}
  collector:
    conf:
      signatureVersion: v4
      enableAssumeRole: true
      recurse: true
      maxBatchSize: 10
      bucket: <my infrastructure logs bucket>
      path: /ELB/AWSLogs/${aws_acct_id}/elasticloadbalancing/${aws_region}/${_time:%Y}/${_time:%m}/${_time:%d}/
      region: us-west-2
      assumeRoleArn: arn:aws:iam::<accountid>:role/LogReadAssume
    destructive: false
    type: s3
  input:
    type: collection
    staleChannelFlushMs: 10000
    sendToRoutes: false
    preprocess:
      disabled: true
    throttleRatePerSec: "0"
    breakerRulesets:
      - AWS Ruleset
    pipeline: devnull
    output: devnull
```

We'll need this loaded into a ConfigMap object, so we'd run kubectl to create a ConfigMap from the directory where our `jobs.yml` file resides:

`kubectl create configmap job-config --from-file <containing directory> -n <deployment namespace>`

So if that file is in a directory called `./config-dir`, and we're deploying the leader chart into the `logstream` namespace, we'd create it like this:

`kubectl create configmap job-config --from-file ./config-dir -n logstream`

##### extraConfigmapMounts Config

In our `values.yaml` file, we need to specify the ConfigMap and where to mount it:

```yaml
extraConfigmapMounts:
  - name: job-config
    configMap: job-config
    mountPath: /var/tmp/job-config
```	

This example will mount the files in the ConfigMap into the pod's `/var/tmp/job-config` directory. 

##### Copying the Config Files

You could simply define, in the `values.yaml` file (or via `--set`):

```yaml
env:
  CRIBL_BEFORE_START_CMD_3: "cp /var/tmp/job-config /opt/cribl/config-volume/groups/group1/local/cribl/jobs.yml"
```

However, there are two potential problems with that:
1. There is no guarantee that the destination directory tree will be there. (The first time a pod spins up, it won't be.)
2. If the pod has crashed and spun up anew, blindly copying will overwrite any changes previously made. This is rarely desirable behavior.

##### File Copying Pattern

Since we might want to copy multiple configuration files in one shot, it makes sense to use some sort of "flag file" to ensure that we copy the files only once. The script snippet to copy the `jobs.yaml` file looks like this, formatted for readability:

```yaml
FLAG_FILE=/opt/cribl/config-volume/job-flag
if [ ! -e $FLAG_FILE ]; then
  mkdir -p /opt/cribl/config-volume/groups/group1/local/cribl # ensure the directory tree exists
  cp /var/tmp/job-config/jobs.yml /opt/cribl/config-volume/groups/group1/local/cribl # copy the file
  touch $FLAG_FILE
fi
```

This looks to see if the file `/opt/cribl/config-volume/job-flag` exists, and if it doesn't, creates the directory tree, copies the config file(s), and then creates the job flag file. However, we need to format it a little differently to easily encompass it in the `env` variable:

```yaml
env: 
  CRIBL_BEFORE_START_CMD_3: "FLAG_FILE=/opt/cribl/config-volume/job-flag; if [ ! -e $FLAG_FILE ]; then mkdir -p /opt/cribl/config-volume/groups/group1/local/cribl; cp /var/tmp/job-config/jobs.yml /opt/cribl/config-volume/groups/group1/local/cribl; touch $FLAG_FILE; fi"
```

Once you run `helm install` with this in the `values.yaml` file, you can do `kubectl exec` on the pod to execute a shell:

`kubectl exec -it <pod name> -- bash`

...and then look at `/opt/cribl/config-volume/groups/group1/local/cribl/jobs.yml` to verify that it is in place. 

## Uninstall the Infrastructure

To spin down deployed pods, use the [helm uninstall](https://helm.sh/docs/helm/helm_uninstall/) command – where `<release‑name>` is the namespace you assigned when you installed the chart: <br/>
  `helm uninstall <release-name>`

You can append the `--dry-run` flag to verify which releases will be uninstalled before actually uninstalling them: <br/>
  `helm uninstall <release-name> --dry-run`

## Known Issues

* Cribl's current architecture supports **only** TCP ports in Worker Groups'/Fleets' `service` > `ports` configuration. This restriction might be removed in future versions.

* The upgrade process from pre-2.4.0 versions creates an `initContainer`, which will run prior to any instance of the Cribl Stream pod. Because the coalescence operation will not overwrite existing data, this is not a functional problem. But depending on your persistent-volume setup, the `initContainer`'s precedence might cause pod restarts to take additional time while waiting for the volume claims to release. The only upgrade path that will have this issue is 2.3.* -> 2.4.0. In the next iteration, we'll remove the `initContainer` from the upgrade path. 

* The upgrade process leaves the old `PersistentVolume`s and `PersistentVolumeClaim`s around. This is, unfortunately, necessary for this upgrade path. In follow-on versions, we will remove these volumes  from the chart.

* See [EKS-specific issues](https://github.com/criblio/helm-charts/blob/master/common_docs/EKS_SPECIFICS.md#known-eks-problems) on our GitHub repo.
