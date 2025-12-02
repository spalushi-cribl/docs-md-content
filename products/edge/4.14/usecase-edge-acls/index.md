# Using ACLs to Allow Cribl Edge to Read Files


Running Cribl Edge as an unprivileged user is a best practice. However, without modifying the default Linux permissions, you will run into issues in accessing files owned by other users.

![Default Linux permissions](ed-linux-permissions.b2f0bcc9e4.png)
{border="false"}

Linux systems allow you to layer an Access Control List (ACL) on top of the default Linux permission set. With ACLs, you can apply a more specific set of permissions to a file or directory without (necessarily) changing the base ownership and permissions. For details, see [Introduction to ACLs](https://www.redhat.com/sysadmin/linux-access-control-lists).

As an example, you might want to read data from the `/var/log` directory. This directory is typically owned by the `root` user, with a permission set of `750` on the directory. This means the `cribl` user will not be able to read or list the files in the directory, because the `Other` group has zero permissions.

To achieve compliance with benchmarks such as CIS or NIST, we can use the ACLs to grant the `cribl` group access to this folder and any files, without disturbing the current permissions.

> ##### CIS Benchmark 4.2.3
>
> Make sure that permissions are configured on all log files. Log files must have the correct permissions to ensure that sensitive data is archived and protected. `Other/world` should not have the ability to view this information. `Group` should not have the ability to modify this information.
>
{.box .info}

To accomplish this, we can grant the `cribl` group `read` and `execute` access to the files and directories inside `/var/log`, by running this command: 

```shell
setfacl -Rm g:cribl:r-X /var/log
```

Breaking down the command's options':
 - `-R` : Recursive
 - `-m` : Modify
 - `g:cribl` = `cribl` group, this could be `u:cribl` if you wanted to limit to the `cribl` user.
 - `r-X`: Read and execute. Capital `X` means execute only on directories.

This modifies only the current files in the directory, if you want the appropriate ACL applied to any future files created here, add the `-d` flag (for default):

```shell
setfacl -Rdm g:cribl:r-X /var/log
```

Now, any rotated or created files will apply the ACL set.

## Checking the ACLs

To verify the ACLs on a file or directory, run the following command:

```shell
getfacl <file or folder>
```
This will output a listing of the applied ACLs, including the directory's defaults:

```shell
$ getfacl /<directory>
# file: <file>
# owner: <owner>
# group: <group>
user::rwx
group::rwx
other::---
default:user::rwx
default:user:<user>:rwx
default:group::rwx
default:mask::rwx
default:other::---
```
## Installing ACL Utilities 

The ACL utilities might not be installed, by default, on the OS. For example, on Ubuntu (Debian-based) systems, you will need to install the `acl` package. For Debian-based tools using [apt]( https://en.wikipedia.org/wiki/APT_(software)):

```shell
apt install acl
```

For Red Hat-based tools using [yum](https://en.wikipedia.org/wiki/Yum_(software)):

```shell
yum install acl
```