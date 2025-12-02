# How to Secure the Auth Token for the Leader Node


For new installations, the Cribl Stream Leader generates its own secure, random
auth token. The Leader also creates a new token when you switch from
Single-instance to the Distributed mode by configuring a Node to be a Leader Node.

> In versions before v4.5.0, the default token was `criblmaster`. This
> default auth token does not change automatically if you upgrade your deployment to v4.5.0 or
> later. 
{.box .info}

If you need to change your auth token (for example, you prefer to use your own token or you are still using the older default of `criblmaster`),
change the token value to a unique secure value.

This applies whether you configure your Leader via the UI, or via a Docker Compose file.

> **High Availability (Failover) Mode**
> 
> Beginning in Cribl Stream 4.7, the auth token information that is stored in the [`leader.yml`](leaderyml) file on the failover volume will take precedence over the auth token stored in the local `instance.yml` file. This way, a single token is shared between primary and any standby Leaders.
>  
> Before 4.7, there is no `leader.yml` file; instead, the auth token is in the  `instance.yml` file only. 
> Whenever you change an auth token, you must update it to be identical on all Leaders. See [Configuring Additional Leader Nodes](/stream/deploy-add-second-leader#configure) in the failover topic.
> 
{.box .success}

### Change the Auth Token Value

> Changing the auth token once you have Worker Nodes deployed will break communication between the Nodes and the Leader.
> If you do change the auth token value, make sure that you update the auth tokens for the Leader and the 
> Worker Nodes to the same value.
{.box .warning}

Create a unique token value with strong security. We recommend creating a string
that contains at least 14 characters and includes uppercase letters, lowercase
letters, and numbers.

#### Allowed Characters in the Leader Auth Token

Starting in version 4.6.1, these are the only allowed characters you can use in the auth token for your Cribl Stream Leader:

- a-z: Lowercase letters from a to z. 
- A-Z: Uppercase letters from A to Z.
- 0-9: Digits from 0 to 9. 
- `_`: Underscore. 
- `!`: Exclamation mark.
- `-`: Hyphen (dash).

In version 4.5.1 and newer, you _can't_ use these special characters in the Leader auth token:

- `<`: Open angle bracket.
- `>`: Close angle bracket.
- `"`: Double quotation mark.
- `{`: Open brace.
- `}`: Close brace.
- `|`: Pipe.
- `\`: Backslash.
- `^`: Caret.
- `'`: Single quotation mark.
- `` ` ``: Backtick.
- Whitespace


One option is to generate a token value with a shell command. For example, on Linux or Mac OS, the following command generates a 32-character value:
  ```shell
  head /dev/urandom | LC_ALL=C tr -dc A-Za-z0-9 | head -c32 | cut -c 1-
  ```

Once you have created a secure auth token value, you can change it:

- In the **Auth token** field in the UI.
- In the appropriate place in your Docker Compose file (if applicable).
- For the Leader via command line: `./cribl mode-master -u <token>`. If your Cribl Stream deployment
  uses High Availability (HA), you must use the command line (not the UI) with the
  command listed above.
 - For the Worker Nodes via command line: 
   - [`./cribl mode-managed-edge -u <token>`](cli-mode-managed-edge) for Cribl Edge.
   - [`./cribl mode-worker -u <token>`](cli-mode-worker) for Cribl Stream.

     > Running either of the commands resets all the related options.
     > This means that, in addition to the token, you must provide the command with the current values of all the other arguments.
     {.box .info}

You can find the **Auth token** setting by navigating to **Settings** > 
**Global** > **System** > **Distributed Settings** > **Leader Settings**.

