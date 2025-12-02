# Fleet Inheritance


Manage Fleet configuration with inheritance.

---

Inheritance simplifies and speeds up Fleet setup by allowing configurations to automatically propagate to Subfleets.

When creating a new Fleet, you can indicate a parent that the new Fleet will inherit configurations from.
Your new Fleet will become a Subfleet of the parent.
Whenever you modify configurations in the parent, its Subfleets will receive the same changes.

To take advantage of inheritance, you can organize your Fleets and Subfleets into a hierarchy of configuration layers.
Layering the configurations simplifies managing a large number of Edge Nodes that share common yet differentiated configurations.
To learn more about planning Fleets and Subfleets, see [Design Fleet Hierarchy](fleets-design).

## How Inheritance Works

When you save and commit a configuration change in a Fleet,
its Subfleets (and their Subfleets, that is, all its descendants)
will receive the same configuration change.

You can view the configuration changes that can be inherited in the **Git Changes** modal before committing them.
This view represents the files that contain configuration changes.
See [Configuration files](configuration-files) for more details about those files.

> When you make changes to a parent Fleet configuration, use the **Commit and Deploy with Subfleets** option.
> This way, you are always keeping the inherited configurations in sync.
{.box .success}

### Overriding Inheritance

If you change a configuration setting in a Subfleet, the inheritance link with the parent Fleet breaks.
Any further changes to the modified setting in the parent will no longer propagate to the Subfleet
and its further descendants.

To revert broken inheritance (that means, to allow a Subfleet to inherit again from its parent after inheritance has been broken),
manually reset the values for a given configuration key to the same state as the parent.

> Consult the **Git Changes** modal after resetting to make sure no unintended Subfleet-level configuration is left.
{.box .success}

Sources, Destinations, Packs, Knowledge Objects, and other objects, are inherited as a whole.
If you modify the configuration for an object in a Subfleet,
this object will no longer inherit further changes, even in other settings.

To revert a configuration change to an object, you can delete this object in the Subfleet.
The Subfleet will then inherit the configuration from its parent again.

### Removing Inherited Sources or Destinations

If you delete an inherited Source or Destination from the Subfleet,
the Subfleet will inherit it again from its parent Fleet.
If you want to ensure a Source or Destination does not propagate again to the Subfleet,
**disable** it instead of deleting.

## Inherited Configurations

Subfleets inherit the following Fleet-level configurations:

- Fleet Settings
- Sources and Destinations
- Routes and Pipelines
- Packs (including local modifications to Packs installed from an external source)
- Knowledge Objects
- Data samples
- Certificates and shared Secrets

> Settings configured on the **Overview** tab of **Fleet Settings**,
> such as **Description**, **Tags**, and **Time to keep disconnected Nodes**,
> are not inherited by Subfleets.
{.box .info}

### Certificate Inheritance

Certificates, configured in **Fleet Settings** > **Security** > **Certificates**,
are inherited by Subfleets alongside other Fleet settings.
They are stored locally on each Edge Node in the `local/edge/auth/certs/` directory.

For more information about certificates, see [Secure Sources and Destinations with Certificates](securing-sources-dest).
