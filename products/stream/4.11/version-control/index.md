# Version Control


Tracking, backing up, and restoring configuration changes for single-instance and distributed deployments

---

Cribl Stream integrates with Git clients and remote repositories to provide version control of the Cribl Stream configuration. This integration offers backup and rollback for single-instance and distributed deployments.

These options are separate from the Git repo responsible for version control of Worker configurations, located on the Leader Node in distributed deployments. We cover all these options and requirements on this page.

> You can also use the Cribl API to automate commit and deploy commands. Read [Commit and Deploy Changes](/api/commit-deploy/) in the API documentation for more information.
>
{.box .info}

> Cribl.Cloud deployments [do not currently support](cloud-vs-self-hosted#git-preconfigured) integration with external Git clients or remote repos.
>
{.box .warning}

## Install Git (Local or Standalone/Single-Instance) {#git-installation}

To verify that `git` is available, run:

`git --version`

The minimum version that Cribl Stream requires is: **1.8.3.1.** If you don't have `git` installed, see the official [Git installation site](https://git-scm.com/download).


> Git is a hard requirement for certain Cribl Stream features.
>
{.box .warning}

### Distributed Deployments

For [distributed deployments](requirements#git), `git` **must** be installed and available locally on the host running the Leader Node.

**All configuration changes must be committed before they are deployed.** The Leader notifies Workers that a new configuration is available, and Workers pull the new configuration from the Leader Node.

### Licensing Dashboard

Even on single-instance deployments, the [Monitoring > Licensing](monitoring#licensing) dashboard will display configuration change markers only if you have `git` installed.

## Commit Changes {#committing}

Once you have installed Git, you can commit configuration changes using the `git` CLI. You can also commit changes interactively, using the Cribl Stream UI.

Pending commits have a red dot indicator, as shown in this image. Select **Commit** to proceed.

![Changes pending commit](git-pending-commit.png)
{scale="50%" border="true"}

Next, in the resulting **Git Changes** modal, you can verify the diffs in the configuration changes. Enter a commit message, or select **Generate** to have [Cribl Copilot](/copilot/) (if enabled) create a message for you. Other options here include clearing check boxes for individual files to exclude them from the commit (as shown below), and selecting **Undo All Changes** to reverse the changes instead of committing them.

![Reviewing a pending commit](git-commit.png)
{border="true"}

When you're ready to commit to your changes, select **Commit**. Look for a **Commit successful** confirmation message at the lower right.

## Commit and Deploy Controls {#commit-deploy}

[Snippet not found: content/shared/4.11/snippets/_global-commit-and-worker-group-commit.md]

## Revert Commits {#reverting}

If you want to revert a commit, you can do it:

- [In Cribl Stream UI](#revert-commits-in-the-ui)
- [Via `git` CLI](#revert-commits-via-cli)

### Revert Commits in the UI

Reverting a commit in the UI proceeds slightly differently for Single-instance and for Distributed deployments.
In a Distributed deployment, you revert commits for each Worker Group separately.

To revert a commit in the UI:

1. If you are on a Distributed deployment, in the sidebar, select **Worker Groups**, then select the Worker Group you want to revert the commit for.
2. Select the commit you want to revert. To do this:
   - in a Distributed deployment, use the drop-down listing commits,
   - in a Single-instance deployment, open the **Version Control** menu on the top bar.
   (Note that you're not selecting a single commit to roll back, but the *commit to roll back to*.)

![Revert to a previous commit](git-restore-select-commit.png)
{scale="70%" border="true" caption="Selecting commit to revert in Single-instance (left) and Distributed (right) deployment"}

3. Then, in the resulting **Git History** modal, verify the diffs in the configuration changes and select **Revert to**.

![Diff preview before reverting a commit](git-revert-ui-4.9.1.png)
{scale="60%" border="true"}

4. Confirm the revert. This will cause Cribl Stream to restart.

### Revert Commits via CLI

To revert a commit via CLI:

1. Make sure you have [committed](#committing) all your recent configuration changes.
1. Stop Cribl Stream.
1. In your terminal, navigate to your [`$CRIBL_HOME`](environment-variables#cribl_home) location (typically, `/opt/cribl`).
1. Run `git log` to get a list of most recent commits and find the hash of the commit you want to revert.
1. Run `git revert <commit-hash>` to revert this commit.
   
   If you want to revert more than one commit, list all their hashes as arguments for `git revert`.

   > Running `git revert` will create one or more new "Revert" commits that record the change in your git history.
   {.box .info}

1. Restart Cribl Stream.

## Support For Remote Repositories {#remote-setup}

Cribl supports Git remote repositories (but they are not required) for version control of all configuration changes.

> This feature requires certain license tiers. For details, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

You can configure a Leader Node with Git remote push capabilities through the [Cribl Stream CLI](cli-mode-master), or through the Cribl Stream user interface. From Settings, select the **Global** tab, then select **System** > **Git Settings**.

To create a repo, see these tutorials:

- [Setting Up a Repository](https://www.atlassian.com/git/tutorials/setting-up-a-repository) (CLI instructions, host-agnostic, from Atlassian).
- [Creating a New Repository](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/creating-a-new-repository) (specific to the GitHub Web UI).
- [Create a Repo](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github/create-a-repo) (longer GitHub-specific tutorial, also covers committing changes).

### Remote Formats Supported

Remote URI schema patterns should match this regex:
`(?:git|ssh|ftps?|file|https?|git@[-\w.]+):(\/\/)?(.*?)(\.git\/?)?$`

You can find a list of supported formats in this external article: [What are the supported git url formats?](https://stackoverflow.com/questions/31801271/what-are-the-supported-git-url-formats)

For example:

  * GitHub or other providers: `<protocol>://git@example.com/<username>/<reponame>.git`
  * Local Git servers: `git://<host.xyz>:<port>/<user>/path/to/repo.git`

### Secure Local Configuration and Remote Repos {#securing-remote-repos}

> Make a local backup of your whole `$CRIBL_HOME` directory before you set a remote repo. This safeguards your Cribl Stream configuration against being overwritten by a misconfigured remote.
>
> Create each new remote as an empty repo, with no README file and no commit history. (See the following GitHub and GitLab examples.)
>
> Some files that are used by Cribl Stream (both Leader and Worker Groups) contain sensitive keys; examples are `cribl.secret` and `...auth/ssh/git.key`. These will be pushed to the remote repo as part of the entire directory structure under version control. Ensure that this repo is secured appropriately.
>
{.box .danger}

### Connect to a Remote Repo over HTTPS (Recommended) {#connecting-to-remote-with-https}

Cribl recommends connecting to a remote repo over HTTPS. The following examples show a token-based and a password-based HTTPS connection to GitHub.

#### Example: Connect to GitHub over HTTPS with a Personal Access Token {#https-example}

1. [Create a new GitHub repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository). <br/>
  For best results, create a new empty repo, with no README file and no commit history. (This will prevent `git push` [errors](git-push-errors).) Note the username and email associated with your login on the repo provider.

1. [Create a personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) with **repo** scope.

1. Copy the token to your clipboard.

1. In Cribl Stream, from Settings, select the **Global** tab, then select **System** > **Git Settings**.

1. In the **Remote** tab, select **Authentication type**: **None**.

6. Fill in the **Remote URL** field with your username and repo name. Use the format below: <br/>
  `https://<username>:<token>@github.com/<owner>/<repository>.git`

For additional details, see the GitHub [Creating a Personal Access Token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) tutorial.

> For GitHub repos specifically, use only personal access tokens in the **Remote URL** field. GitHub [announced](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/) its end of support for plaintext passwords as of August 13, 2021.
>
{.box .warning}

#### Example: Connect to GitHub over HTTPS with Username and Password {#https-username-example}

1. [Create a new GitHub repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository). <br/>
  For best results, create a new **empty** repo, with no readme file and no commit history. (This will prevent `git push` [errors](git-push-errors).) Note the username and email associated with your login on the repo provider.

1. In Cribl Stream, from Settings, select the **Global** tab, then select **System** > **Git Settings**.

1. In the **Remote** tab, select **Authentication type**: **Basic**.

1. Enter your credentials in the **User** and **Password** fields.

### Connect to a Remote with SSH {#connecting-to-github-with-ssh}

You can set up SSH keys from the CLI, or upload keys via the UI. If you have a passphrase set, this functionality is available only through the CLI - see [Encryption: Configuring Keys with the CLI](securing-data-encryption#configuring-keys-with-cli). The example below outlines the UI steps.

#### Example: Connect to GitHub with SSH {#ssh-example}

1. [Create a new GitHub repository](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-new-repository). <br/>
  For best results, create a new **empty** repo, with no readme file and no commit history. (This will prevent `git push` [errors](git-push-errors).) Note the user name and email associated with your login to the repo provider.

2. [Add an SSH public key](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) to your GitHub account.

3. In Cribl Stream, from Settings, select the **Global** tab, then select **System** > **Git Settings**.

4. In the **Remote** tab, fill in the **Remote [repo] URL**. In the generic example below, replace `<username>` with your user name on the repo provider:

   * **Remote URL**: `<protocol>://[user@]server[:port][/path/to/repo.git]` (For example: `ssh://user@server/project/path.git`)

   * For GitHub specifically, the URL/protocol format must be: `ssh://user@server/project/path.git`

   * A specific (fictitious) GitHub example: `ssh://myuser@git.example.com/repos/myproject.git`


    > The Remote Host (or SSH Remote) field currently requires a full `SSH` URL, including the `ssh:// prefix`. While the `[user@]server:project.git` (Git-style) format is more common, it is not yet supported. Translate Git-style URLs to SSH-style: for example, `user@server:project/path.git` becomes `ssh://user@server/project/path.git`.
    {.box .warning}

5. Paste in the **SSH private key**.
   > The key will paste in with an appended newline below it. Do not delete this newline before you save the Remote Settings, or else you will trigger an error of the form: `fatal: Could not read from remote repository.`
   {.box .warning}

6. As the user running Cribl Stream, run this command to add the GitHub keys to `known_hosts`:

   `ssh-keyscan -H github.com >> ~/.ssh/known_hosts`

For additional details, see the GitHub [Connecting to GitHub with SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) tutorial.

![Cribl Stream Git settings](git-settings-annotated-4101.png)
{border="true"}

### GitLab Notes

For repos hosted on GitLab, we recommend that you:

- Create a new empty (blank) project, with no README file and no commit history. (Clear the **Initialize repository with a README** check box.)
  > This will prevent `git push` [errors](git-push-errors) by creating a project with no branch, not even `main`. When you later push the Cribl Stream `master` branch, that will create a corresponding `master` branch on the GitLab remote with no conflicts.
  {.box .success}

- Create a GitLab project access token for authentication. See the [GitLab documentation](https://docs.gitlab.com/ee/user/project/settings/project_access_tokens.html), which also covers project bot conventions.

- With project bots, the first token's username is set to `project_{project_id}_bot`. The password is the alphanumeric token.

- Create the token with `write_repository` scope.

- Specify a remote URL in HTTPS format. For example:
`https://localgitlab.<yourdomain.ext>/<yourusername>/cribl.git`

- If you do see a `main` branch in the Cribl Stream UI, do not select it. Leave the branch at `master`, then use the Stream UI's **Commit** and **Push** commands. This will create the desired `master` branch on GitLab’s remote.

The GitLab **Repository Settings > Push Rules** section includes these two settings of interest:

- As needed, enable **Check whether author is a GitLab user**.

- Understand the consequences of of enabling **Prevent committing secrets to Git**. This blocks commits of `.pem` and `.key` files. If you have [certificates](securing-tls-overview#env-vars) or [SSH keys](securing-data-encryption#encryption-keys) configured, this will break commits from Cribl Stream, throwing only a generic `API Error` in the UI. Check your `git` CLI client for more-specific diagnostics.

### Additional Git Settings {#git-settings}

From Settings, select the **Global** tab, then select **System** > **Git Settings**. Select the **General** tab to modify the following additional Git configurations, all of which are optional:

- **Branch**, **GitOps workflow**: These drop-downs are available to configure [GitOps](gitops).
  > See the [GitOps](gitops) documentation for details and precautions.
  {.box .warning}
- **Collapse actions**: Combine multiple Git buttons to one, to reduce repetitive clicks. See [Collapse Actions](#collapse-actions) below.
- **Default commit message**: Enter a placeholder message to apply to all commits from Cribl Stream. This also reduces clicks.
- **Git timeout**: Maximum time (in milliseconds) to wait for Git processes to complete before killing them. If a Leader instance hangs, try lowering this value from the default `300000` (300 seconds = 5 minutes). However, avoid very low values (like `1000` ms), which might time out even when Git processes are working properly. Enter `0` to wait indefinitely.

![Git Authentication Type settings](git-settings-general-4101.png)
{border="true"}

On the **Git Settings** > **Remote** tab, you can change the **Authentication Type** from its **SSH** default to **Basic** authentication. This displays two additional fields:
- **User**: Username on the repo.
- **Password**: Authentication password (for example, a GitHub personal access token).

![Git Authentication Type settings](git-settings-remote-4101.png)
{border="true"}

> GitHub (specifically) does not support Basic authentication.
>
{.box .warning}

On the **Git Settings** > **Scheduled Actions** tab, you can schedule a **Commit**, **Push**, or **Commit & Push** action to occur on a predefined interval.

![Git Scheduled Actions selection](git-scheduled-actions.4a602cdad7.png)
{border="true"}

For the selected action type, you can define a [cron schedule](https://en.wikipedia.org/wiki/Cron), and a commit message distinct from the **General** tab's **Default commit message**. Then select **Save**.

![Saving a Git Scheduled Action](git-cron-schedule.dbb849e91c.png)
{border="true"}

You can schedule only one type of action. To swap to a different type, select it from the **Scheduled global actions** drop-down, and resave. To turn off scheduled Git commands, select **None** from the drop-down, and resave.

On the **Git Settings** > **Copilot** tab, you can toggle the **Generate commit messages automatically with Cribl Copilot** option on to have Cribl Copilot auto-generate every commit message for you. Cribl Copilot-generated messages override any default commit message you may have set.

![Enabling Copilot-generated commit messages](commit-copilot.png)
{border="true"}

## Push Configs to a Remote Repo  {#pushing-to-a-remote-repo}

Once you've configured a remote, a **Git Push** button appears in the **Global Config** dialog. You can use this to copy committed configuration changes to your remote.

![Git Push button](remote-repo-push-button.f75f94819a.png)
{scale="60%" border="true"}

> The branch indicator will normally read `master`, as shown above, unless you have enabled [GitOps](gitops) with an appropriate license.
>
{.box .info}

### Collapse Actions {#collapse-actions}

If you enabled the **Git Settings** > **Collapse Actions** option, you will be offered a combined **Commit & Push** button in the **Global Config** dialog.

![Git combined actions button](remote-repo-commitpush-button.780172e9d0.png)
{scale="50%" border="true"}

At the Group level, enabling **Collapse Actions** adds a combined **Commit & Deploy** option to the modal you see after clicking **Commit** for the Group config.

> Enabling **Collapse Actions** with a remote repo simplifies the **Git Changes** confirmation dialog. It substitutes just a commit **Message** text box, with **OK** and **Cancel** buttons - omitting the diff view. Don't enable this option if you prefer to inspect configuration changes before committing.
>
{.box .success}

### Back Up Configs Out of Band  {#manual}

Once you've configured your remote repo, changes to all Cribl Stream [config files](configuration-files) under the `$CRIBL_HOME` directory will normally be backed up to your remote whenever you select the above **Git Push** or **Commit & Push** UI buttons. The exceptions are:

- Any configuration files you've chosen to store outside `$CRIBL_HOME`.
- Any configuration files residing in paths you've [added to](#gitignore) `.gitignore`.

To back up these files to your remote, you'll need to either:

- Change the above paths/settings, or
- Use an external `git` client to manually push them.

### Troubleshooting Push Errors {#troubleshooting}

To resolve errors commonly encountered when pushing to a remote repo, see:

- [Git Push Errors](git-push-errors)
- [Git Remote Repos & Trusted CAs](git-certificate-errors)

> ##### {{< id `troubleshooting` >}} Troubleshooting Resources
>
> [Cribl University](https://cribl.io/university/) offers a Troubleshooting Criblet on [Commit & Deploy](https://university.cribl.io/#/online-courses/b7db4738-a193-4bc2-8e71-cdaf67ad60d1) best practices and error recovery. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You'll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses - but Cribl's training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
>
{.box .info}



## Restore Leader from a Remote Repo {#restoring-master-from-remote-repo}

If a remote repo is configured and has the latest known good Leader configuration, this section outlines the general steps to restore the config from that repo.

![Restoring from remote repo](cribl-master-remote-repo-restore.aefd4a1eac.png)
{border="true"}

Let's assume that either the entire `$CRIBL_HOME` directory of the Leader is corrupted, or you're replicating the remote repo's config onto a fresh instance. Let's also assume that the remote repo has the form:
`git@github.com:<username>/<reponame>.git`.

1. **Important**: In a directory of choice, untar the **same Cribl Stream version** that you're trying to restore, but do not start it.

  > Make sure you download the **matching** Cribl Stream version. The Cribl [Download page](https://cribl.io/download/) foregrounds current version of Cribl Stream, but provides a link to [this archive](https://cribl.io/download/past-releases/) of prior releases.
  {.box .warning}

2. Change directory into `$CRIBL_HOME` and initialize `git`:

  ```shell
  # git init
  ```

3.  If you are using SSH key authentication, specify the key using the following command:

  ```shell
  GIT_SSH_COMMAND='ssh -i </path/to/github/repo>.key -o IdentitiesOnly=yes'
  ```

  > As an alternative to executing `GIT_SSH_COMMAND` on the fly, you can set your key in `$CRIBL_HOME/.git/config` with:
  > `git config core.sshCommand "ssh -i /path/to/key"`

  > Or set it globally with:
  > `git config --global core.sshCommand "ssh -i /path/to/key"`
  {.box .success}

4. Ensure that you have proper access to the remote repo:

  ```shell
  # git ls-remote git@github.com:<username>/<reponame>.git
  56331fabb4822eaec4ca0ffd008d6e9974c1e419f	HEAD
  5631fabb4822eaec4ca0ffd008d6e9974c1e419f	refs/heads/master
  ```

5. Next, add/configure the remote:

  ```shell
  # git remote add origin git@github.com:<username>/<reponame>.git
  ```

6. Now set up your local branch to exactly match the remote branch:

  ```shell
  # git fetch origin
  # git reset --hard origin/master
  ```

7. Finally, to confirm that the commits match, run this command while in `$CRIBL_HOME`:

  ```shell
  # git show --abbrev-commit
  ```

  You should see output indicating that `HEAD` points to both `master` and `origin/master`, as in this example:

  ```shell
  commit 5631fab (HEAD -> master, origin/master)
  Author: First Last <email@example.com>
  Date:   Fri Jan 31 10:16:07 2020 -0500

      admin: Last commit before failure/crash
  ......
  ```

Step 6 above pulls in all the latest configs from the remote repo. Step 7 confirms the local repo matches the remote. You should now be able to start the Leader as normal. Once up and running, Workers should start checking in after about 60 seconds.

> ##### Verify cribl.secret
>
> The `cribl.secret` file - located at `$CRIBL_HOME/local/cribl/auth/cribl.secret` - contains the secret key that is used to encrypt sensitive settings on configuration files (for example, AWS Secret Access Key). Make sure this file is properly restored on the new Leader, because it is required to make encrypted conf file settings usable again.
>
{.box .warning}

## .gitignore File {#gitignore}

A `.gitignore` file specifies files that `git` should ignore when tracking changes. Each line specifies a pattern, which should match a file path to be ignored. Cribl Stream ships with a `.gitignore` file containing a number of patterns/rules, under a section of the file labeled `CRIBL SECTION`.

``` {title=".gitignore"}
# Do NOT REMOVE CRIBL and CUSTOM header lines!
# DO NOT REMOVE rules under the CRIBL section as they may be reintroduced on update.
# You can ONLY comment out rules in the CRIBL section.
# You can add new rules in the CUSTOM section.
### CRIBL SECTION -- DO NOT REMOVE ###
default/ui
default/data/ui
/bin/
/lib/
/log/
/pid/
data/uploads
/diag/
**.tmp
/state/
**/local/cribl/messages.yml
**/local/edge/messages.yml
**/default/cribl/lakes.yml
**/local/cribl/lakes.yml
/thirdparty/
*.cpuprofile
**/auth/ssh/git.key
ha-version.txt
### CUSTOM SECTION -- DO NOT REMOVE ###

<User-defined patterns/rules go here>
```

### Worker Deployment: How to Handle Customer-Defined `.gitignore` Exclusions {#bundler}

The `bundler` object in `cribl.yml` allows you to deploy files to GitOps that are usually excluded via `.gitignore`, like `cribl.secret` and `secrets.yml`. This facilitates loading custom secrets per environment and Group.

> Files listed in the `bundler` object are deployed, but they are never committed to GitOps. You must implement your own logic (outside GitOps) to place the correct secret files for each environment and Group prior to deployment to ensure that the correct secrets are present. This approach safeguards sensitive information while leveraging the `bundler` functionality within your GitOps workflow.
>
{.box .warning}

To leverage this functionality for deployment-critical files, configure the Leader's `cribl.yml` file located at `$CRIBL_HOME/local/cribl/cribl.yml` using the `bundler.bundleGitIgnoredPatterns` setting. This ensures essential files reach Workers, even if they are not tracked with version control due to customer-specific `.gitignore` rules. It is not necessary to restart the Leader after editing the Leader's `cribl.yml` file.

##### Example: Use `.gitignore` with `bundler`

To exclude all [Cribl secrets](authentication#the-criblsecret-file) from Git, in `.gitignore`, enter `local/cribl/auth/cribl.secret` to exclude the Leader's `cribl.secret` file as well as the glob pattern `**/local/cribl/auth/cribl.secret` to exclude all `cribl.secret` files located in the `local/cribl/auth` subdirectory of any Group within the `groups` directory, regardless of the `groupName`.

```
# Exclude Leader's cribl.secret
local/cribl/auth/cribl.secret
    
# Exclude cribl.secret for all Worker Groups
**/local/cribl/auth/cribl.secret
```

To exclude only a specific group's `cribl.secret` file, use `groups/{groupName}/local/cribl/auth/cribl.secret` to `.gitignore` instead of `**/local/cribl/auth/cribl.secret`.

In `bundler.bundleGitIgnoredPatterns`, add `local/cribl/auth/cribl.secret` to ensure that the `cribl.secret` file is included in the config bundle during deployment without being included in version control. Patterns in `bundler.bundleGitIgnoredPatterns` are evaluated with a base directory of `$CRIBL_HOME/groups/<group>`. The bundler does not require the Leader's `cribl.secret`.

If you want to avoid tracking custom certificates as well, use the path for the directory in which the secrets/certificates reside: `local/cribl/auth`.

```
bundler:
  bundleGitIgnoredPatterns:
    - 'local/cribl/auth'
```

#### CRIBL Section

> ##### Do Not Remove CRIBL SECTION or CUSTOM SECTION Headers
>
> Under the `CRIBL SECTION` header, Cribl Stream specifies the files that Git will ignore by default. Further files for Git to ignore should be listed under the `CUSTOM SECTION` header.
>
> Do **not** add any lines to the `CRIBL SECTION` block, because the next update will just overwrite them anyway. Do not remove any lines either. The only modifications that will survive updates are commented lines.
>
> Do not remove any section header lines, or unexpected behavior might occur on update.
>
{.box .warning}

#### CUSTOM Section

User-defined, custom patterns/rules can be **safely defined** under the `CUSTOM SECTION`. Cribl Stream will **not** modify the contents of `CUSTOM SECTION`.

Good candidates to add here include large lookup files - especially large binary database files. For details, see [Git Push Errors: Large Files Detected](git-push-errors#largefile).

#### Files Skipped with .gitignore

If you have files that you've set `.gitignore` to skip, you will need to back them up and restore them by means other than Git. For example, you can periodically copy/rsync them to a backup destination, and then restore them to their original locations after you complete the steps above.

Files specified in `.gitignore` are not only excluded from pushes to the remote repo, but are also excluded from Worker Group config bundles. When Workers load a new config that references a skipped (and missing) file, this can produce unexpected results, and usually errors.

For example, if you add `**/auth/certs/leader*` to `.gitignore`, this will prevent the `leader.crt` and `leader.key` files from leaking to Worker Nodes in config deployments.

Configs that you `.gitignore` in this way on the Leader will not persist on Workers after they receive their next deploy. However, these configs will be available in the next few rolling backup bundles in `state/`.
