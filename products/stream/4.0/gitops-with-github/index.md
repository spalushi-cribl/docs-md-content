# GitOps/GitHub Tutorial


This tutorial shows one of many possible ways to set up Cribl Stream GitOps, relying on a GitHub repo with SSH authentication.

If you are using GitLab, Bitbucket, or a self-hosted Git server, you can adapt the GitHub examples here, or you can rely on our generic [GitOps](gitops) page.

> See also our [Managing Stream with GitOps Sandbox](https://sandbox.cribl.io/course/gitops).
>
{.box .success}

## Prerequisites

To use GitOps, you must have:
- A customer-managed (on-prem), [distributed](deploy-distributed) Cribl Stream deployment. (Cribl.Cloud does not currently support GitOps).
- Two Leader instances. 
- A Cribl Enterprise [license](licensing), which can be shared between the two Leaders.
- A GitHub account. (You can create one [here](https://github.com/signup).)

## Setting Up Your Deployment for GitOps {#setting-up}

In your distributed Cribl Stream deployment, set up two separate environments: one for Development and one for Production. This way, you can do your work in the Development environment, validate and test your changes there, and then merge the changes into Production. For safety, the Production environment will be read-only.

In **both** environments:
* On the Leader, install `git` using the command:  
   `sudo yum install git -y`
* Cribl strongly recommends that you use [systemd](deploy-single-instance#systemd-boot), as we do throughout this tutorial. (If not using systemd, use [environment variables](gitops#env-vars) instead of `systemctl` commands.)
* Cribl strongly recommends that you use SSH authentication, as we do throughout this tutorial.

In the Production environment:
* On the Leader, install `jq` using the command:  
   `sudo yum install jq -y`

## Creating SSH Public and Private Keys {#creating-keys}

Starting on the command line:
1. Generate public and private SSH keys, specifying [Ed25519](https://en.wikipedia.org/wiki/EdDSA) as the key type: 
    ```shell
       ssh-keygen -t ed25519 -C "your_email@example.com"
    ```
2. Validate that the `/root/.ssh/` directory contains both of the newly generated keys:
    * The private key that Cribl Stream will use: `id_ed25519`. 
    * The public key that GitHub will use: `id_ed25519.pub`.
3. Copy the public SSH key to your clipboard.

Next, sign in to your GitHub account, and on the [SSH and GPG keys](https://github.com/settings/keys) page, enter the public SSH key:
1. Click **New SSH key**.
2. Paste the key in the **Key** field.
3. Click **Add SSH key**.

> ##### Prevent Push and Pull Errors {{< id `backup` >}}
>
> Before you proceed to the next section: See our [general warnings](version-control#securing-remote-repos) to first locally back up your `$CRIBL_HOME` directory, and to create any new remote as a completely empty repo with no README.
>
{.box .danger}

## Creating a Private GitHub Repo {#creating-repo}

On your GitHub [profile](https://github.com/settings/profile) page: 
1. Click **Go to your personal profile**. 
2. On the resulting page,click the **Repositories** tab.
3. Click **New** to create a new repository.

Name the repo `cribl`, and change its default **Public** setting to **Private**. The repo's default branch will be named `main`, and we'll assume that name in this tutorial. (Older repos' default branch might instead be named `master`.)

A representative repo would have this basic structure:

![Sample repo topography](gitops-repo-structure.47f2b21285.png)
{scale="70%"}

## Configuring Your Production Leader {#config-production-leader}

> You can optionally create a new user with the `gitops` [Role](roles). This Role's only permission is to `POST` to the `sync` endpoint. If you choose to do this, the **Remote URL** that you enter below should include this username.
>
{.box .info}

In Cribl Stream, in the Production Leader's, select **Settings** > **Global Settings** > **Git Settings**. Then apply the following settings.

### General Tab

Leave **Branch** set to `main` (the only branch created thus far), and **GitOps Workflow** set to `None`.

### Remote Tab

* In **Remote URL**, enter the URL for your private Git repo, e.g.:  
   `git@github.com:<your_Git_username>/cribl.git`.
* From the **Authentication type** drop–down, select `SSH`.
* In **SSH private key**, copy and paste the private key (`/root/.ssh/id_ed25519`).
* Toggle **SSH strict host key checking** to `No`.

Click **Save**, then **Commit** and **Git Push**. You should now see your commit in the `main` branch of your private GitHub repo.

In a terminal on the Production Leader, run as root:
  ```shell
     sudo su -
  ```

Add `systemctl` overrides to set environment variables that will make the Production Leader read-only on startup, and set the branch to `prod`:
1. Use `systemctl` to open the `cribl.service` file in an editor:  
  ```shell
     systemctl edit cribl.service 
  ```
2. When [prompted](deploy-single-instance#persisting-overrides), add the following overrides:
  ```shell
      [Service]
      Environment=CRIBL_GIT_OPS=push
      Environment=CRIBL_GIT_BRANCH=prod
  ```
3. Restart:  
  ```shell
     systemctl daemon-reload && systemctl restart cribl
  ```

> Once the system has restarted, the `CRIBL_GIT_OPS=push` override will be in effect, meaning that your Production environment will be in read-only mode.
>
{.box .warning}

## Setting Up the GitHub Branches {#setting-up-branches}

In your GitHub repo: 

1. Click the branches tab (if there's only a `main` branch in your repo, the tab will be labeled **1 branch**).

![The branches tab](usecase-gitops-ssh-23.4e4b353c53.png)

2. On the `main` branch's row, click the pencil button at far right.
3. Rename `main` to `prod`.
4. From the branches drop-down (which should now say `prod`), create a new branch named `dev`.

> Before you proceed to the next section: Switching to a branch whose remote has **not** been hydrated with your valid Cribl Stream configuration can delete your local Stream config. This risk exists whether you switch branches using Stream's UI, or using environment variables. This is why, [above](#backup), we emphasize first creating backups.
>
{.box .danger}

## Pointing Your Development Leader to Its Branch {#pointing-dev-leader-to-branch}

On Cribl Stream's Development Leader, select **Settings** > **Global Settings** > **Git Settings**. Then apply the following settings.

### Remote Tab

* In **Remote URL**, enter the URL for your private Git repo, e.g.:  
   `git@github.com:<your_Git_username>/cribl.git`.
* From the **Authentication type** drop–down, select `SSH`.
* In **SSH private key**, copy and paste the private key (`/root/.ssh/id_ed25519`).
* Toggle **SSH strict host key checking** to `No`.
* Click **Save**.

### General Tab

* Set **Branch** to `dev`. 
* Set **GitOps Workflow** to `None`. 

Click **Save**, then restart the Development Leader, which will now be able to write to GitHub.

## Working in the Development Environment {#working-in-dev}

Your goal is to make changes in the Development environment, and later propagate them to Production. To demonstrate this, try adding a Route, as a minimal unit of work. 

1. On Cribl Stream's Development Leader, navigate to **Routing** > **Data Routes**. 
2. Click **+ Route**.
3. Configure the new Route as follows:

    | Name | Filter | Pipeline/Output |
    | ----------- | ----------- | ----------- | 
    | `test-gitops` | `true` | `devnull:devnull` |

4. Click **Commit/Deploy**. 
5. Click **Commit** and **Git Push**. 

This will send the configurations to your `dev` branch on GitHub.

## Merging Changes from `dev` to `prod` {#merging-from-dev-to-prod}

On your GitHub `dev` branch, you should see the new `route.yml` appearing within a subdirectory of the `local` directory.
1. Click **Compare & pull request**. Be sure that the `compare` drop-down says `dev`, and the `base` drop-down says `prod`:  

![Comparing `dev` to `prod`](usecase-gitops-ssh-24.ef72cde61b.png)

2. Add some comments.
3. Click **Create pull request**. The UI should propose to `merge 1 commit into prod from dev`.
4. Assuming that GitHub does not detect any conflicts, click **Merge pull request**, then **Confirm merge**.  

If the merge is successful, GitHub will display a confirmation message. You should now see the new `route.yml` in both the `dev` and `prod` branches.

## Syncing Your Production Leader to GitHub {#sync-to-github}

Now, to make your Production environment pick up the changes from the `prod` branch, you'll send an HTTP request from the Production Leader to the Cribl API. 

Adapt the following request, substituting your Production Leader's IP address and admin password for the placeholders. 
```shell
    curl -X POST "http://<Production_Leader_IP>:9000/api/v1/version/sync" \
    -H "accept: application/json" \
    -H "Authorization: Bearer $(curl http://<Production_Leader_IP>:9000/api/v1/auth/login \
    -H 'Content-Type: application/json' \
    -d "{\"username\":\"admin\",\"password\":\"<Production_Leader_password>\"}" 2>/dev/null | jq -r .token)" \
    -d "ref=prod&deploy=true"
```

> Nested within the request to `/version/sync` is another request to `/auth/login`. The nested request obtains the Bearer token required by the parent request. For more about Bearer tokens, see [this section](gitops#bearer-token).
>
{.box .info}

You should now see the changes you made in your Development environment (in this example, the new Route) in the Production environment, too.

There's much more you can do with GitOps. To learn about `C.env.CRIBL_GIT_BRANCH` and GitHub Actions, see [Going Further](gitops#going-further).
