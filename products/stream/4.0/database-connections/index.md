# Database Connections


You configure Database Connections to define DBMS resources from which the [Database Collector](collectors-database) can retrieve events.

## Accessing Database Connections {#access}

You access Database Connections from Cribl Stream's top nav under **Processing** > **Knowledge** > **Database Connections**. Select **New Database Connection** to open a modal that provides the following controls.

## Initial Settings

**ID**: Enter a unique ID for this connection. (A descriptive ID will help all users recognize the connection by purpose.)

**Database type**: Select from the drop-down options. The currently supported options are `MySQL` or `SQL Server`.

> The **Database type** that you select exposes slightly different configuration fields and compatible values, listed below.
>
{.box .info}

![New Database Connection modal](st-db-connection-modal.34c2bd39ae.png)
{border="true"}

## Authentication Settings

Set the **Authentication method** to one of the following buttons.

**Connection string**: Exposes a field where you directly enter your database connection string. The string's format varies according to your selected **Database type**.

- MySQL format: `mysql[s]://[[user][:password@]][host][:port][/db][?option=value]`

- SQL Server format: `Server=localhost; Database=test;User Id=SA; Password=Testing123![; Option=Value]`

**Secret**: Exposes a **Connection-string secret** drop-down, where you select an existing [stored text secret](securing-and-monitoring#secrets) that provides your connection string in the appropriate format listed above. A **Create** button is available to define a new secret.

## Additional Settings

**Connection timeout (ms)**: How long, in milliseconds, Cribl Stream should wait before assuming that a connection has failed. This defaults to `10000` (10 sec.) for MySQL databases, and to `15000` (15 sec.) for SQL Server databases. For both, the minimum allowed value is `1000` (1 sec.), and the  maximum is `60000` (1 minute).

**Request timeout (ms)**: This setting appears only where **Database type** is set to `SQL Server`. Defines how long, in milliseconds, Cribl Stream should wait before assuming a request has failed. Minimum `1000` (1 sec.); default `15000` (15 sec.); maximum `60000` (1 minute).

## Testing the Connection

To test database configuration, click **Test Connection** at the modal's lower left.
This will perform the test from the Leader Node.
To test a connection from a Worker Node, [teleport](deploy-distributed#worker-access) to the Worker first.
