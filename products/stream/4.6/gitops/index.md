# GitOps


With Cribl Stream's GitOps features, you can manage Cribl Stream configuration with standard version-control systems and CI/CD flow. You can separate development from production configurations, and thus, safely build and continuously deploy your observability pipelines. The production environment will be read-only, and thus strongly protected against unauthorized or unintended changes. This intentionally restrictive approach is a good fit for some but not all organizations.

> The GitOps workflow described here assumes that your development, staging, and production environments are identical or nearly identical. If, on the other hand, your development environment differs significantly from production (in its branching structure, for example, or if users have wide latitude for experimentation), you should consider solutions other than GitOps.
> 
> GitOps requires an Enterprise license and a distributed Cribl Stream deployment.
>
{.box .success}

> Cribl.Cloud does not support GitOps.
>
{.box .cloud}

## How to Use This Doc

This doc has three sections. 

* Work through all three sections, in order, once. 
* Once you have done that, you are finished with the first two sections, [Preparing Your Git Repo](#preparing-repo) and [Preparing Cribl Stream](#preparing-cribl).
* Next, incorporate the HTTP request from the third section ([GitOps Workflow](#workflow)) into your CI/CD tool infrastructure.
* From then on, your normal production routine will be to just repeat the GitOps Workflow (that is, the third section) again and again.

You'll perform setup steps in the Cribl Stream UI, in your Git management system, and at the command line. Screenshots are in light mode for Cribl Stream, and in dark mode for GitHub examples.

Environment and branch names have been chosen to help you keep track of what you're doing – if you decide to use different ones, exercise care. 

Procedures begin with instructions that are agnostic to the Git management system you want to use, whether that is free GitHub, GitHub Enterprise, GitLab, Bitbucket, or a self-hosted Git server. The detailed instructions that follow are for use with free GitHub. If you're using one of the other Git management systems, you'll need to substitute appropriate instructions for the ones that begin "To do this in GitHub ..."

Check out Cribl's free [GitOps sandbox](https://sandbox.cribl.io/course/gitops) for a "deep dive" and to work through a practical example.

If you encounter either of the following situations, see the linked appendix: 

* You need to [upgrade](#upgrade-with-gitops) your GitOps-enabled Cribl Stream deployment.
* You are not using systemd, and need to override the defaults using [environment variables](#env-vars). 

## Preparing Your Git Repo {#preparing-repo}

The procedure in this section only needs to be done once. When you've completed it, you can [prepare Cribl Stream](#preparing-cribl) for GitOps.

### Creating SSH Public and Private Keys {#creating-keys}

You'll use SSH to connect to your Git management system; Cribl does **not** recommend using basic authentication for GitOps.

At the command line, generate public and private SSH keys, specifying [Ed25519](https://en.wikipedia.org/wiki/EdDSA) as the key type: 
   ```shell
  ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
Verify that the `~/.ssh/` directory contains both of the newly-generated keys:
* `id_ed25519` (the private key that Cribl Stream will use).
* `id_ed25519.pub` (the public key that your Git management system will use).

Copy the public SSH key to your clipboard:
   ```shell
   cat ~/.ssh/id_ed25519.pub
   ```

Associate the public key with your Git management account. 

To do this in GitHub: 

1. If you do not have a GitHub account, [create one](https://github.com/signup).
1. Sign in to your GitHub account.
1. On the [SSH and GPG keys page](https://github.com/settings/keys), click **New SSH key**.
1. Paste the key in the **Key** field.
1. Click **Add SSH key**.

### Creating a Private Git Repo {#creating-repo}

You need to create a dedicated, private Git repo – your Development environment will write to it, and your Production environment will sync to it. Create a private repo named `cribl` that contains one branch, named `main`, now.

To do this in GitHub:

1. In your GitHub [profile](https://github.com/settings/profile) page, click **Go to your personal profile**.
1. On the resulting page, select the **Repositories** tab.
1. Click **New** to create a new repository.
1. Name the repo `cribl`.
1. Change the repo from **Public** to **Private**.
1. Click **Create repository**. The new repo's landing page will open. 
1. In the Quick setup box, click **creating a new file** to open the text editor. 
   > This step and the next are **required** because they establish the commit history that procedures in the next section depend on.
   {.box .warning}
1. Enter some text, enter a name for the file, and click **Commit changes** (once in the text editor and again in the pop-up).


By default, the repo will contain one branch named `main`.

### Setting Up the `prod` Branch {#setting-up-prod}

We've chosen names that always make it clear whether we're talking about environments or about branches:
* Development and Production will be your Cribl Stream environments.
* `dev` and `prod` will be the branches in your Git repo.

In this section, we'll set up `prod`; later on, we'll set up `dev`.

Recall that your Git repo has one branch, named `main`. Now, rename `main` as `prod`. 

To do this in GitHub:
 
1. In your GitHub repo's landing page (e.g., `https://github.com/<your_Git_username>/cribl`), select the **1 branch** tab. (That's what the branches tab will be named, assuming that the sole branch is `main`).
   
   ![The branches tab](gitops-01a.4e4b353c53.png)
2. In the `main` branch's row, click the pencil icon at far right, and rename `main` as `prod`.

## Preparing Cribl Stream {#preparing-cribl}

The procedure in this section only needs to be done once. When you've completed it, you can move on to [testing and using](#workflow) GitOps.

### Setting Up Your Environments {#setting-up}

In your distributed Cribl Stream deployment, set up two separate environments, one named `Development` and the other named `Production`. Cribl strongly recommends that you use [systemd](deploy-single-instance#systemd-boot) to manage the `cribl` service. 

Complete the following steps in **both** environments:

* Create one Cribl Leader and two Cribl Workers.
* On the Leader, install a Cribl Enterprise License.
* On the Leader, install `git` using the following command (or the equivalent for your OS):  
   `sudo yum install git -y`

Then, in the Production environment:

* On the Leader, install `jq` – the GitOps workflow instructions [below](#sync-to-git) use `jq`.
    * Run this command (or the equivalent for your OS):  
   `sudo yum install jq -y`
* Note the Production Leader's IP address for use later on.

### Configuring Your Production Leader {#config-production-leader}

First, you'll ensure that your Production Leader and your Git repo can communicate; then, you'll put your Production Leader into read-only mode.

#### Creating the `gitops` User {#create-user}

1. Create a new local user called `gitops`. 
1. On the Production Leader's local filesystem, temporarily record the password for the `gitops` user; we'll refer to it as `GITOPS_USER_PASSWORD`.
1. Associate the default `gitops` [Role](roles) with the `gitops` user.
    * This Role's only permission is to `POST` to the `/version/sync` endpoint of the Cribl API (in the [API Reference](/api-reference), see **versioning**).

#### Adding a Secret to Git {#upload-secret}

Your Git repo needs to be able to communicate with your Production Leader. This requires that you store a secret in your Git management system. We'll use `GITOPS_USER_PASSWORD` as the secret. 

From the Production Leader, upload the `GITOPS_USER_PASSWORD` to the Git management system (or perform an equivalent operation) now. 

To do this in GitHub:
1. In the GitHub site for your repo, navigate to **Settings** > **Secrets and variables**.
1. Click **New repository secret** and follow the prompts.

When done, you should see a `GITOPS_USER_PASSWORD` row under **Actions secrets**:

![The GitOps user password has been uploaded to GitHub](gitops-02.fd192e8dea.png)
{border="true"}

#### Connecting Your Production Environment to Your Git Repo {#connect-to-repo-prod}

In Cribl Stream, in the UI for the Production Leader, select global ⚙️ **Settings** (lower left) > **Git Settings**.

In the **Remote** tab:
1. In **Remote URL**, enter the URL for your private Git repo. 
    * On GitHub, the **Remote URL** will look like this:  
   `git@github.com:<your_GitHub_username>/cribl.git`
1. Make sure that in the **Authentication type** drop–down, `SSH` (the default) is selected.
1. In **SSH private key**, copy and paste the private key (`~/.ssh/id_ed25519`).
   > The key will paste in with an appended newline below it. Do not delete this newline before you save the Remote Settings, or else you will trigger an error of the form: `fatal: Could not read from remote repository.`
   {.box .warning}
1. Toggle **SSH strict host key checking** off. This will allow the remote repository to accept your host key information. If you miss this step, you will trigger an error. 
1. Click **Save**.
1. Toggle **SSH strict host key checking** on again.
1. Click **Save** (again!).
3. **Commit** and **Push**.
   
In the **General** tab:

1. Set **Branch** to `prod`.
1. Leave **GitOps Workflow** set to `None`.
2. Click **Save**.

Check your private Git repo – in the `prod` branch, you should now see your commit.

#### Making the Production Leader Read-Only {#make-leader-ro}

In a terminal on the Production Leader, become root:
  ```shell
  sudo su -
  ```

Add `systemctl` overrides to set environment variables that will (a) make the Production Leader read-only upon startup, and (b) set the branch to `prod`.
1. Use `systemctl` to open the `override.conf` file in an editor:  

   ```bash
   systemctl edit cribl 
   ```
2. When [prompted](deploy-single-instance#persisting-overrides), add the following overrides:

   ```bash
   [Service]
   Environment=CRIBL_GIT_OPS=push
   Environment=CRIBL_GIT_BRANCH=prod
   ```
3. Restart: 

   ```bash
   systemctl daemon-reload && systemctl restart cribl
   ```


### Creating the `dev` Branch

Recall that your Git repo has one branch, named `prod`. Now, create a new branch named `dev`, **from your existing** `prod` branch.

To do this in GitHub:
 
1. In your GitHub repo's landing page (e.g., `https://github.com/<your_Git_username>/cribl`), select the **1 branch** tab. (That's what the branches tab will be named, assuming that the sole branch is `prod`).
   
   ![The branches tab](gitops-01c.png)
1. Click **New branch** to open the **Create a branch** modal.
2. Make sure that **Source** is set to `prod`.
3. In the **New branch name** field, enter `dev`.
   
   ![Creating the `dev` branch](gitops-01d.png)
4. Click **Create new branch** to close the modal.
5. Back in the **Branches** tab, click **Overview** to refresh the tab – you should now see both the `prod` and `dev` branches listed.

### Configuring Your Development Leader {#config-dev-leader}

First, you'll point your Development Leader to the `dev` branch in the remote repo. Then, you'll ensure that one or more users exist that can push configs to that repo.

### Pointing Your Development Leader to Its Branch {#pointing-dev-leader-to-branch}

In Cribl Stream, in your Development Leader's UI, select global ⚙️ **Settings** (lower left) > **Git Settings**.

In the **Remote** tab:
1. In **Remote URL**, enter the URL for your private Git repo – and if the URL includes a Git username, this should be `gitops`, the one you created [above](#create-user). 
    * On GitHub, the **Remote URL** will look like this:  
   `git@github.com:<your_GitHub_username>/cribl.git`
1. Make sure that in the **Authentication type** drop–down, `SSH` (the default) is selected.
1. In **SSH private key**, copy and paste the private key (`~/.ssh/id_ed25519`).
   > The key will paste in with an appended newline below it. Do not delete this newline before you save the Remote Settings, or else you will trigger an error of the form: `fatal: Could not read from remote repository.`
   {.box .warning}
1. Toggle **SSH strict host key checking** off. This will allow the remote repository to accept your host key information. The remote host’s information is stored in `~/.ssh/known_hosts`. If you miss this step, you will trigger an error.
1. Click **Save**.
1. Toggle **SSH strict host key checking** on again. 
2. Click **Save** (again!), but do not **Commit** or **Push** yet.

In the **General** tab: 

1. Set **Branch** to `dev`.
1. Leave **GitOps Workflow** set to `None`.
2. Click **Save**, but do not **Commit** or **Push** yet.

Restart the Development Leader. When it comes back up, it will be able to write to your Git repo.

#### Setting Up Users to Push to the Remote Repo {#set-up-users}

You will need one or more users capable of pushing configs to the remote repo. There is more than one way to achieve this. Your choice will depend on the needs of your organization.

* Any user with the `admin` role already has ability to push to the remote repo. If this is your preferred approach, skip to the [next section](#pointing-dev-leader-to-branch).
* If you prefer to allow one or more **regular** users to push to the remote repo, **without** having the power of the `admin` role, you can create such users by completing the procedure below.

First, edit the `roles.yml` file to make a new `push_to_remote` role. (We're using the command line here because you cannot perform this operation in the UI.)

1. SSH into the Leader node.
1. `cd` into your `$CRIBL_HOME/local/cribl` directory.
1. Open or create the `roles.yml` file in a text editor.
1. Without changing the existing text in the file, paste in the following snippet:

   ```yaml 
   push_to_remote:
     policy:
       - POST /version/sync
       - POST /version/commit
       - POST /version/push
       - GET /system/settings/git-settings
     description: Members with this role can push configs to a remote repo.
   ```
1. Restart Cribl Stream.

Next, back in the UI, create a new `config_pusher` user.

1. In global ⚙️ **Settings** (lower left) > **Access Management** > **Local Users**, click **Add User** to open the user creation modal.
1. Name the new local user `config_pusher`.
1. In addition to the usual configuration steps, use the **Roles** drop-down to associate the `push_to_remote` role with the `config_pusher` user.
1. Click **Save**.
1. **Commit** and **Push**.

#### Verify That Your New User Can Push to the Remote Repo {#verify-user}

1. Log out, then log back in as `config_pusher`.
1. The **Version Control** drop-down should now display a **Push** button.

Later, when you are ready to push new configs to the remote repo, click this new **Push** button.

## GitOps Workflow {#workflow}

If you've completed the above preparatory sections, your GitOps workflow is ready go. Here's an overview, with links to detailed procedures.

* **[Work in the Development Environment](#working-in-dev)**: All your commits and testing happen in the Development environment first; you push changes to the `dev` branch in your Git repo.
* **[Merge Changes from dev to prod](#merging-from-dev-to-prod)**: When you are satisfied that your changes are ready for production, you create a pull request that brings the `prod` branch up-to-date with `dev`. This is all in your Git management system, and changes nothing in your Cribl Stream Production environment.
* **[Sync Your Production Leader to Git](#sync-to-git)**: For your changes to take effect in production, you will send an HTTP request to the `/version/sync` endpoint of the Cribl API, on the Production Leader. Once you are in production, it will be your CI/CD infrastructure – not you – that sends the request. This will cause the Production Leader to pull in the latest changes from your Git repo's `prod` branch – without violating the Production Leader's read-only status.

The procedures include a harmless test task so that you can validate your setup and acquire a feel for the workflow.

### Working in the Development Environment {#working-in-dev}

In this step, you'll make changes in the Development environment, and subsequently propagate them to Production. To test this with a minimal unit of work, start by adding a Route. 

In Cribl Stream, in the Development Leader's UI, navigate to **Routing** > **Data Routes**. 
1. Click **Add Route**.
1. Configure the new Route as follows:

     | Name | Filter | Pipeline | Output |
     | ----------- | ----------- | ----------- | ----------- | 
     | `test-gitops` | `true` | `devnull` | `devnull:devnull` |

1. Click **Commit**.

1. In the **Git Changes** modal, add a commit message and click **Commit & Deploy**.

1. Finally, [push the changes to your remote repo](/stream/version-control#pushing-to-a-remote-repo) via the **Global Config** dialog, whose appearance will vary depending on how you've configured **Git Settings** > **Collapse Actions**. Make sure the dialog says that the **Git branch** you're pushing to is `dev`, as configured [above](#pointing-dev-leader-to-branch).

### Merging Changes from `dev` to `prod` {#merging-from-dev-to-prod}

In your Git management system's `dev` branch, you should see the new `route.yml` appearing in the `/groups/<group_name>/local/cribl/` directory. 

Merge the changes from the `dev` branch to `prod` now. 

To do this in GitHub: 

1. Click **Compare & pull request**. Be sure that the `base` drop-down says `prod` and the `compare` drop-down says `dev`.

    ![Comparing `dev` to `prod`](gitops-04.ef72cde61b.png)
1. Add some comments.
1. Click **Create pull request**. The UI should say that you want to `merge 1 commit into prod from dev`.
1. If GitHub detects conflicts, fix them.
1. Have the appropriate person on your team review the pull request.
1. You (or the appropriate person) should now click **Merge pull request**, then **Confirm merge**.

If the merge is successful, GitHub will display a confirmation message. Your committed changes have now been merged from `dev` to `prod`.

### Syncing Your Production Leader to Git {#sync-to-git}

Your changes so far have all been done in the Development environment. Meanwhile, you have committed to the `dev` branch, and pushed to the `prod` branch **in your Git management system** – but your Production **environment** has not changed. 

Since, for safety's sake, you made the Production environment read-only, you can neither write to it nor update it via a UI. Instead, the following must happen: 

* You will send an HTTP request to the `/version/sync` endpoint of the Cribl API, on the Production Leader (in the [API Reference](/api-reference), see **versioning**). Once you are in production, it will be your CI/CD infrastructure – not you – that sends the request.

* This will cause the Production Leader to pull in the latest changes from your Git repo's `prod` branch – without violating the Production Leader's read-only status.

Below you'll see a script you can use to send the HTTP request. 

* This script is generic, and will require you to adapt it to work with the Git management system you're using. 
* Once you run the script, you should see the changes you made in your Development environment (in this example, the new Route) in the Production environment, too.

#### Syncing to Git: Generic Script {#sync-to-git-generic}

Substitute your Production Leader's IP address and the `gitops` user's password for the placeholders.

```shell
#!/bin/bash

# Authenticate and Save the Token.
TOKEN=$(curl http://<Production_Leader_IP_address>:9000/api/v1/auth/login \
     -H 'Content-Type: application/json' \
     -d '{"username":"gitops","password":"$GITOPS_USER_PASSWORD"}' 2>/dev/null | \
     jq -r .token)

# Set up the Authentication Header
export AUTH_HEAD="Authorization: Bearer $TOKEN"

# Make the notification call
curl -X POST "http://<Production_Leader_IP_address>:9000/api/v1/version/sync" -H "accept: application/json" -H "${AUTH_HEAD}" -d "ref=prod&deploy=true"
```

#### Making the HTTP Request from CI/CD {#ci-cd}

Now that you have run one of the above scripts to prove that your GitOps workflow works, you should incorporate the script's HTTP request into your CI/CD setup. Once that is done, you're in production with GitOps! To develop, test, and push changes to production, just keep running through the whole GitOps [workflow](#workflow). 

## Appendix: Upgrading {#upgrade-with-gitops}

Upgrading a Cribl Stream GitOps deployment is essentially a variation on the day-to-day GitOps workflow described in the previous section, with the key difference that you take the Production environment out of, then put it back into, read-only mode. 

Once you have begun the upgrade, **do not** make any changes unrelated to the upgrade process, in either the Development or Production environment, until the upgrade process is complete. For example, do not start creating any new Routes or Pipelines.

First, in your Development environment:
1. Validate that Cribl Stream is working as expected, and that the latest changes have been pushed to the `dev` branch in your Git repo.
1. Stop the Cribl Stream Leader and back up `$CRIBL_HOME` (the Cribl install directory).
1. Use the UI to upgrade the Leader and Workers.
1. Create a PR to merge the `dev` branch into `prod`. Merge the PR.

Next, in your Production environment: 
1. Stop the Cribl Stream Leader and back up `$CRIBL_HOME` (the Cribl install directory).
1. Take your Production environment **out of read-only mode** according to the instructions [below](#undo-ro).
1. Use the UI to upgrade the Leader and Workers – but **do not** commit changes after the upgrade.
1. Put the Leader **back into read-only mode** according to the instructions [above](#make-leader-ro).
1. Sync the Production environment to the `prod` branch in your Git repo.

Finally, confirm that all Workers in both environments have the latest configuration.

#### Taking the Production Leader Out of Read-Only Mode {#undo-ro}

In a terminal on the Production Leader, become root:

```bash
sudo su -
```

Comment out the `systemctl` overrides that set environment variables that (a) had made the Production Leader read-only upon startup, and (b) had set the branch to `prod`.
1. Use `systemctl` to open the `override.conf` file in an editor:  

   ```bash
   systemctl edit cribl
   ```
2. When [prompted](deploy-single-instance#persisting-overrides), comment out the Git-related overrides as shown:

   ```bash
   [Service]
   # Environment=CRIBL_GIT_OPS=push
   # Environment=CRIBL_GIT_BRANCH=prod
   ```
3. Restart: 

   ```bash
   systemctl daemon-reload && systemctl restart cribl
   ```

## Appendix: Environment Variables {#env-vars}

If you are not using systemd to manage the `cribl` service, you can use environment variables to override the defaults in the UI.

| **Name** | **Purpose** |
| --- | --- |
| CRIBL_GIT_REMOTE | Location of the remote repo to track. Can contain username and password for HTTPS auth. |
| GIT_SSH / GIT_SSH_COMMAND | See [Git's documentation](https://git-scm.com/docs/git#Documentation/git.txt-codeGITSSHcode). |
| CRIBL_GIT_BRANCH | Git ref (branch, tag, commit) to track/check out. |
| CRIBL_GIT_DEPLOY | Controls whether to deploy Workers or not. |
| CRIBL_GIT_AUTH | One of: `none`, `basic`, or `ssh`. |
| CRIBL_GIT_USER | Used for `basic` auth. |
| CRIBL_GIT_PASSWORD | Used for `basic` auth. |
| CRIBL_GIT_OPS | Controls which GitOps workflow to use – either `push` or `none`. |
