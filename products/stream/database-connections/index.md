# Database Connections


Database Connections define the database management systems used by [Database Collectors](collectors-database) to retrieve events.

## Accessing Database Connections {#access}

To access Database Connections, select **Worker Groups** from the sidebar, and choose a Worker Group.
Then, on the **Worker Groups** submenu, select **Processing**, then **Knowledge**, then **Database Connections**.

Select **Add Database Connection** to open a modal that provides the following controls.

## Initial Settings

**ID**: Enter a unique ID for this connection. (A descriptive ID will help all users recognize the connection by purpose.)

**Database type**: Select `MySQL`, `SQL Server`, `Postgres`, or `Oracle` from the drop-down.

- Oracle supports database version 12.1 and newer. Queries can retrieve up to [1 GB of large objects](https://node-oracledb.readthedocs.io/en/latest/user_guide/lob_data.html#querying-lobs) (CLOBS, LOBS, and NCLOBS) per field and use the `node-oracledb` library in [Thin mode](https://node-oracledb.readthedocs.io/en/latest/user_guide/appendix_a.html).

> The **Database type** that you select exposes slightly different configuration fields and compatible values, listed below.
>
{.box .info}

![New Database Connection modal](st-db-connection-modal-4.4.png)
{border="true"}

## Authentication Settings

Set the **Authentication method** to one of the following options.

**Connection String**: Exposes a field where you directly enter your database connection string. The string's format varies according to your selected **Database type**.

> Connection strings should be properly escaped, especially when your password might contain a special character. For example, characters like `@`, `:`, `/`, `?`, `#`, or `%` may be interpreted as delimiters inside a URI. If they aren’t escaped, the connection string can break or fail to authenticate. We recommend using percent-encoding (URL encoding) for these characters. For example, a password of `MyP@ssw0rd!` would be encoded into `MyP%40ssw0rd%21`.  
  {.box .info}

- MySQL format: `mysql[s]://[[user][:password@]][host][:port][/db][?option=value]`

- SQL Server format: `Server=localhost; Database=test;User Id=SA; Password=Testing123[; Option=Value]`
  - To use a non-standard port, set your Server value as follows: `Server=localhost,1431`, where 1431 is the requested port.

  > SQL Server using an Azure AD Service Principal requires a JSON configuration object instead of a simple connection string. See the [Service Principal example](#serviceprincipal) for details.
  {.box .success}


- Postgres format: `postgres[ql]://[userspec@][hostspec][/dbname][?paramspec]`

- Oracle format: Easy Connect or Connection Description format: 
  - Easy Connect: `<host_name>/<service_name>`
  - Connection Description: `(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=your_host_name)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=your_service_name)))`
  - Enter the **User** (username) and **Password** for your Oracle database account. This account must have the necessary permissions to perform the desired operations.

> Cribl Stream requires Oracle database connections to use inline connection strings instead of external `tnsnames.ora` files. The inline connection string follows a specific format, using `FIELD=VALUE` pairs to specify details like the server, port, and service name. If you encounter connection errors, double-check the formatting and ensure all necessary fields are included. See [Oracle's documentation](https://node-oracledb.readthedocs.io/en/latest/user_guide/connection_handling.html#connectionstrings) for details.
>
{.box .warning}

**Connection String Secret**: Exposes a **Connection string secret** drop-down, where you select an existing [stored text secret](securing-secrets) that provides your connection string in the appropriate format listed above. A **Create** button is available to define a new secret.

**Stored Credentials**: When **Database type** is set to `Oracle`, you can use [stored secrets](securing-secrets) for both your database login and connection details. Select a username and password secret for the **Credentials secret** field and a text secret for the **Connection string secret** field to ensure secure, centralized management.

**Config**: When **Database type** is set to `SQL Server`, exposes a pane where you can enter a JSON connection configuration object. The object uses the [Tedious](https://www.npmjs.com/package/tedious) driver and the [node-mssql](https://tediousjs.github.io/node-mssql/) client.

## Additional Settings

**Connection timeout (ms)**: How long, in milliseconds, Cribl Stream should wait before assuming that a connection has failed. This defaults to `10000` (10 sec.) for MySQL databases, and to `15000` (15 sec.) for SQL Server databases. For both, the minimum allowed value is `1000` (1 sec.), and the  maximum is `60000` (1 minute).

**Request timeout (ms)**: This setting appears only where **Database type** is set to `SQL Server`. Defines how long Cribl Stream should wait before assuming that a request has failed. If a query requires a longer timeout than the default `15000` (15 seconds), be sure that you know why – long-running queries can affect other parts of the system. Minimum is `1000` (1 second); maximum is `600000` (10 minutes).

## Testing the Connection

**Leader Node**: Select **Test Connection** at the modal's lower left. This initiates the connection from the Leader Node, and requires that the Leader Node can access the database. Connection test failures are logged in the config helper log for the respective Worker Group on the Leader.

**Worker Node**: First ensure the Database Connection is committed and deployed, then [teleport](setting-up-leader-and-worker-nodes#teleporting) to the Worker and perform a **Test Connection**. Connection test failures are logged in the logs for the Worker Group.

## Examples

To adapt the following examples to your deployment, substitute your own credentials, domain names, etc, for those in the examples.

### Connecting to a MySQL Database

* Database type: `MySQL`
* Connection String: `mysqls://userName:pa$$w0rd@server:3306/dbName`

### Connecting to an Oracle Database

* Database type: `Oracle`
* Connection String:
  * Easy Connect: `my-oracle-server.example.com/XEPDB1`
  * OracleNet Connection (Named Pair): `(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=my-oracle-server.example.com)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XEPDB1)))`

### Connecting to SQL Server with a Service Principal {#serviceprincipal}

#### Authentication Steps

Before you begin, you must have an application registered in Microsoft Entra ID and have the following three values available from your Azure Portal:

* **Tenant ID**: The Directory (tenant) ID for your Azure instance.
* **Client ID**: The Application (client) ID for your app registration.
* **Client Secret**: The secret value you generated for your Service Principal.

When configuring your Database Connection:
1.  Set the **Database type** to `SQL Server`.
1.  Set **Authentication method** to `Config`. You will provide a complete **Configuration Object** that contains all connection and authentication details.
1.  In the configuration object, set the `authentication.type` to `azure-active-directory-service-principal-secret` and provide your credentials within the `authentication.options` block. For example,

Replace all placeholder values (indicated by `<...>` brackets) with your specific server details and credentials.

```json
{
  "server":  "<your-server-name.database.windows.net>",
  "database": "<your-database-name>",
  "port": 1433,
  "authentication": {
    "type": "azure-active-directory-service-principal-secret",
    "options": {
      "tenantId": "<your-tenant-id>",
      "clientId": "<your-application-id>",
      "clientSecret": "<your-client-secret-value>"
    }
  },
  "options": {
    "encrypt": true,
    "trustServerCertificate": false
  }
}
```

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
  "port": 1433,
  "server": "server",
  "database": "database"
}
```
