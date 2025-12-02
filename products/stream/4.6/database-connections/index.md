# Database Connections


You configure Database Connections to define DBMS resources from which the [Database Collector](collectors-database) can retrieve events.

## Accessing Database Connections {#access}

You access Database Connections from Cribl Stream's top nav under **Processing** > **Knowledge** > **Database Connections**. Select **New Database Connection** to open a modal that provides the following controls.

## Initial Settings

**ID**: Enter a unique ID for this connection. (A descriptive ID will help all users recognize the connection by purpose.)

**Database type**: Select from the drop-down options. The currently supported options are `MySQL`, `SQL Server`, and `Postgres`.

> The **Database type** that you select exposes slightly different configuration fields and compatible values, listed below.
>
{.box .info}

![New Database Connection modal](st-db-connection-modal-4.4.png)
{border="true"}

## Authentication Settings

Set the **Authentication method** to one of the following options.

**Connection String**: Exposes a field where you directly enter your database connection string. The string's format varies according to your selected **Database type**.

- MySQL format: `mysql[s]://[[user][:password@]][host][:port][/db][?option=value]`

- SQL Server format: `Server=localhost; Database=test;User Id=SA; Password=Testing123![; Option=Value]`

- Postgres format: `postgres[ql]://[userspec@][hostspec][/dbname][?paramspec]`

**Secret**: Exposes a **Connection string secret** drop-down, where you select an existing [stored text secret](securing-and-monitoring#secrets) that provides your connection string in the appropriate format listed above. A **Create** button is available to define a new secret.

**Config**: When **Database type** is set to `SQL Server`, exposes a pane where you can enter a JSON connection configuration object. The object uses the [Tedious](https://www.npmjs.com/package/tedious) driver and the [node-mssql](https://tediousjs.github.io/node-mssql/) client.

## Additional Settings

**Connection timeout (ms)**: How long, in milliseconds, Cribl Stream should wait before assuming that a connection has failed. This defaults to `10000` (10 sec.) for MySQL databases, and to `15000` (15 sec.) for SQL Server databases. For both, the minimum allowed value is `1000` (1 sec.), and the  maximum is `60000` (1 minute).

**Request timeout (ms)**: This setting appears only where **Database type** is set to `SQL Server`. Defines how long Cribl Stream should wait before assuming that a request has failed. If a query requires a longer timeout than the default `15000` (15 seconds), be sure that you know why – long-running queries can affect other parts of the system. Minimum is `1000` (1 second); maximum is `600000` (10 minutes).

## Testing the Connection

**Leader Node**: Select **Test Connection** at the modal's lower left. This initiates the connection from the Leader Node, and requires that the Leader Node can access the database. Connection test failures are logged in the config helper log for the respective Worker Group on the Leader.

**Worker Node**: First ensure the Database Connection is committed and deployed, then [teleport](deploy-distributed#worker-access) to the Worker and perform a **Test Connection**. Connection test failures are logged in the logs for the Worker Group.

## Examples

To adapt the following examples to your deployment, substitute your own credentials, domain names, etc, for those in the examples.

### Connecting to a MySQL Database

* Database type: `MySQL`
* Connection String: `mysqls://userName:pa$$w0rd@server:3306/dbName`

### Using Active Directory to Connect to SQL Server on Azure

* Database type: `SQL Server`
* Connection String: `Server=tcp:server.database.windows.net,1433;User ID=userName@domain.onmicrosoft.com;Password=pa$$w0rd;Encrypt=True;TrustServerCertificate=False;Authentication="Active Directory Password";Database="dbName"`

### Using NTLM to Connect to SQL Server

[Windows Challenge/Response](https://learn.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm), also known as Microsoft New Technology LAN Manager (NTLM), is an authentication protocol for systems running the Windows operating system.

* Database type: `SQL Server`
* Configuration Object: 
```
{
  "authentication": {
    "type": "ntlm",
    "options": {
      "userName": "userName",
      "password": "pa$$w0rd",
      "domain": "domain"
    }
  },
  "options": {
    "connectTimeout": 15000,
    "trustServerCertificate": true
  },
  "connectionTimeout": 15000,
  "port": 1433,
  "server": "server",
  "database": "database"
}
```