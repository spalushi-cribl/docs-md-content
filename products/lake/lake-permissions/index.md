# Cribl Lake Permissions


Manage access to Cribl Lake by assigning product-level Permissions.

---

You can manage access to Lake Datasets by assigning specific product-level Permissions
within each [Workspace](/stream/workspaces/) to [Members](/stream/members/) and [Teams](/stream/teams/).

You can assign the following Permissions at the Cribl Lake product level.

| Permission                                              | Read Only | Editor | Admin |
| ------------------------------------------------------- | --------- | ------ | ----- |
| Read Lake Datasets                                           | ✓         | ✓      | ✓     |
| Create and edit Lake Datasets                              |           | ✓      | ✓     |
| Delete Lake Datasets                                         |           |        | ✓     |
| Read [Lakehouses](lakehouse)                            | ✓         | ✓      | ✓     |
| Create and delete [Lakehouses](lakehouse) |           |        | ✓     |
| Assign and unassign Lake Datasets to [Lakehouses](lakehouse) |           |        | ✓     |

Some Cribl Lake capabilities require certain product-level Permissions set in other products from the Cribl Product Suite:

| Capability | Permission |
| ---------- | ------------------------ |
| View the **Integrated with** column in the Dataset table, listing the [Cribl Lake Collectors](collectors-cribl-lake) and [Destinations](destinations-cribl-lake) that a Lake Dataset is connected with. | [Read Only, Editor, or Admin](/stream/permissions#product/) Permission for Cribl Stream. |
| Search a Lake Dataset directly from the Dataset table, the Member/Team needs [Cribl Search access](/search/sharing#share-datasets-and-dataset-providers/). | [User, Editor, or Admin](/search/cloud-managing#search-member-roles/) Permission for Cribl Search, and Read Only or higher [Permission for this Lake Dataset](/search/sharing#share-datasets-and-dataset-providers/). |

