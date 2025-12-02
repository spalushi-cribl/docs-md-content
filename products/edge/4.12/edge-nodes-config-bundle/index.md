# Managing Edge Node Config Bundles


Learn about Edge Node config bundles and how to manage them

---

## Config Bundles
Config bundles are compressed archives of all config files and associated data
that an Edge Node needs to operate. Both Leader and Worker Nodes automatically manage config bundles in the following ways.

### Leader Node Bundles
The Leader creates bundles upon Deploy, and
manages them as follows:

- Bundles are wiped clean on startup.
- While running, at most 5 bundles per group are kept.
- Bundle cleanup is invoked when a new bundle is created.

### Edge Node Bundles
The Edge Node pulls bundles from the Leader and manages them as follows:

- Last 5 bundles and backup files are kept.
- At any point in time, all files created in the last 10 minutes are kept.
- Bundle cleanup is invoked after a reconfigure.

## Bundle in S3 {#remote-bundle}

You can store bundles in an AWS S3 bucket to reduce the Leader load. This offloads bundle distribution from the Leader to your designated S3 bucket. Edge Nodes will pull bundles directly from S3, minimizing Leader strain.

You can configure bundling in S3 through the following ways:

- **UI**: When [configuring](setting-up-leader-and-edge-nodes#config-leader) **Leader Settings** on a Leader Node, specify an S3 bucket (format: `s3://${bucket}`) for remote bundle storage in the **S3 Bundle Bucket URL** field.
- **YAML**: In the [`instance.yml` config](instanceyml) file, specify the S3 bucket in `master.configBundles.remoteUrl`.
- **Environment Variable**: Use the `CRIBL_DIST_LEADER_BUNDLE_URL` [environment variable](environment-variables#distributed).

### Optimized Bundle Delivery from S3 {#optimized-S3bundle}

Cribl prioritizes efficient updates for Edge Nodes, regardless of their network environment. Here's how:

- **Direct S3 Downloads (Preferred)**: Cribl prioritizes downloading upgrade packages directly from Amazon S3 (S3) whenever possible, ensuring efficient updates for Edge Nodes connected to Cribl.Cloud or an on-premise Leader. The source is specified in the Leader's **Global Settings** > **Upgrades** > **Package source** setting.
- **Automatic Failover**: If the S3 download fails, a built-in failover automatically retrieves the bundle from the Leader itself (port `4200`). This redundancy guarantees updates even in challenging network environments, including private networks with restricted internet access. For these networks, Cribl prioritizes a CDN download before reverting to the Leader Node as a final resort.
- **Allowlist S3 Access (Optional)**: To further optimize downloads for Edge Nodes with internet access, consider adding the S3 access (`*.s3.amazonaws.com`) to the allowlist in your firewalls or proxies (due to non-static S3 addresses). This proactive step helps avoid disruptions.
  
This multi-step approach ensures efficient and reliable Edge Node updates in various network configurations.

### Authenticating to S3

When storing configuration bundles in an Amazon S3 bucket, Cribl Edge needs appropriate credentials to access the bucket. There are several ways to provide these credentials, with IAM roles being the most secure and recommended approach.

#### IAM Roles (Recommended)

The best practice is to configure an IAM role with read access to your S3 bucket and attach this role to the EC2 instance (or other AWS service) where your Cribl Edge Leader and Edge Nodes are running. Cribl Edge will automatically use the instance's IAM role to authenticate to S3. This method avoids the need to manage AWS credentials directly within Cribl and is the most secure option.

To authenticate via IAM roles: 

1. Create an IAM role with the `AmazonS3ReadOnlyAccess` policy (or a custom policy with more restrictive permissions if needed) for the S3 bucket containing your bundles.
2. Attach this IAM role to the EC2 instance(s) running your Cribl Edge Leader and Worker Nodes.
3. Provide the path to your S3 bucket using the [CRIBL_DIST_LEADER_BUNDLE_URL environment variable](environment-variables#distributed) or the [S3 Bundle Bucket URL field in the **Distributed**/ **Leader** settings](setting-up-leader-and-edge-nodes#use-the-ui) (For example, `s3://your-bucket-name/path/to/bundles`). `path/to/bundles` is optional.  

#### AWS Access Keys

Alternatively, you can provide AWS access keys directly. Ensure the environment in which Cribl Edge runs in contains the following variables:

```bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_ACCESS_KEY"
export CRIBL_DIST_LEADER_BUNDLE_URL="s3://your-bucket-name/path/to/bundles"
```

### Encryption and Backward Compatibility {#compatibility}

If your Edge Nodes are running versions older than 4.9.0 and your Leader is v.4.9.0 or newer, deployments will always use Leader-hosted bundles. This happens because of how encryption works: 

- Newer Leaders encrypt bundles when uploaded to S3 to enhance security and prevent the exposure of sensitive information like secret keys.
- Older Edge Nodes check the code for errors by comparing its checksum (a unique identifier).
- Since the Edge Nodes check the encrypted code, they will always find errors because the checksum changes after encryption.

You might see an error similar to the one below in your logs:
`Checksum mismatch, expected=f781b2bace2e1869e840b0ee200b57378ecae985, found=a59af697c140521ea7f42af7630ac1fce88e4ff0"**`