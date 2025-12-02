# Backup & Rollback via the UI


Transporting an Edge installation into the future. 

---

## Backup and Rollback

When you initiate an upgrade through the UI, Edge first stores a backup of your
current stable deployment. If the upgrade fails, then by default, Edge will
automatically roll back to the stored backup package. You can adjust this
behavior at **Settings** > **Global Settings** > **System** >
**General Settings** > **Upgrade & Share Settings**, using the following
controls. 

> Edge can perform rollbacks only on Nodes that were running at least
> v.3.0.0 before the attempted upgrade. 
>
{.box .warning}

**Enable automatic rollback**: Edge will automatically roll back an upgrade if
the Edge server fails to start, or if the Edge Node fails to connect to the
Leader. (Toggle to `No` to defeat this behavior.)

**Rollback timeout (ms)**: Time to wait, after an upgrade, before checking each
Node's health to determine whether to roll back. Defaults to `30000`
milliseconds, i.e., 30 seconds.

**Rollback condition retries**: Number of times to retry the health check before
performing a rollback. Defaults to `5` attempts.

**Check interval (ms)**: Time to wait between health-check retries. Defaults to
`1000` milliseconds, i.e., 1 second.

**Backups directory**: Specify where to store backups. Defaults to
`$CRIBL_HOME/state/backups`.

**Backup persistence**: A relative time expression specifying how long to keep
backups after each upgrade. Defaults to `24h`.
