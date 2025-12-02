# Cribl Edge 4.3.1


## New Features 

This release provides the following improvements:

As of October 16th, 2023, the Windows Cribl Edge 4.3.1. installer is now available for download. To upgrade, you must first uninstall the existing version of Cribl Edge. For details, see [Upgrading Windows Edge Nodes](/edge/upgrading#win-upgrade). 

Fleet management indicates which Edge Nodes are and are not 
eligible for upgrade. 

Amazon SNS and Slack are now available as [Notification targets](notifications-targets). 

The SNMP Source now supports SNMPv3 for user authentication and data privacy. 

The SNMP Trap Source now supports multiple users via SNMPv3, allowing you to
receive traps from multiple devices configured with different users. 

## Corrections 

This release includes the following fixes:

We fixed an issue that prevented Edge Nodes from upgrading to 4.3.0
when the Leader was upgraded first. [CRIBL-20067]

## Documentation Updates

We're pleased to showcase some new documentation updates, including updates to the
left sidebar and landing page. Here's what to look for:

- The Edge Docs landing page contains new topics, including helpful resources and
  more topics in the **Getting started with** section.
- **Known Issues** now has its own link in the top navigation.

Left sidebar changes:

- **Monitoring** is now its own top-level folder. It used to be a child of
  **Administering**.
- We've renamed **Knowledge** to **Knowledge Libraries**.
- **Tips and Tricks** is now **Using Integrations**, and this header contains a new
  sub-category called "Integrating with Other Services".



