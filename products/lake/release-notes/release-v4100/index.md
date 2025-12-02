# Cribl Lake 4.10.0


## New Features

This release provides the following improvements:

### Dataset Deletion Handling

Cribl Lake now handles deleting datasets in a more graceful way.
When you mark a Dataset as ready for deletion, you will see a "Deletion in progress" tag next to it.
The data will be removed gradually, with data older than 24 hours disappearing at midnight UTC the following day
and more recent data being deleted after that time.

Once a Dataset is marked for deletion, you are no longer charged for any residual data that it contains.

### Cribl Lake Logs

You can now view Cribl Lake-related logs directly from the sidebar:
select **Logs** and use the drop-down to narrow down the type of log you want to see.

### Experience Improvements

To reflect the range of scenarios beyond username/password authentication,
the login failure message for the UI now reads "Authentication unsuccessful. Please try again."
