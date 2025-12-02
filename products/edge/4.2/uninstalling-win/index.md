# Uninstalling Cribl Edge from Windows


You can uninstall Cribl Edge – whether for a clean reinstall or permanently – either via the [Windows UI](#ui) or via your [command prompt](#msi). 

### Using the Windows UI {#ui}

To uninstall Cribl Edge using Windows' graphical UI:

1. On your Windows Server, search for **Add or remove programs**. 

2. In the **Apps & features** list, scroll to **Cribl Edge**, and click **Uninstall**. 

3. In the confirmation dialog, click **Yes**. 

A success message will confirm removal. 

> This uninstall process does not automatically delete the `local`, `log`, `pid`, and `state` directories, because those folders include contents generated after the original installation completed. To completely remove the  application, you must explicitly delete the folders from the filesystem.
>
{.box .info}

### Using the Command Prompt

If you installed Cribl Edge using the [Msi Installer](deploy-windows#msi), you can uninstall it by running the following at a command prompt:

```shell
    - msiexec /x cribl-<version>-<build>-<arch>.msi
```
