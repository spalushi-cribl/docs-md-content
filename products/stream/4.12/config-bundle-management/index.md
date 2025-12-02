# Manage Config Bundles


Cribl Stream uses config bundles, compressed archives containing configuration files and data essential for Worker operation. 

## Config Bundles

Both Leader and Worker Nodes automatically manage config bundles in the following ways.

### Leader Node Bundles

The [Leader Node](deploy-distributed#concept-leader-node) actively manages bundles, performing the following tasks:

- **Cleanup on startup**: Upon startup, the Leader Node clears all bundles.
- **Running state**: The Leader Node maintains a maximum of five bundles per [Worker Group](worker-groups) while running.
- **Automatic cleanup**: Creating a new bundle triggers the deletion of older ones. 

### Worker Node Bundles

The [Worker Node](deploy-distributed#concept-worker-node) pulls bundles from the Leader Node and manages them as follows:

- **Cache**: Worker Nodes cache the latest five bundles and their backups.
- **Recent file retention**: Worker Nodes retain any files created within the last ten minutes.
- **Reconfigure cleanup**: Reconfigured events initiate bundle cleanup.

## Store Bundles Remotely {#remote-bundle}

Optionally, reduce Leader load by storing bundles in an Amazon S3 bucket. This offloads distribution, allowing Worker Nodes and Edge Nodes to pull bundles directly from S3.

### Configure S3 Storage 

You can configure bundling in S3 through the following ways:

- **UI**: While [configuring](setting-up-leader-and-worker-nodes#config-leader-ui) **Leader Settings** on a Leader Node, specify an S3 bucket (format: `s3://${bucket}`) for remote bundle storage in the **S3 Bundle Bucket URL** field.
- **YAML**: Define the S3 bucket URL in the  `master.configBundles.remoteUrl` property of your [`instance.yml`](instanceyml) configuration file.
- **Environment variable**: Set the `CRIBL_DIST_LEADER_BUNDLE_URL` [environment variable](environment-variables#distributed).

### Optimized Bundle Delivery {#optimized-S3bundle}

Cribl Stream prioritizes efficient bundle downloads for Worker Nodes. Here's how:

- **Direct S3 downloads (preferred)**: When possible, Cribl prioritizes direct downloads from Amazon S3 for optimal performance.
- **Private network fallback**: Worker Nodes on private networks attempt to retrieve bundles from a Content Delivery Network (CDN). If blocked, they automatically fall back to downloading from the Leader Node.
- **Optional S3 firewall allowlist**: Consider adding `*.s3.amazonaws.com` to your firewall or proxy allowlist (due to non-static S3 addresses) for Worker Nodes with internet access. This ensures smooth downloads and avoids potential disruptions. 

### Authenticating to S3

When storing configuration bundles in an Amazon S3 bucket, Cribl Stream needs appropriate credentials to access the bucket. There are several ways to provide these credentials, with IAM roles being the most secure and recommended approach.

#### IAM Roles (Recommended)

The best practice is to configure an IAM role with read access to your S3 bucket and attach this role to the EC2 instance (or other AWS service) where your Cribl Stream Leader and Worker Nodes are running. Cribl Stream will automatically use the instance's IAM role to authenticate with S3. This method avoids the need to manage AWS credentials directly within Cribl and is the most secure option.

To authenticate via IAM roles: 

1. Create an IAM role with the `AmazonS3ReadOnlyAccess` policy (or a custom policy with more restrictive permissions if needed) for the S3 bucket containing your bundles.
2. Attach this IAM role to the EC2 instance(s) running your Cribl Stream Leader and Worker Nodes.
3. Provide the path to your S3 bucket using the [CRIBL_DIST_LEADER_BUNDLE_URL environment variable](environment-variables#distributed) or the [S3 Bundle Bucket URL field in the **Distributed**/ **Leader** settings](setting-up-leader-and-worker-nodes#use-the-ui) (For example, `s3://your-bucket-name/path/to/bundles`). `path/to/bundles` is optional. 

No further configuration is required. Cribl Stream will automatically discover and use the instance's IAM role.

#### AWS Access Keys

Alternatively, you can provide AWS access keys directly. Ensure the environment in which Cribl Stream runs in contains the following variables:

```bash
export AWS_ACCESS_KEY_ID="YOUR_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_SECRET_ACCESS_KEY"
export CRIBL_DIST_LEADER_BUNDLE_URL="s3://your-bucket-name/path/to/bundles"
```

### Encryption and Backward Compatibility {#compatibility}

If your Workers are running versions older than 4.9.0 and your Leader is v.4.9.0 or newer, deployments will always use Leader-hosted bundles. This happens because of how encryption works: 

- Newer Leaders encrypt bundles when uploaded to S3 to enhance security and prevent the exposure of sensitive information like secret keys.
- Older Workers check the code for errors by comparing its checksum (a unique identifier).
- Since the Workers check the encrypted code, they will always find errors because the checksum changes after encryption.

You might see an error similar to the one below in your logs:
`Checksum mismatch, expected=f781b2bace2e1869e840b0ee200b57378ecae985, found=a59af697c140521ea7f42af7630ac1fce88e4ff0"**`