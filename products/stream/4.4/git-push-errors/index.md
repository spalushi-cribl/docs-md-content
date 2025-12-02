# Git Push Errors


This page anticipates common errors you might see in Cribl Stream's UI, or in the `git` CLI, when [pushing a commit](version-control#pushing-to-a-remote-repo).

## Failed to Push Some Refs

Your first push to a remote repo might fail with one of several `failed to push some refs` errors. 

As a first step in debugging these errors, edit the `$CRIBL_HOME/.git/config` file to make sure that its `name` and `email` key values match the credentials you've set on your repo provider or git server. 

Also make sure that the `remote "origin"` key value matches the remote you set when you [connected to the remote repo](version-control#remote-setup). This example shows all three keys, with placeholder values:

```shell
[user]
	name = <your-login-name>
	email = <email@example.com>
[remote "origin"]
	url = https://<user-name>:<token>@github.com/<username>/<repo-name>
```

Next, verify the remote repo from the command line, as follows:

```shell
cd $CRIBL_HOME/.git
git remote -v 
```

In response, `git` should echo your configured remote twice – once for `fetch` and once for `push` operations.

If all of the above settings are correct, the `push` is very likely blocking because the remote repo has some commit history, or was simply created with a `readme.md` file. For command-line instructions to remedy this – by syncing your local repo to its remote – see GitHub's [Dealing with Non-Fast-Forward Errors](https://docs.github.com/en/github/using-git/dealing-with-non-fast-forward-errors) topic.

## Large Files Detected {#largefile}

A push command might also trigger "large file" warnings or, more seriously, errors of this form (CLI/GitHub example):

```shell
remote: warning: File data/lookups/geo.mmdb is 60.12 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: [################################################################]
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File groups/default/data/lookups/largelookup.csv is 313.91 MB; this exceeds GitHub's file size limit of 100.00 MB
```

Cribl recommends adding such large files to [`.gitignore`](version-control#gitignore), to exclude them from subsequent `push` commands. As the above examples show, typical culprits are large `.csv` or `.mmdb` lookup files. A simple option is to place these files in a `$CRIBL_HOME` subdirectory that's already listed in `.gitignore` – for details, see [Managing Large Lookups](large-lookups).

Other available workarounds include staging such files **outside** `$CRIBL_HOME`, or using plugins to accommodate the large files. For GitHub-specific options, see [Working with Large Files](http://git.io/iEPt8g).

## See Also

- [Git Remote Repos with Trusted CAs](git-certificate-errors)
