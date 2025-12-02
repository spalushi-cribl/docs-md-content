# Leader's Auth Token


For new installations, Cribl Edge generates a secure, random Leader's auth token.
A new token is also created when you switch from Single-instance to the Distributed mode
by configuring a node to be a Leader mode.

> In versions before v4.5.0, the default token was `criblmaster`.
> It is not changed if you upgrade your deployment to v4.5.0 or later.
{.box .info}

If you prefer to use your own token (or you are still using the older default of `criblmaster`),
change the token value to a unique secure value.

This applies whether you configure your Leader via the UI, or via a Docker Compose file.

### Changing the Auth Token Value

> Changing the auth token once you have Edge Nodes deployed will break communication between the nodes and the Leader.
> 
> We recommend changing the auth token *before* deploying Edge Nodes.
{.box .warning}

Create a unique, strongly secure token value. Cribl recommends creating a string that contains at least 14 characters and includes uppercase letters, lowercase letters, and numbers.

One option is to generate a token value with a shell command. For example, on Linux or Mac OS, the following command generates a 32-character value:
  ```shell
  head /dev/urandom | LC_ALL=C tr -dc A-Za-z0-9 | head -c32 | cut -c 1-
  ```

Once you have created a secure auth token value, you can change it:

- in the **Auth token** field in the UI
- in the appropriate place in your Docker Compose file (if applicable)
- via command line: `./cribl mode-master -u <token>`

In the UI, you'll find the **Auth token** setting under **Global Settings** > **System** > **Distributed Settings** > **Leader Settings**, as shown in the screenshot below.

![Distributed > Leader Settings](leader-settings.eb34f1c547.png)
{border="true"}
