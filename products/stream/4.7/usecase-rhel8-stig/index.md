# RHEL 8 Security Technical Implementation Guide


This page explains the necessary configurations to run Cribl Stream on a system compliant with the Red Hat Enterprise Linux (RHEL) 8 Department of Defence (DOD) Security Technical Implementation Guides (STIGs).

Operating any system on a network, especially within the DOD, requires obtaining an Authority to Operate (ATO), involving compliance with Security Technical Implementation Guides (STIGs). These guides cover various levels, including the operating system, database, and application. For Cribl Stream, the most relevant STIGs are those governing the operating system deployment. This ensures a hardened environment with configured tools to safeguard it. However, out of the 500+ STIGs, certain actions may be blocked by default, requiring explicit configuration to permit Cribl Stream execution.

## SELinux

To ensure compliance with RHEL 8 requirements, SELinux targeted policy must be enabled (STIG reference: [V-230282](https://www.stigviewer.com/stig/red_hat_enterprise_linux_8/2021-12-03/finding/V-230282)).

When running Stream on a system with SELinux, it's crucial to download the tgz file directly to the `/opt/` directory. Additionally, ensure that the .tgz file is uncompressed within the `/opt/` directory. Moving files on a system with SELinux may lead to complications.

For comprehensive information on SELinux configuration, refer to our [SELinux Configuration](usecase-selinux) document.

## firewalld

[firewalld](https://firewalld.org/) is a dynamic firewall management tool that offers support for network/firewall zones, allowing you to define the trust level of network connections or interfaces. It encompasses features for managing IPv4 and IPv6 firewall settings, Ethernet bridges, and IP sets. According to the DOD STIG, firewalld should enforce a deny-all rule and selectively open ports.

For compliance with RHEL 8 standards, the firewall must adhere to a deny-all, allow-by-exception approach for permitting connections to external systems (STIG reference: [V-230504](https://www.stigviewer.com/stig/red_hat_enterprise_linux_8/2021-03-04/finding/V-230504)).

In a Cribl Stream environment, the Leader Node requires ports `4200` and `9000` over TCP to be open. Additionally, Worker Nodes should have port `9000` over TCP open if Worker UI access is enabled, along with any necessary Source ports.

### Configuring firewalld for Use with Cribl Stream

These steps assume you already have firewalld enabled and that ports are already configured to allow communication traffic (such as SSH). The following steps show you how to configure an existing firewalld to allow ports necessary for Cribl Stream to operate. 

1. Determine the [zone](https://firewalld.org/documentation/zone/) that is currently being used by running the command: 
   ```
   $ firewall-cmd --get-active-zones
   ```
   
   Example response:
   ```
   mysupersecurezone
    interfaces: eth0
    ```
    
    In the above response, the zone that is in use is `mysupersecurezone`. You may have multiple NICs with multiple zones. In this case, you will need to determine the correct NIC/zone that Cribl Stream will use.
    
1. Add the required ports.
   1. On the Leader, add the ports with the following configuration:
      ```
      sudo firewall-cmd --zone=<ZONE> --permanent --add-port=4200/tcp
      sudo firewall-cmd --zone=<ZONE> --permanent --add-port=9000/tcp
      ```
   1. Once this is complete, you can verify the status by running the command: 
      ```
      sudo firewall-cmd --zone=<ZONE> --permanent --list-ports
      ```
   1. For the configuration to take effect, you need to reload the firewalld service by running the command:
      ```
      $ sudo firewall-cmd --reload
      ```
   1. On Workers, add the necessary ports based on your Source configurations.
      ```
      sudo firewall-cmd --zone=<ZONE> --permanent --add-port=9000/tcp
      sudo firewall-cmd --zone=<ZONE> --permanent --add-port=<CUSTOM_PORT>/tcp
      ```
   1. Once this is complete, you can verify the status by running the command: 
      ```
      sudo firewall-cmd --zone=<ZONE> --permanent --list-ports
      ```
   1. For the configuration to take effect, you need to reload the firewalld service by running the command: 
      ```
      $ sudo firewall-cmd --reload
      ```

## fapolicy

[fapolicy](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/assembly_blocking-and-allowing-applications-using-fapolicyd_security-hardening) is a software framework that regulates application execution through a user-defined policy, adhering to an allow-list approach. According to the [DISA STIG for RHEL](https://www.stigviewer.com/stig/red_hat_enterprise_linux_8/2020-11-25/finding/V-230523), you should explicitly permit or trust applications to execute within a deny-all catch-all policy. This framework serves as an efficient method to prevent the execution of untrusted and potentially malicious applications on the system.

### Configuring Stream with fapolicy {#config-fapolicy}

Since Cribl binaries are not explicitly trusted by default, you need to add them to the trust store to ensure that fapolicy will allow them to execute. The following commands assume that `$CRIBL_HOME` is `/opt/cribl` - modify this to match your `$CRIBL_HOME` location.

1. Add the Cribl binaries to be trusted by fapolicy by running the command: 
   ```
   sudo fapolicyd-cli --file add /opt/cribl/bin/ --trust-file cribl
   ```
1. When using Cribl Stream 4.5 and higher with a Destination utilizing Binary Protobuf payload encoding, ensure to include the protobuf-libraries in the trust file.
   ```
   fapolicyd-cli --file add /opt/cribl/data/protobuf-libraries/ --trust-file cribl
   ```
1. Add the git directory to fapolicy. This is required for the initial `git init` operation.
   ```
   sudo fapolicyd-cli --file add /usr/share/git-core/ --trust-file cribl
   ```
   > After executing these commands, a trust file with the path and filename `/etc/fapolicyd/trust.d/cribl` will be created. It will contain the path, the size, and the SHA256 hash for each trusted file. An example of a trust file is provided below under [Additional Resources](#additional-resources).
   >
   {.box .info}

1. Update the fapolicy trust store after applying the above configurations. This has to be done any time that a trusted file changes, when updating git, or installing Stream updates.
   ```
   sudo fapolicyd-cli --update
   ```
### Upgrading Stream with fapolicy
 
UI-based upgrades will fail when running fapolicy since fapolicy tracks individual binaries and their associated SHA256 hashes. The error received will be similar to:

   `Upgrade failed: EPERM: operation not permitted, open '/opt/cribl/unpack.zlXiIxD.tmp/cribl`

#### Upgrade the Leader Node

To run an upgrade on the Leader Node when using fapolicy, follow the below steps. See our [upgrading docs](upgrading#upgrading-the-leader-via-the-ui) for more details.

1. Commit and deploy your desired last version. (This will be your most recent checkpoint.)
1. Optionally, `git push` to your configured remote repo.
1. [Stop](cli-reference#stop) Cribl Stream.
   1. Optional but recommended: Back up the entire `$CRIBL_HOME` directory.
   1. Optional: Check that the Worker Nodes are still functioning as expected. In the absence of the Leader Node, they should continue to work with their last deployed configurations.
1. Uncompress the new Cribl Stream version on top of the old one. Make sure that the proper permissions are applied after the new version is uncompressed.
1. Run the command `sudo fapolicyd-cli --update` to update the fapolicy trust file.
1. [Start](cli-reference#start) Cribl Stream and log back in.
1. Wait for all the Worker Nodes to report to the Leader, and ensure that they are correctly reporting the last committed configuration version.

#### Upgrade the Worker Nodes

1. [Stop](cli-reference#stop) Cribl Stream on each Worker Node.
1. Uncompress the new version on top of the old one. Ensure that the proper permissions are applied after the new version is uncompressed.
1. Run the command `sudo fapolicyd-cli --update` to update the fapolicy trust file.
1. [Start](cli-reference#start) Cribl Stream.

## Troubleshooting with `--debug-deny`

When running into errors when fapolicy is enabled, you can enable fapolicy in debug mode to help diagnose the file that is causing the issue.

It's important to ensure that fapolicy is disabled before stopping its service. If fapolicy is running and the following command is executed, the box will hang. Stop the service with the command: `sudo systemctl stop fapolicyd`.

To start fapolicy in debug mode run the following command. This will send the output of the debug to the file `fapolicy.output`. Using the `--debug-deny` flag, you are only going to capture deny events.

```
sudo fapolicyd --debug-deny 2> fapolicy.output &
```

To stop the fapolicyd debug command:

```
kill <pid>
```

The following errors indicate that the Cribl binaries are not trusted. Follow the steps in [Configuring Stream with fapolicyd](#config-fapolicy) to designate the Cribl Stream binaries as trusted.

```
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1820 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1821 exe=/opt/cribl/bin/cribl : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1822 exe=/opt/cribl/bin/cribl : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1824 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1827 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1829 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1831 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1833 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1835 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1837 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1839 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
rule=13 dec=deny\_audit perm=execute auid=-1 pid=1841 exe=/usr/lib/systemd/systemd : path=/opt/cribl/bin/cribl ftype=application/x-executable trust=0
```

The following error will occur if the `git-core` directory is not trusted.

```
rule=11 dec=deny\_audit perm=open auid=-1 pid=1635 exe=/usr/bin/git : path=/usr/share/git-core/templates/hooks/fsmonitor-watchman.sample ftype=text/x-perl trust=0
```

## Common Errors

### fapolicy

   ```bash
   \[cribl@rhel\_leader ec2-user\]$ /opt/cribl/bin/cribl start
   bash: /opt/cribl/bin/cribl: Operation not permitted
   ```

   ```bash
   \[ec2-user@rhel\_leader ~\]$ sudo /opt/cribl/bin/cribl boot-start enable -u cribl
   internal/modules/cjs/loader.js:1144
     return process.dlopen(module, path.toNamespacedPath(filename));
                    ^

   Error: /opt/cribl/bin/kerberos.node: cannot open shared object file: Operation not permitted
       at Object.Module.\_extensions..node (internal/modules/cjs/loader.js:1144:18)
       at Module.load (internal/modules/cjs/loader.js:950:32)
       at Function.Module.\_load (internal/modules/cjs/loader.js:790:12)
       at Module.require (internal/modules/cjs/loader.js:974:19)
       at require (internal/modules/cjs/helpers.js:93:18)
       at Object../src/sluice/js/input/kafka/kerberos-bindings/kerberos.ts (/opt/cribl/bin/cribl.js:14:12107088)
       at r (/opt/cribl/bin/cribl.js:14:107)
       at Object../src/sluice/js/input/kafka/kerberos-authenticator/authenticator.ts (/opt/cribl/bin/cribl.js:14:12104474)
       at r (/opt/cribl/bin/cribl.js:14:107)
       at Object../src/sluice/js/input/util.ts (/opt/cribl/bin/cribl.js:14:12133931) {
     code: 'ERR\_DLOPEN\_FAILED'
   }
   ```

   Both of the above errors are indicate that the Cribl `bin` directory is not trusted. Run the following two commands to ensure that the binaries are trusted.

   ```
   sudo fapolicyd-cli --file add /opt/cribl/bin/ --trust-file cribl
   sudo fapolicyd-cli --update
   ```

### Git

If you receive any of these errors related to git, this is because git was not trusted. Follow the steps in [Configuring Stream with fapolicy](#config-fapolicy) to add the git directory to the fapolicy.

**Git not found**
```json
{"time":"2022-10-09T19:41:05.772Z","cid":"api","channel":"gitmgr","level":"warn","message":"Git not found. Config versioning will not be available","error":{"message":"fatal: --local can only be used inside a git repository\\n","stack":"Error: fatal: --local can only be used inside a git repository\\n\\n    at Object.action (/opt/cribl/bin/cribl.js:14:8731127)\\n    at Ji.exec (/opt/cribl/bin/cribl.js:14:8731453)\\n    at /opt/cribl/bin/cribl.js:14:8696667\\n    at new Promise (<anonymous>)\\n    at sr.handleTaskData (/opt/cribl/bin/cribl.js:14:8696554)\\n    at sr.<anonymous> (/opt/cribl/bin/cribl.js:14:8696165)\\n    at Generator.next (<anonymous>)\\n    at i (/opt/cribl/bin/cribl.js:14:8680029)\\n    at processTicksAndRejections (internal/process/task\_queues.js:95:5)"}}
```

```
fatal: not a git repository (or any of the parent directories): .git
```

![git error](git-no-repo.png)
{scale="100%" border="true"}

## Additional Resources

This is an example of a trust file.

```bash
\[ec2-user@rhel\_leader ~\]$ sudo cat /etc/fapolicyd/trust.d/cribl
# AUTOGENERATED FILE VERSION 2
# This file contains a list of trusted files
#
#  FULL PATH        SIZE                             SHA256
# /home/user/my-ls 157984 61a9960bf7d255a85811f4afcac51067b8f2e4c75e21cf4f2af95319d4ed1b87
/opt/cribl/bin/cribl.init.d.template 1246 30630f30db8d6fd429e534d349a3c5ea1178eb95dad7ac12729d124ace55e5c1
/opt/cribl/bin/cribl.service.template 439 864594d5a0807edcfbb1593383c760ebfb742791516e302a8f53fa3c245cafc3
/opt/cribl/bin/cribl.sh 946 de156435dbd4c566b316e70d91b08e8eb404e22468c6fdddb69dc597668075ca
/opt/cribl/bin/install-worker.sh.template 4472 aa428d738926b2aba33066d9211f4285119e9a8ccf6a81df2418703293c71c2d
/opt/cribl/bin/start.sh 373 0b22244e96b4ae9472f41a9d1cd4bc4a98cd406aabdd3b0fdddff33e9f0036bc
/opt/cribl/bin/cribl 80968808 46ac829fb9b7cd7e02e8f564d9766d15e79a2792414b4c4e31a6938d365a9a68
/opt/cribl/bin/criblnative.node 606496 84375185ade3ff64fed0bc9bcf1e03cb15d77bf9362598964e59d713a96ec4ea
/opt/cribl/bin/kerberos.node 2078160 8be303b3780b1db0417929faaea6eae9844ba54f145365e3952e9fced32ebe27
/opt/cribl/bin/parquet.node 36158248 c20e9ad5c5fd940d34665c757eb133b8ebeeed3bb5f1c65b459b928f1dcb10a7
/opt/cribl/bin/scope 30358824 af3e44ef6184f41cd4f93bb96fd26726f81d470b7d7b7c41cc3e27def8fd5cd6
/usr/share/git-core/contrib/completion/git-completion.tcsh 4801 9142f7232f10c85c5c95866835bdc6368896d9eddc68433a39593b9f1d850293
/usr/share/git-core/contrib/completion/git-prompt.sh 17778 54ff8c23867cf3921bfc0220b7953b08f7b16f90f07d7bd472cc28daa2ade91e
/usr/share/git-core/contrib/hooks/post-receive-email 22069 2a8091438f32611d5e7925f5d0cded27b21e6e9c14a207563ad92775cfcc4bec
/usr/share/git-core/contrib/hooks/pre-auto-gc-battery 1265 3953dbbd7b876bf16c2cdc2c1657e389910fe3507635cebb91c0c665f0732c08
/usr/share/git-core/contrib/hooks/multimail/CHANGES 9950 fa125b3a43d2c1b7c7078cac315e03ca7343979a19133de44395b63ae2cacfbf
/usr/share/git-core/contrib/hooks/multimail/CONTRIBUTING.rst 2532 b336d962a0fc5fa2dfdef8304b988fe3a9ae7c2f1ecca19e5246862f714907c5
/usr/share/git-core/contrib/hooks/multimail/README.Git 589 741ae7a64784b42070c71739a064458f930ba518c35c07f260f58e756f02290a
/usr/share/git-core/contrib/hooks/multimail/README.migrate-from-post-receive-email 6096 0037ec76e20c21d27860510cab0fcbf4390dc7ad82f10cdc2b128028d939b49e
/usr/share/git-core/contrib/hooks/multimail/README.rst 34571 f6b733ee43f3718ccce3a495e5f3200d1b8bb054c5f1d78bd08559763a615b77
/usr/share/git-core/contrib/hooks/multimail/doc/customizing-emails.rst 2215 99f62da073fe2a9d1964594a36d909d0400af38b5d0ad0b01b6c7d34321998c5
/usr/share/git-core/contrib/hooks/multimail/doc/gerrit.rst 1987 4193bd5142d5cbbbc7c4c83e8b92636904e1facb516617e28f3978ce7a747908
/usr/share/git-core/contrib/hooks/multimail/doc/gitolite.rst 4436 92dcc7a23ca3407c096d483edae4d6d71d764110bcffaa81ae17be53d246a951
/usr/share/git-core/contrib/hooks/multimail/doc/troubleshooting.rst 3255 3419f3db57b5f749083b88c4f930d2c67af4f98f6011e5e3726b2a8392a5a8b9
/usr/share/git-core/contrib/hooks/multimail/git\_multimail 160742 60ce0fd1b0423787fb554dcf18bb90b6815d9cff656ddbef05c1977ed35a3e95
/usr/share/git-core/contrib/hooks/multimail/migrate-mailhook-config 8011 d2b7a882576d7ebc717fe4dbbd9adf00fbfee574c854c17753b506c4a4772aaf
/usr/share/git-core/contrib/hooks/multimail/post-receive.example 3577 7d7fe98bd455bd0d04602811894a4d8cd1dcdf61b7af10f70b2738ddc203ecb6
/usr/share/git-core/contrib/hooks/setgitperms.perl 6920 4e1076233ebfbdee41c86521a5e5807072515b3e944136f29d00e67d13eb925b
/usr/share/git-core/contrib/hooks/update-paranoid 11648 6f74d3ffdf0f7359844d6accbb2cc713282bcd4542c2c29f959213644ea4a21d
/usr/share/git-core/contrib/diff-highlight 7281 7cef44fbf328c7167476df4d2d4906122d412efefab5c9d8d129c5f94b0ff864
/usr/share/git-core/templates/description 73 85ab6c163d43a17ea9cf7788308bca1466f1b0a8d1cc92e26e9bf63da4062aee
/usr/share/git-core/templates/hooks/applypatch-msg.sample 478 0223497a0b8b033aa58a3a521b8629869386cf7ab0e2f101963d328aa62193f7
/usr/share/git-core/templates/hooks/commit-msg.sample 896 1f74d5e9292979b573ebd59741d46cb93ff391acdd083d340b94370753d92437
/usr/share/git-core/templates/hooks/post-update.sample 189 81765af2daef323061dcbc5e61fc16481cb74b3bac9ad8a174b186523586f6c5
/usr/share/git-core/templates/hooks/pre-applypatch.sample 424 e15c5b469ea3e0a695bea6f2c82bcf8e62821074939ddd85b77e0007ff165475
/usr/share/git-core/templates/hooks/pre-commit.sample 1643 f9af7d95eb1231ecf2eba9770fedfa8d4797a12b02d7240e98d568201251244a
/usr/share/git-core/templates/hooks/pre-merge-commit.sample 416 d3825a70337940ebbd0a5c072984e13245920cdf8898bd225c8d27a6dfc9cb53
/usr/share/git-core/templates/hooks/pre-push.sample 1374 ecce9c7e04d3f5dd9d8ada81753dd1d549a9634b26770042b58dda00217d086a
/usr/share/git-core/templates/hooks/pre-receive.sample 544 a4c3d2b9c7bb3fd8d1441c31bd4ee71a595d66b44fcf49ddb310252320169989
/usr/share/git-core/templates/hooks/push-to-checkout.sample 2783 a53d0741798b287c6dd7afa64aee473f305e65d3f49463bb9d7408ec3b12bf5f
/usr/share/git-core/templates/hooks/update.sample 3650 8d5f2fa83e103cf08b57eaa67521df9194f45cbdbcb37da52ad586097a14d106
/usr/share/git-core/templates/hooks/fsmonitor-watchman.sample 4655 f3c0228d8e827f1c5260ac59fdd92c3d425c46e54711ef713c5a54ae0a4db2b4
/usr/share/git-core/templates/hooks/pre-rebase.sample 4898 4febce867790052338076f4e66cc47efb14879d18097d1d61c8261859eaaa7b3
/usr/share/git-core/templates/hooks/prepare-commit-msg.sample 1492 e9ddcaa4189fddd25ed97fc8c789eca7b6ca16390b2392ae3276f0c8e1aa4619
/usr/share/git-core/templates/info/exclude 240 6671fe83b7a07c8932ee89164d1f2793b2318058eb8b98dc5c06ee0a5a3b0ec1
```